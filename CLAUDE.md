# CLAUDE.md

Bu dosya, bu proje üzerinde çalışırken Claude'un (Claude Code dahil) izlemesi gereken bağlamı ve kuralları içerir.

## Proje

**VCE — Validity Control Engine for MailSender Pro** — **Apache Airflow + MySQL tabanlı, kural yönetimi veritabanında olan, üretim seviyesi veri kalitesi sistemi.** ---

- GitHub: https://github.com/SHapeloglu/ValidityControlEngine

## Teknoloji Yığını

- MySQL (PyMySQL)
- requests
- pytest
- SQL betikleri
- Python

## Önemli Dosyalar

_(belirgin giriş noktası bulunamadı)_

Mimari ayrıntılar için bkz. `architect.md`.

## Sık Kullanılan Komutlar

```bash
pytest
```

## Kurallar

- `.env`, parola, token ve API anahtarlarını asla commit etme.
- Her çalışma oturumunun sonunda `session.md`ye kısa kayıt düş; görev durumunu `task.md`de güncelle.
- Önceliklendirilmemiş fikirleri `backlog.md`ye yaz; somutlaşınca `task.md`ye taşı.

## Çalışma Dosyaları

| Dosya | Amaç |
|---|---|
| `architect.md` | Mimari ve dizin yapısı referansı |
| `task.md` | Aktif / devam eden / tamamlanan görevler |
| `backlog.md` | Önceliklendirilmemiş fikir ve teknik borç havuzu |
| `session.md` | Oturum günlüğü — her oturum sonunda güncellenir |
