# backlog.md — VCE Fikir Havuzu

- Dashboard'u statik HTML'den canlı sayfaya (Power BI / Superset / basit Flask) taşı.
- Bildirim kanalları: Slack / Teams / e-posta (MailSender üzerinden) şablonları.
- Kural yönetimi için küçük web arayüzü (INSERT yerine form + `test_rule.py` entegrasyonu).
- Diğer kaynaklar: PostgreSQL/MSSQL bağlantı soyutlaması (DQ projesi `/opt/dq` ile birleşme fırsatı).
- Kalite skorunu MailSender gönderim kararına bağlama (skor eşiğin altındaysa toplu gönderimi durdur).

## Ekleme Şablonu

```markdown
### Başlık
- **Kategori:** yeni özellik / iyileştirme / teknik borç / araştırma
- **Neden:** kısa gerekçe
- **Notlar:** büyüklük, bağımlılıklar, riskler
```
