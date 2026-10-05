# architect.md — VCE — Validity Control Engine for MailSender Pro Mimari Referansı

Bu dosya projenin yapısının hızlı-referans özetidir. Kod değiştikçe güncel tutun.

## Genel Bakış

**Apache Airflow + MySQL tabanlı, kural yönetimi veritabanında olan, üretim seviyesi veri kalitesi sistemi.** ---

## Teknoloji Yığını

- MySQL (PyMySQL)
- requests
- pytest
- SQL betikleri
- Python

## Dizin Yapısı

```
.github/
  workflows/
.sqlfluff
README.md
dags/
  mailsender_vce_main.py
  mailsender_vce_ml_lifecycle.py
  mailsender_vce_partition_manager.py
  mailsender_vce_remediation.py
  operators/
dashboard/
  vce_dashboard.html
operators/
  vce_operators.py
  vce_operators_extended.py
  vce_operators_ml_lifecycle.py
pyproject.toml
sql/
  01_vce_schema.sql
  02_vce_seed_rules.sql
  03_vce_extensions.sql
  04_vce_ml_lifecycle.sql
tests/
  conftest.py
  requirements-test.txt
  test_vce_operators.py
tools/
  deploy_local.ps1
  test_rule.py
```

## Modüller / Kaynak Dosyalar

- `dags/mailsender_vce_main.py` — mailsender_vce_main.py
- `dags/mailsender_vce_ml_lifecycle.py` — mailsender_vce_ml_lifecycle.py
- `dags/mailsender_vce_partition_manager.py` — mailsender_vce_partition_manager.py
- `dags/mailsender_vce_remediation.py` — mailsender_vce_remediation.py
- `operators/vce_operators.py` — vce_operators.py
- `operators/vce_operators_extended.py` — vce_operators_extended.py
- `operators/vce_operators_ml_lifecycle.py` — vce_operators_ml_lifecycle.py
- `dags/operators/vce_operators.py` — vce_operators.py
- `dags/operators/vce_operators_extended.py` — vce_operators_extended.py
- `dags/operators/vce_operators_ml_lifecycle.py` — vce_operators_ml_lifecycle.py

## Giriş Noktaları ve Yapılandırma

_(belirgin giriş noktası bulunamadı)_

## Dağıtım / Çalışma Ortamı

- GitHub: https://github.com/SHapeloglu/ValidityControlEngine

## Diğer Dokümanlar

- `README.md`

## Mimari Kararlar

_Önemli tasarım kararlarını ve gerekçelerini buraya ekleyin (ör. "X yerine Y seçildi çünkü ...")._
