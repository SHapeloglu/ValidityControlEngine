# architect.md — VCE Mimarisi

```
Airflow Scheduler
 ├─ mailsender_vce_main            06:00 UTC her gün   → DataQualityOperator, TableValidationOperator, ColumnStats, Distribution
 ├─ mailsender_vce_remediation     03:00 UTC her gün   → RemediationOperator
 ├─ mailsender_vce_ml_lifecycle    07:00 UTC her gün   → ConceptDrift, ModelPerformance, QualityScore, SLAMonitor
 ├─ mailsender_vce_weekly_report   Pazartesi 07:00 UTC → DataProductReportOperator
 └─ mailsender_vce_partition_manager  ayın 1'i 01:00   → aylık partition aç/temizle
        │
        ├─ conn "vce"        ─► MySQL şema vce (kurallar, sonuçlar, baseline, skorlar)
        └─ conn "mailsender" ─► MySQL şema aws_mailsender_pro_v3 (kural SQL'leri burada çalışır)
dashboard/vce_dashboard.html ─► vce tablolarındaki sonuçların statik HTML görünümü
```

## Operatörler

| Dosya | Sınıflar | Görev |
|---|---|---|
| `vce_operators.py` | `VCEBaseOperator` | İki bağlantı, loglama, bildirim altyapısı |
| | `DataQualityOperator` | `vce_dq_rules`'tan aktif kuralları yükle → SQL çalıştır → `vce_dq_executions`'a yaz → anomali baseline güncelle |
| | `TableValidationOperator` | `vce_table_validations` tanımlı iki sorgu sonucunu karşılaştır |
| | `RemediationOperator` | Tanımlı temizlik SQL'leri (sınırlı DELETE), `vce_remediation_log` |
| `vce_operators_extended.py` | `ColumnStatsOperator`, `DistributionCheckOperator`, `FailedRowsSamplingMixin`, `get_column_trend`, `get_distribution_history` | Kolon profili, dağılım sapması, ihlal eden satır örnekleri |
| `vce_operators_ml_lifecycle.py` | `ConceptDriftOperator`, `ModelPerformanceOperator`, `QualityScoreOperator`, `SLAMonitorOperator`, `DataProductReportOperator` | Anomali modeli geri bildirimi ve drift, günlük kalite skoru, SLA ihlalleri, haftalık data product raporu |

## Şema (vce)

- **01**: `vce_dq_rules`, `vce_dq_executions`*, `vce_table_validations`, `vce_table_val_executions`*, `vce_rule_audit_log` (trigger), `vce_remediation_log`*, `vce_anomaly_baselines`
- **03**: `vce_failed_rows_samples`*, `vce_column_stats`*, `vce_column_stats_config` (10 hazır), `vce_distribution_checks` (4 hazır)
- **04**: `vce_anomaly_feedback`, `vce_concept_drift_log`, `vce_model_performance`, `vce_data_products` (6 hazır), `vce_quality_scores`*, `vce_sla_violations`, `vce_data_product_changelog`

(* aylık partition)

## Kural Tipleri

- **threshold** — SQL tek sayı döner, eşikle karşılaştırılır.
- **anomaly** — sonuç tarihsel baseline'a (ortalama/sapma) göre değerlendirilir; geri bildirimle drift tespiti.
- Kurallar `domain` / `subdomain` ile gruplanır; `pre_sql` desteği var.

## CI/CD

- `ci.yml`: flake8 + black + isort, Airflow test DB init, `pytest`.
- `cd.yml`: deploy paketi artifact'ı (DAG + operatör dosyaları). SSH ile erişilemeyen yerel Airflow için `tools/deploy_local.ps1`.

## Mimari Kararlar

- **Kurallar DB'de** — yeni kontrol için deploy gerekmez; iş analistleri SQL ile kural ekleyebilir.
- **İki bağlantı** — yetki sınırı ve yanlışlıkla prod verisi değiştirmeye karşı koruma.
- **Soda Core / Great Expectations yerine kendi motoru** — MySQL partition + Airflow ile hafif kurulum; GE/Soda'dan seçili fikirler (failed rows, column stats, distribution) uyarlandı (README'de gerekçe).
- **Coca-Cola Amatil VCE mimarisi** esas alındı, MySQL'e uyarlandı.
