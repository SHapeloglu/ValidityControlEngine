# task.md — VCE Görevleri

## 🔜 Sıradaki

- [ ] Operatör kopyalarını tekilleştir: `operators/` ↔ `dags/operators/` (sembolik link, deploy betiğinde kopyalama ya da tek kaynak + `PYTHONPATH`)
- [ ] Testleri Airflow kurulu olmadan da çalıştırılabilir yap (`airflow` importlarını conftest'te stub'la) ya da README'ye "Airflow gerekli" notu düş
- [ ] MailSender güncel şeması (sunucu kopyası `/opt/mailsender` — MSV henüz deploy edilmedi, DB yerelde) ile kural SQL'lerinin uyumunu `tools/test_rule.py --all` ile doğrula — MailSender tablolarında değişiklik oldu mu?

## 🚧 Devam Eden

_(şu anda boş)_

## ✅ Tamamlanan

- [x] 2026-10-05 — Çalışma dosyaları kod okunarak yeniden yazıldı (bu sunucuda Airflow olmadığı için testler çalıştırılamadı: 51 × ModuleNotFoundError)
- [x] 2026-04-29 — Son güncelleme ("vce")
- [x] 2026-04-12 → 04-18 — Şema, kurallar, operatörler, ML lifecycle, CI/CD, dashboard
