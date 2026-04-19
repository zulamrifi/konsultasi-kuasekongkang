

# 🕌 Sistem Konsultasi Agama Islam KUA

Aplikasi web ringan berbasis file tunggal (*Single-File Application*) untuk layanan tanya jawab dan konsultasi keagamaan masyarakat. Aplikasi ini dirancang agar mudah dide-deploy, cepat diakses, dan responsif di semua perangkat (termasuk *mobile*). 

Dibangun dengan antarmuka modern dan terintegrasi dengan database *real-time*, sistem manajemen akun dinamis, serta sistem notifikasi otomatis.

## ✨ Fitur Utama
* **Form Konsultasi Publik:** Pengguna dapat mengirimkan pertanyaan dengan opsi untuk menyembunyikan identitas (Anonim).
* **Daftar Pertanyaan Terjawab:** Masyarakat dapat membaca, mencari, dan memberikan komentar pada pertanyaan yang sudah dijawab oleh penyuluh agama.
* **Panel Admin KUA:** Dasbor khusus untuk mengelola antrian pertanyaan, memposting jawaban, dan mengedit/menghapus data.
* **Database Real-time:** Menggunakan **Firebase Firestore** untuk penyimpanan dan sinkronisasi data seketika tanpa perlu memuat ulang halaman.
* **Manajemen Admin Dinamis:** Menggunakan **Firebase Authentication** sehingga *password* dan *email* admin dapat diganti kapan saja melalui Dasbor Firebase tanpa perlu mengedit kode HTML.
* **Notifikasi Telegram:** Peringatan instan ke HP/Grup Telegram penyuluh setiap kali ada pertanyaan baru yang masuk.
* **Dukungan Teks Arab (Dalil):** Fitur pemformatan khusus untuk menampilkan teks Al-Qur'an dan Hadis dengan rapi (rata kanan, *font* Noto Naskh Arabic).

---

## 🛠️ Prasyarat
Sebelum menginstal dan memodifikasi aplikasi ini, pastikan Anda memiliki:
1. Akun [Google/Firebase](https://firebase.google.com/) (Untuk membuat database Firestore dan sistem Authentication).
2. Akun [Telegram](https://web.telegram.org/) (Untuk membuat Bot notifikasi).
3. Teks editor (seperti VS Code, Sublime Text, atau editor bawaan sistem).

---

## 🚀 Panduan Instalasi & Konfigurasi

### Langkah 1: Pengaturan Firebase (Database)
1. Buka [Firebase Console](https://console.firebase.google.com/) dan buat proyek baru.
2. Di menu kiri, pilih **Firestore Database** dan klik **Create database** (Pilih lokasi server terdekat, misalnya Singapura).
3. Buka tab **Rules** (Aturan) di Firestore dan ubah kodenya menjadi seperti ini (untuk mode uji coba publik):
   ```javascript
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /{document=**} {
         allow read, write: if true;
       }
     }
   }
   ```
4. Masuk ke **Project Settings** (ikon gir) > **General**, gulir ke bawah, dan tambahkan aplikasi web (`</>`).
5. Salin objek `firebaseConfig` yang diberikan oleh Firebase.
6. Buka file `index.html`, cari bagian `const firebaseConfig = { ... }` dan ganti isinya dengan data konfigurasi proyek Anda.

### Langkah 2: Pengaturan Akun Admin (Firebase Authentication)
Anda tidak perlu menulis *password* di dalam HTML. Akun dikelola langsung dari Firebase:
1. Di Firebase Console menu sebelah kiri, klik **Build** > **Authentication**.
2. Klik tombol **Get Started**.
3. Pilih tab **Sign-in method**, klik opsi **Email/Password**, aktifkan (Enable), lalu simpan.
4. Pindah ke tab **Users**, lalu klik **Add User**.
5. Masukkan alamat Email dan Password rahasia Anda. Ini akan menjadi akses masuk (login) ke Panel Admin KUA.
*(Kapan pun Anda ingin mengganti password admin, cukup kembali ke menu Users ini dan reset atau hapus/buat ulang akunnya).*

### Langkah 3: Pengaturan Notifikasi Telegram
1. Buka aplikasi Telegram, cari **@BotFather**, dan ketik `/newbot` untuk membuat bot baru.
2. Simpan **HTTP API Token** yang diberikan oleh BotFather.
3. Masukkan bot tersebut ke dalam Grup Telegram KUA (atau gunakan chat pribadi), lalu kirim pesan `/start` atau pesan apapun ke bot tersebut.
4. Buka URL berikut di browser untuk melihat ID Chat Anda (ganti `TOKEN_BOT_KAMU` dengan token Anda):
   `https://api.telegram.org/botTOKEN_BOT_KAMU/getUpdates`
5. Cari tulisan `"chat":{"id": -123456789...`. Angka tersebut adalah **Chat ID** Anda.
6. Buka file `index.html`, cari fungsi `kirimNotifTelegram()`.
7. Masukkan **Token** dan **Chat ID** Anda ke dalam variabel `botToken` dan `chatId`.

---

## 🌐 Cara Publikasi (Deployment)

Karena ini adalah file statis murni, Anda bisa mengunggah (hosting) file `index.html` ini ke berbagai layanan hosting gratis dengan sangat mudah, seperti:
* **Vercel** (Sangat disarankan: Cukup seret dan lepas folder proyek atau hubungkan ke repositori GitHub ini).
* **GitHub Pages** (Aktifkan melalui *Settings > Pages* di repositori Anda).
* **Netlify** atau hosting CPanel tradisional.

---

## 💡 Panduan Penggunaan Khusus (Panel Admin)

Saat Anda menjawab pertanyaan di Panel Admin dan ingin menyisipkan **Dalil (Ayat Al-Qur'an atau Hadis)**, gunakan *tag* pendek `[arab]` dan `[/arab]` untuk mengapit teks Arab tersebut.

**Contoh Penulisan di Kolom Jawaban:**
```text
Jika muntah terjadi secara tidak sengaja, maka puasanya tetap sah.

[arab] مَنْ ذَرَعَهُ قَيْءٌ وَهُوَ صَائِمٌ فَلَيْسَ عَلَيْهِ قَضَاءٌ [/arab]

Berdasarkan Hadis Riwayat Tirmidzi.
```

Sistem akan secara otomatis menyulap teks yang diapit tersebut menjadi format Arab khusus dengan rata kanan, ukuran *font* yang lebih besar, dan garis dekorasi emas.

---
*Dibuat untuk memfasilitasi pelayanan umat secara digital.* 🕌
