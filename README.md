# Pengujian API Firebase Auth dengan Postman
Repositori ini mendokumentasikan langkah-langkah pengujian end-to-end integrasi Firebase Authentication dengan Backend menggunakan Postman. Pengujian ini berfungsi sebagai *blueprint* untuk memastikan semua *endpoint* berfungsi dengan benar sebelum masuk ke tahap implementasi aplikasi klien (misalnya Flutter).

## Alat Tempur
Sebelum memulai, pastikan Firebase Project dan Postman sudah siap.
Website:
- [`Firebase`](https://firebase.google.com/)
- [`Postman on the web`](https://www.postman.com//)

Link Download:
- [`Postman app`](https://www.postman.com/downloads/)

## Setup Firebase Project
Masuk ke website [`Firebase`](https://firebase.google.com/), Sign In terlebih dahulu atau Sign Up jika belum ada akun.

jika sudah tekan tombol `Buka Konsol`

<img src="images/1-firesetup/firesetup(1.1).PNG" width="700">

Create a new Firebase Project

<img src="images/1-firesetup/firesetup(2).PNG" width="700">

Beri nama project

<img src="images/1-firesetup/firesetup(3).PNG" width="700">

Continue (biarkan default)

<img src="images/1-firesetup/firesetup(4).PNG" width="700">
<img src="images/1-firesetup/firesetup(5).PNG" width="700">

Ketika sampai di Configure Google Analytics, pilih `Default Account for Firebase` lalu tekan Create Project, tunggu proses selesai

<img src="images/1-firesetup/firesetup(6).PNG" width="700">

Sampai ditampilan awal ketika selesai membuat Firebase Project tekan tombol `+ Add app`

<img src="images/1-firesetup/firesetup(9.1).PNG" width="700">

Lalu pilih yang web app

<img src="images/1-firesetup/firesetup(10.1).PNG" width="700">

Beri nama (biarkan checkbox kosong), lalu `Register app`

<img src="images/1-firesetup/firesetup(11).PNG" width="700">

Ini adalah bagian penting untuk Pengujian nanti, kita continue saja dulu

<img src="images/1-firesetup/firesetup(12).PNG" width="700">

Tampilan awal ketika selesai membuat Firebase Project

<img src="images/1-firesetup/firesetup(13).PNG" width="700">

## Setup Firebase Auth

Tekan tombol dropdown `Build` lalu pilih menu Authentication

<img src="images/2-fireauth/fireauth(1.1).PNG" width="700">

Get Started

<img src="images/2-fireauth/fireauth(2).PNG" width="700">

Pada Sign-in method ini ada 2 yang harus kita aktifkan, yang pertama `Email/Password` pada Native Providers

<img src="images/2-fireauth/fireauth(3).PNG" width="700">
<img src="images/2-fireauth/fireauth(4).PNG" width="700">
<img src="images/2-fireauth/fireauth(5).PNG" width="700">

Lalu tekan tombol `Add new provider` dan aktifkan juga yang `Google` pada bagian Additional Providers

<img src="images/2-fireauth/fireauth(6).PNG" width="700">

Beri nama dan isi dengan email yang sedang digunakan/aktif, lalu tekan tombol `Save`

<img src="images/2-fireauth/fireauth(7).PNG" width="700">
<img src="images/2-fireauth/fireauth(8).PNG" width="700">

## Setup Postman

Sekarang kita setup Postman nya, dimulai dari buat `Environment` terlebih dahulu

<img src="images/3-postmanenvi/postmanenvi(1).PNG" width="700">
<img src="images/3-postmanenvi/postmanenvi(2).PNG" width="700">
<img src="images/3-postmanenvi/postmanenvi(3).PNG" width="700">

Pada Variable `FIREBASE_API_KEY` isi value nya dengan API Key Project Firebase yang kita buat diawal tadi, kembali ke `Firebase` tekan tombol gear lalu pilih `Project Settings` dan scroll down

<img src="images/3-postmanenvi/postmanenvi(4.1).PNG" width="700">

Salin apiKey nya dan masukan pada Environment yang kita buat di Postman

<img src="images/3-postmanenvi/postmanenvi(5.1).PNG" width="700">

Isi USER_EMAIL dengan email aktif untuk testing, dan isi USER_PASSWORD nya juga

<img src="images/3-postmanenvi/postmanenvi(6).PNG" width="700">

## Setup Request
### 1. Request Sign Up

Pertama kita buat `Collection` agar struktur rapi, dan beri nama (bebas)

<img src="images/4-reqsignup/reqsignup(1).PNG" width="700">

Lalu `Add Request`

<img src="images/4-reqsignup/reqsignup(2.1).PNG" width="700">

Beri nama `Sign Up` dan atur method ke `POST` lalu isi URL dengan `https://identitytoolkit.googleapis.com/v1/accounts:signUp?key={{FIREBASE_API_KEY}}`

<img src="images/4-reqsignup/reqsignup(3).PNG" width="700">

Pindah ke tab `Headers`, isi dengan `Content Type` dan value `application/json`

<img src="images/4-reqsignup/reqsignup(4).PNG" width="700">

Masuk ke tab Body pilih yang raw dan isi dengan ini:
```bash
{
  "email": "{{USER_EMAIL}}",
  "password": "{{USER_PASSWORD}}",
  "returnSecureToken": true
}
```
Sebelum di `Send`, kita pastikan dulu Environment mana yang kita gunakan, kita akan menggunakan Environment yang kita buat

<img src="images/4-reqsignup/reqsignup(5.1).PNG" width="700">
<img src="images/4-reqsignup/reqsignup(6).PNG" width="700">

Lalu kita `Send` jika berhasil akan mendapatkan respons `200 OK`

<img src="images/4-reqsignup/reqsignup(7).PNG" width="700">

Sekarang kita lihat apakah masuk ke Firebase, pindah ke tab Users pada bagian `Authentication`, jika berhasil send, maka email nya akan tersimpan di Firebase

<img src="images/4-reqsignup/reqsignup(8).PNG" width="700">

Jangan lupa untuk mengambil/copy `idToken` dan masukan/paste di Variables `FIREBASE_ID_TOKEN` pada Environment yang kita buat

<img src="images/4-reqsignup/reqsignup(9.1).PNG" width="700">
<img src="images/4-reqsignup/reqsignup(10).PNG" width="700">

### 2. Request Verify Email

Kita buat request baru pada collection kita lalu beri nama `Verify Email`, pilih method `POST` dan isi URL dengan https://identitytoolkit.googleapis.com/v1/accounts:sendOobCode?key={{FIREBASE_API_KEY}}

<img src="images/5-reqverify/reqverify(1).PNG" width="700">

Tab Headers (ikuti seperti difoto)

<img src="images/5-reqverify/reqverify(2).PNG" width="700">

Tab Body isi dengan ini lalu `Send`
```bash
{
  "requestType": "VERIFY_EMAIL",
  "idToken": "{{FIREBASE_ID_TOKEN}}"
}
```
<img src="images/5-reqverify/reqverify(3).PNG" width="700">

Jika berhasil akan mendapatkan respons `200 OK`

<img src="images/5-reqverify/reqverify(4).PNG" width="700">

Sekarang kita buka Gmail kita untuk membuka email verifikasinya, biasanya email ada di `Spam`, jika sudah ketemu kita tinggal klik link yang ada pada email

<img src="images/5-reqverify/emailverif(1).PNG" width="700">
<img src="images/5-reqverify/emailverif(2).PNG" width="700">

### 3. Request Cek Verify

Lanjut kita buat request lagi dan beri nama `Cek Verify`, pilih method `POST` dan masukan URL https://identitytoolkit.googleapis.com/v1/accounts:lookup?key={{FIREBASE_API_KEY}}

<img src="images/6-reqcekverify/reqcekverify(1).PNG" width="700">

Lalu kita langsung ke Tab Body dan isi dengan
```bash
{
  "idToken": "{{FIREBASE_ID_TOKEN}}"
}
```
<img src="images/6-reqcekverify/reqcekverify(2).PNG" width="700">

Lalu send, dan lihat status pada respon nya bagian emailVerified, jika `true` berarti benar berhasil terverifikasi

<img src="images/6-reqcekverify/reqcekverify(3).PNG" width="700">
<img src="images/6-reqcekverify/reqcekverify(4).PNG" width="700">

### 4. Request Login

Lanjut kita buat request lagi dan beri nama `Login`, pilih method `POST` dan masukan URL https://identitytoolkit.googleapis.com/v1/accounts:lookup?key={{FIREBASE_API_KEY}}

<img src="images/7-reqlogin/reqlogin(1).PNG" width="700">

Lalu kita langsung ke Tab Body dan isi dengan
```bash
{
  "email": "{{USER_EMAIL}}",
  "password": "{{USER_PASSWORD}}",
  "returnSecureToken": true
}
```
<img src="images/7-reqlogin/reqlogin(2).PNG" width="700">

Lalu send, jika respons `200 OK` maka berhasil login

<img src="images/7-reqlogin/reqlogin(3).PNG" width="700">

## Script Tambahan
### 1. Script Untuk Request Sign Up
```bash
const json = pm.response.json();
if (pm.response.code === 200) {
  pm.environment.set("FIREBASE_ID_TOKEN", json.idToken);
  pm.environment.set("FIREBASE_LOCAL_ID", json.localId);
  pm.environment.set("FIREBASE_REFRESH_TOKEN", json.refreshToken);
  console.log("Register sukses. UID:", json.localId);
  console.log("PERHATIAN: Email belum diverifikasi. Lanjut ke Step 2.");
} else {
  console.log("Register gagal:", json.error.message);
}
```
Tujuan: Menyimpan data pengguna baru secara otomatis agar bisa langsung digunakan untuk minta email verifikasi.
Jadi saat pindah ke Step 2 (Kirim Email Verifikasi), kita tidak perlu capek-capek copy-paste token yang panjang. Postman akan otomatis mengambilnya dari kotak penyimpanan.

<img src="images/4-reqsignup/scriptsignup.PNG" width="700">

### 2. Script Untuk Request Verify Email
```bash
if (pm.response.code === 200) {
  const json = pm.response.json();
  console.log("Email verifikasi dikirim ke:", json.email);
  console.log("Sekarang buka inbox email dan klik link verifikasi.");
  console.log("Setelah klik, lanjut ke Step 3 untuk cek status.");
} else {
  console.log("Gagal kirim email:", pm.response.json().error.message);
}
```

Tujuan: Memberikan notifikasi pengingat kepada penguji (Anda) bahwa proses sudah pindah ke kotak masuk (inbox) email.
Fungsi utama script ini murni sebagai pengingat (Log). Ia akan memunculkan tulisan di layar Console Postman untuk mengingatkan Anda.

<img src="images/5-reqverify/scriptverif.PNG" width="700">

### 3. Script Untuk Request Login
```bash
const json = pm.response.json();
if (pm.response.code === 200) {
  // Update environment dengan idToken BARU hasil login
  pm.environment.set("FIREBASE_ID_TOKEN", json.idToken);
  pm.environment.set("FIREBASE_REFRESH_TOKEN", json.refreshToken);
  console.log("Login berhasil. Token diperbarui.");
  console.log("Lanjut ke Step 5: kirim token ke backend.");
} else {
  console.log("Login gagal:", json.error.message);
}
```

Tujuan: Memperbarui (Update) token lama dengan token baru yang sudah memuat status bahwa email telah diverifikasi.
Setelah Anda mengklik link di email, Firebase sudah tahu email Anda valid. Tapi, token lama yang ada di Postman (dari Step 1) masih token jadul yang menyatakan email Anda belum valid.

<img src="images/7-reqlogin/scriptlogin.PNG" width="700">
