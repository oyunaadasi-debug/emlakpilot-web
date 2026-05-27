# App Store Hazırlık Paketi

Bu klasör App Store yüklemesi için hazır dosyalar içerir.

## Dosyalar

| Dosya | Amaç |
|---|---|
| `privacy-policy.html` | Gizlilik Politikası (host edilecek) |
| `terms.html` | Kullanım Şartları (host edilecek) |
| `support.html` | Destek sayfası (App Store Connect "Support URL" için) |
| `app-store-metadata.md` | App Store Connect form içeriği (kopya-yapıştır) |

---

## 1) HTML'leri Nasıl Host Ederim? (GitHub Pages — ÜCRETSİZ, 5 dakika)

1. **Yeni GitHub repo oluştur:** `emlakpilot-web` (public)
2. Bu 3 HTML dosyasını repo'nun köküne yükle (`privacy-policy.html`, `terms.html`, `support.html`)
3. GitHub repo → **Settings → Pages**
4. "Branch: main" → "/(root)" → Save
5. 1-2 dakika sonra şu URL'ler aktif olur:
   - `https://<kullanici-adin>.github.io/emlakpilot-web/privacy-policy.html`
   - `https://<kullanici-adin>.github.io/emlakpilot-web/terms.html`
   - `https://<kullanici-adin>.github.io/emlakpilot-web/support.html`

Bu URL'leri App Store Connect form'larında kullan.

---

## 2) App Store Connect'te Uygulama Oluşturma

Apple Developer hesabı onaylandıktan sonra:

1. https://appstoreconnect.apple.com → My Apps → "+"
2. New App seçenekleri:
   - Platform: iOS
   - Name: **EmlakPilot**
   - Primary Language: Turkish
   - Bundle ID: **com.emlakpilot.app** (önce Apple Developer'da identifier oluşturduktan sonra burada görünür)
   - SKU: **emlakpilot-001**
3. Form içeriklerini `app-store-metadata.md`'den kopyala-yapıştır
4. App Privacy bölümü için yine `app-store-metadata.md`'deki "Privacy Nutrition Labels" tablosunu izle

---

## 3) Build Yükleme (EAS)

```bash
# Bir kez:
npm i -g eas-cli
eas login              # Expo hesabıyla
eas init               # app.json'daki projectId otomatik dolar

# iOS production build (App Store):
eas build --platform ios --profile production

# Build tamamlandıktan sonra TestFlight'a otomatik veya:
eas submit --platform ios --profile production
```

İlk submit için `eas.json`'a App Store Connect App ID (ascAppId) doldurulmalı.

---

## 4) Demo Hesap Oluşturma (Reviewer için)

Yüklemeden ÖNCE Supabase'de bu kaydı oluştur:

```bash
node scripts/seed_test_user.mjs
# Ya da SQL Editor'da manuel:
# email: appreview@emlakpilot.com  password: AppReview2026!
```

Sonra bu hesaba bol miktarda örnek ilan/müşteri ekle (bos hesap reject sebebi).

---

## Kontrol Listesi (Yüklemeden önce)

- [ ] GitHub Pages açık, 3 URL erişilebilir
- [ ] Apple Developer hesabı onaylı
- [ ] Bundle ID com.emlakpilot.app Apple Developer'da kayıtlı
- [ ] EAS Project ID app.json'a doldu
- [ ] App Store Connect'te uygulama oluşturuldu, ASC App ID eas.json'a yazıldı
- [ ] Demo hesap oluşturuldu, sample data dolu
- [ ] Ekran görüntüleri hazırlandı (en az 6.7" + 6.9" iPhone)
- [ ] iOS production build başarılı
- [ ] TestFlight'ta kendi telefonunda test edildi
- [ ] App Review form'u dolduruldu
- [ ] Submit for Review
