# Bombon AI — Android (Capacitor)

## Yang sudah dikerjakan
- Project Capacitor + platform Android sudah di-generate (folder `android/`).
- Login Google: dibuka lewat Custom Tabs (`@capacitor/browser`), bukan WebView biasa — WebView polos ditolak Google. Callback ditangkap via deep link `com.bombon.ai://login-callback`.
- Tombol lampirkan file (`attachBtn`) sekarang minta izin penyimpanan dulu (`@capacitor/filesystem`) sebelum file picker kebuka. Kalau ditolak, muncul alert.
- `www/config.js` sekarang punya `API_BASE` — app manggil Netlify Functions kamu langsung (karena app native gak nge-host `/api/chat` sendiri). Backend (Netlify + Groq) tetap harus online/live.

## Yang WAJIB kamu isi sebelum build
1. `www/config.js`:
   - `SUPABASE_URL` dan `SUPABASE_ANON_KEY` (sama kayak punya web).
   - `API_BASE` → URL Netlify site kamu, contoh `https://bombon-ai.netlify.app`.
2. Supabase Dashboard → Authentication → URL Configuration → **Redirect URLs**, tambahkan:
   ```
   com.bombon.ai://login-callback
   ```
3. Kalau appId `com.bombon.ai` mau diganti, edit `capacitor.config.json`, `AndroidManifest.xml` (package + intent-filter scheme), dan folder `android/app/src/main/java/...`.

## Cara build APK (via GitHub Actions, gak butuh PC)
Udah ada workflow di `.github/workflows/build-apk.yml`. Caranya:

1. Push seluruh folder ini ke repo GitHub baru (bisa lewat app GitHub / Working Copy / termux, atau upload manual file per file di web GitHub kalau kepepet)
2. Buka tab **Actions** di repo tsb → workflow "Build APK" jalan otomatis tiap push ke branch `main` (atau klik "Run workflow" manual)
3. Tunggu selesai (~3-5 menit) → buka run yang sukses → scroll ke bawah ke bagian **Artifacts** → download `bombon-ai-debug-apk` (isinya `app-debug.apk`)
4. Install APK itu di HP kamu (aktifin "Install unknown apps" dulu kalau diminta)

Catatan: `node_modules/` dan `android/.gradle/` sengaja gak dimasukin ke zip biar ringan — pas `npm install` jalan di GitHub Actions, semua ke-generate otomatis, gak perlu kamu isi manual.

## Icon & nama app
Ganti icon di `android/app/src/main/res/mipmap-*/`, dan nama app di `android/app/src/main/res/values/strings.xml`.
