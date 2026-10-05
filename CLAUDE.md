# CLAUDE.md — VCE (Validity Control Engine) for MailSender Pro

MailSender Pro MySQL veritabanı için Apache Airflow tabanlı veri kalitesi sistemi. Temel ilke: **"Kurallar kodda değil, veritabanında yaşar"** — yeni kontrol = `vce.vce_dq_rules`'a bir `INSERT`; DAG/kod değişmez. Eşik (threshold) ve anomali kuralları, tablo karşılaştırma, temizlik (remediation), kolon istatistikleri, dağılım kontrolleri, anomali geri bildirimi / concept drift, data product kalite skoru ve SLA izleme.

- GitHub: https://github.com/SHapeloglu/ValidityControlEngine (2026-04-12 → 04-29)
- Hedef veri: **MailSenderVerifier** (`aws_mailsender_pro_v3` şeması). Bu sunucuda Airflow kurulu değil; geliştirme Windows + Docker Airflow + host MySQL üzerinde.
- Mimari: `architect.md` · Görevler: `task.md` · Fikirler: `backlog.md` · Günlük: `session.md`

## Komutlar

```bash
# Şema (sırayla, host MySQL'de)
mysql -u root -p vce < sql/01_vce_schema.sql        # 7 tablo, 3 trigger, aylık partition
mysql -u root -p vce < sql/02_vce_seed_rules.sql    # 35 hazır kural
mysql -u root -p vce < sql/03_vce_extensions.sql    # GE/Soda ilhamlı ekler
mysql -u root -p vce < sql/04_vce_ml_lifecycle.sql  # ML lifecycle + data product

# Kural SQL'ini canlıya koymadan dene
python tools/test_rule.py --sql "SELECT COUNT(*) FROM aws_mailsender_pro_v3.send_log WHERE status='failed'" --type threshold
python tools/test_rule.py --rule-id 15 | --all

# Testler (apache-airflow kurulu olmalı; yoksa 51 testin hepsi ModuleNotFoundError verir)
pip install apache-airflow -r tests/requirements-test.txt && pytest
black --check . && isort --check . && flake8      # CI ile aynı kontroller

# Windows'ta Airflow klasörüne kopyalama
.\tools\deploy_local.ps1 [-OnlyOperators|-OnlyDAGs|-DryRun]
```

## Kurallar ve Tuzaklar

- **Operatörler iki yerde birebir kopya:** `operators/` (test ve coverage kaynağı, `known_first_party`) ve `dags/operators/` (Airflow'un `dags/` klasöründen import ettiği kopya). Birini değiştirirsen diğerine kopyala — ya da deploy betiğiyle senkronla.
- **İki bağlantı, iki şema:** `vce` conn (vce.* okuma/yazma) ve `mailsender` conn (`aws_mailsender_pro_v3` SELECT + sadece remediation DELETE). Kural SQL'lerinde tabloyu **şema önekiyle** yaz (`aws_mailsender_pro_v3.send_log`); testler bunu kontrol ediyor.
- Kural değişiklikleri `vce_rule_audit_log`'a trigger ile yazılır — kuralları doğrudan UPDATE ile değiştirmek denetim izini korur, DELETE yerine `is_active=0` tercih et.
- Execution/örnek/istatistik tabloları aylık partition'lı; yeni partition'ları `mailsender_vce_partition_manager` DAG'ı açar. Elle `ALTER TABLE … PARTITION` yapmadan önce o DAG'ı oku.
- Black/isort satır uzunluğu 120; Python 3.9 uyumluluğu (`target-version py39`).
- Oturum sonunda `session.md`'ye kayıt düş, `task.md`'yi güncelle.
