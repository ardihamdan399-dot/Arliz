# 📱 Panduan Publikasi Arliz Gameplay ke Play Store

## 🎯 Prasyarat

1. ✅ Node.js & npm (sudah terinstall)
2. ✅ Java JDK 11+ (untuk Bubblewrap)
3. ✅ Google Account (untuk Play Store)
4. ✅ Kartu Kredit (untuk dev account $25)

---

## 📋 STEP 1: Setup Play Store Developer Account

### Daftar Google Play Console

1. Buka: https://play.google.com/console
2. Klik "Create account"
3. Isi detail:
   - **Account type**: Individual
   - **Developer name**: Ardi Hamdan
   - **Email**: ardihamdan399@gmail.com
   - **Phone**: Your number
   - **Address**: Your address
4. Bayar: $25 (sekali selamanya)
5. Done! Tunggu verifikasi ~1 jam

---

## 🔧 STEP 2: Setup Development Environment

### Install Bubblewrap CLI

```bash
# Global installation
npm install -g @bubblewrap/cli

# Verify installation
bubblewrap --version
```

### Install Java JDK (jika belum)

**Windows:**
```bash
# Download dari: https://www.oracle.com/java/technologies/downloads/
# Install & set PATH environment variable
java -version  # Verify
```

**macOS:**
```bash
brew install openjdk@11
```

**Linux:**
```bash
sudo apt-get install openjdk-11-jdk
```

---

## 🛠️ STEP 3: Generate APK/AAB dengan Bubblewrap

### Initialize Project

```bash
# Create working directory
mkdir arliz-build
cd arliz-build

# Initialize Bubblewrap
bubblewrap init \
  --manifest https://ardihamdan399-dot.github.io/Arliz/manifest.json \
  --package-id "com.ardihamdan.arlizgameplay" \
  --app-name "Arliz Gameplay" \
  --app-version 1 \
  --app-version-display "1.0.0" \
  --launcher-icon-path "./icon.png"
```

### Konfigurasi (bubbleconfig.json)

File akan otomatis dibuat. Edit jika perlu:

```json
{
  "appName": "Arliz Gameplay",
  "appVersion": 1,
  "appVersionDisplay": "1.0.0",
  "packageId": "com.ardihamdan.arlizgameplay",
  "launcherIconPath": "./icon-512.png",
  "primaryColor": "#fbbf24",
  "fallbackType": "customtabs",
  "features": {
    "online": true,
    "shortcutSize": 128
  },
  "manifest": "https://ardihamdan399-dot.github.io/Arliz/manifest.json"
}
```

### Generate APK & AAB

```bash
# Generate bundle (untuk Play Store)
bubblewrap build

# Output files:
# - dist/app-signed.aab  (Android App Bundle - untuk Play Store)
# - dist/app-signed.apk  (APK - untuk testing manual)
```

**Proses akan:**
1. Download Android SDK
2. Build Android project
3. Sign dengan keystore
4. Generate APK & AAB
5. Selesai! (~5-10 menit)

---

## 📤 STEP 4: Upload ke Google Play Console

### Login & Create App

1. Buka: https://play.google.com/console
2. Klik "Create app"
3. Isi:
   - **App name**: "Arliz Gameplay"
   - **Default language**: Indonesian (Bahasa Indonesia)
   - **App type**: Game
   - **Category**: Educational
   - **Free or paid**: Free
4. Klik "Create app"

### Fill App Details

**Di bagian "All apps" → "Arliz Gameplay":**

#### 1. App Access
```
- Device categories: Phones, Tablets, Wearables
- Restrictions: None
```

#### 2. Store Listing

**Bagian: "Store Listing"**

- **Short description** (50 karakter):
  ```
  Game Quiz Interaktif - Operasi Intelijen Challenge
  ```

- **Full description** (4000 karakter):
  ```
  Arliz Gameplay adalah game quiz interaktif dengan 50 soal berkualitas tentang Sejarah, Matematika, Fisika, dan Sosiologi Islam.

  🎮 FITUR:
  ✅ 50 Soal Interaktif
  ✅ Sistem Poin & Lives
  ✅ UI Modern & Responsif
  ✅ Bisa Offline
  ✅ Tidak Ada Iklan
  ✅ Tidak Ada Data Collection

  🏛️ KATEGORI SOAL:
  • Sejarah & Filologi
  • Matematika
  • Fisika & Sains
  • Teologi & Sosiologi
  • Budaya Global

  🎯 CARA BERMAIN:
  1. Baca pertanyaan
  2. Lihat jawaban
  3. Verifikasi jawaban Anda
  4. Kumpulkan poin
  5. Selesaikan 50 soal

  ⚡ FITUR PWA:
  • Offline support
  • Install sebagai app
  • Update otomatis
  • File kecil (~15MB)
  • Performa cepat

  Tantang logika Anda sekarang juga!
  ```

- **Screenshot** (Minimal 2, maksimal 8):
  - 1200 x 1920px (portrait)
  - Tangkap: Start screen, gameplay, score screen
  - Upload via Play Console

- **App icon** (512 x 512px):
  - Format: PNG
  - Tanpa transparansi
  - Upload file

- **Feature graphic** (1024 x 500px):
  - Gambar banner untuk store listing
  - Bisa gunakan tools desain (Canva, etc)

#### 3. Content Rating

Klik "Content rating questionnaire":
- Category: Game (Permainan)
- Content: Educational, No violence, No adult content
- Rating: Everyone (Semua kalangan)

#### 4. Target Audience

- **Age range**: Not age restricted
- **Content guidelines**: Pass (sesuai policy)

#### 5. Release Management

**Setup Internal Testing Track:**

1. Klik "Releases" → "Internal testing"
2. Klik "Create release"
3. **Upload APK/AAB:**
   - Drag & drop file `app-signed.aab`
   - Atau gunakan "Browse files"
4. **Release notes:**
   ```
   Version 1.0.0 - Launch Release
   • Initial release
   • 50 quiz questions
   • PWA support
   • Offline functionality
   ```
5. Klik "Review release"
6. Klik "Start rollout to internal testing"

**Test dulu di device:**
- Undang tester (email)
- Test 24-48 jam
- Fix bug jika ada

---

## 🚀 STEP 5: Submit ke Production

**Setelah testing berhasil:**

1. Buat release baru ke "Production"
2. Upload file `app-signed.aab` yang sama
3. Isi release notes
4. Review semua detail
5. Klik "Start rollout to production"
6. **Submit for review**

**Review time:**
- Biasanya: 2-3 jam
- Worst case: 24 jam
- Google akan notify via email

---

## ✅ Checklist Pre-Launch

```
☐ Google Play Developer Account dibuat ($25 dibayar)
☐ Java JDK terinstall
☐ Bubblewrap CLI terinstall
☐ PWA manifest.json sudah siap
☐ Icon 512x512px sudah siap
☐ Screenshots 1200x1920px sudah siap
☐ Feature graphic 1024x500px sudah siap
☐ Store listing filled lengkap
☐ Content rating completed
☐ APK/AAB sudah generated
☐ Internal testing passed
☐ Ready untuk production release
```

---

## 📱 Troubleshooting

### Error: "Java not found"
```bash
# Set JAVA_HOME
export JAVA_HOME=/Library/Java/JavaVirtualMachines/openjdk-11.jdk/Contents/Home
```

### Error: "Android SDK not found"
```bash
# Install Android SDK
bubblewrap install-sdk
```

### APK signing failed
```bash
# Regenerate keystore
bubblewrap create-signing-key
```

---

## 🎉 Setelah Launch

```
✅ App live di Play Store
✅ Dapat download dari: https://play.google.com/store/apps/details?id=com.ardihamdan.arlizgameplay
✅ User bisa install & main
✅ Update bisa dilakukan dari Play Console
✅ Monitor download & ratings
```

---

## 📊 Post-Launch Maintenance

1. **Monitor Reviews** - Baca feedback user
2. **Fix Issues** - Update di web, user dapat otomatis
3. **Add Features** - Develop fitur baru
4. **Collect Data** - Track download & rating

---

**Selamat meluncurkan Arliz Gameplay ke Play Store!** 🚀

Bisa hubungi saya jika ada yang kurang jelas!
