# CLAUDE.md — TBNG SCADA Next Mobile

Android uygulaması: `https://scada2.tbngltd.com` sitesini (repo: sbesiktepe/tbng-scada-next) tam ekran
açan bir **Trusted Web Activity** (paket `com.tbngltd.scada`). Uygulamanın kendi ekranı yoktur; tüm
arayüz web tarafındadır, site güncellenince uygulama da güncellenir. Kullanıcıyla Türkçe konuşulur,
ürün (metinler, kod yorumları) İngilizce.

- Derleme yalnızca GitHub Actions'ta (`.github/workflows/android.yml`): APK + AAB + imza parmak izi.
- İmza anahtarı GitHub secret'larında (`ANDROID_KEYSTORE_BASE64`, `ANDROID_KEYSTORE_PASSWORD`,
  `ANDROID_KEY_ALIAS`, `ANDROID_KEY_PASSWORD`). Anahtar, parola veya `.jks` asla repoya girmez.
- Site doğrulaması (adres çubuğunun kalkması) ana repodaki API'de: `/.well-known/assetlinks.json`,
  sunucu `.env` → `ANDROID_CERT_SHA256`.
- Paket adı ve imza anahtarı Play Store'a çıktıktan sonra değiştirilemez.
- Ayrıntılar: README.md.
