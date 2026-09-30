# TBNG SCADA Next - Android

Uygulama bir **Trusted Web Activity (TWA)**: telefondaki Chrome, `https://scada2.tbngltd.com`
sitesini adres çubuğu olmadan, tam ekran bir uygulama gibi açar. Giriş, grafikler, raporlar ve
indirmeler web ile aynıdır; site güncellenince uygulama da güncellenmiş olur (yeni APK gerekmez).

## APK'yı indirmek

En kolayı: **https://github.com/sbesiktepe/tbng-scada-next-mobile/releases/latest** → Assets →
`tbng-scada-N.apk` (telefondan da açılabilir; GitHub'a giriş yapılmış olmalı).

## APK üretmek (GitHub Actions)

1. GitHub → depo → **Actions** → **Android app** → **Run workflow** (dal: çalışılan dal).
2. İş bitince (2-3 dk) dosyalar **Releases** sayfasına da konur; ayrıca çalıştırmanın sayfasının
   en altında **Artifacts → tbng-scada-apk** (zip) olarak durur:
   `tbng-scada-N.apk` (telefona kurulur), `tbng-scada-N.aab` (Play Store'a yüklenir),
   `certificate-sha256.txt` (imza parmak izi).

## İmza anahtarı (bir kez)

Anahtar olmadan da APK üretilir ve kurulur (hata ayıklama anahtarı), ama üstte küçük bir adres
çubuğu görünür ve her derlemede anahtar değişir. Kalıcı anahtar:

```bash
keytool -genkeypair -v -keystore tbng-scada.jks -alias tbng -keyalg RSA -keysize 4096 -validity 10000 \
  -dname "CN=TBNG SCADA, O=TBNG, C=TR"
base64 -w0 tbng-scada.jks > tbng-scada.jks.b64
```

GitHub → Settings → Secrets and variables → Actions → **New repository secret**:

| Secret | Değer |
|---|---|
| `ANDROID_KEYSTORE_BASE64` | `tbng-scada.jks.b64` dosyasının içeriği |
| `ANDROID_KEYSTORE_PASSWORD` | anahtar deposu parolası |
| `ANDROID_KEY_ALIAS` | `tbng` |
| `ANDROID_KEY_PASSWORD` | anahtar parolası |

`tbng-scada.jks` dosyasını ve parolaları güvenli bir yerde saklayın: **kaybolursa aynı uygulama
bir daha güncellenemez.** Repoya asla koymayın.

## Adres çubuğunu kaldırmak (site doğrulaması)

`certificate-sha256.txt` içindeki `SHA256: AB:CD:...` değerini sunucudaki `.env` dosyasına yazın,
API'yi yeniden başlatın:

```bash
ANDROID_CERT_SHA256=AB:CD:...
docker compose --profile public --profile legacy up -d
curl https://scada2.tbngltd.com/.well-known/assetlinks.json
```

Play Store'a yüklenince Google kendi anahtarıyla da imzalar; Play Console → App integrity'deki
"App signing key certificate" SHA-256 değerini de virgülle ekleyin.

## Telefona kurmak (APK)

APK dosyasını telefona gönderin (e-posta, WhatsApp, USB), açın; Android "bilinmeyen kaynaklardan
yükleme" izni ister (yalnızca o uygulama için). Chrome telefonda kurulu ve güncel olmalı.

## Play Store

Google Play Console hesabı (tek sefer 25 $) → yeni uygulama → `tbng-scada-N.aab` dosyası
yüklenir → gizlilik politikası adresi, ekran görüntüleri, kısa açıklama → kapalı test (şirket
içi) veya yayın.
