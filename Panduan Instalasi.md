🕌 Sistem Konsultasi Agama Islam KUA
Aplikasi web ringan berbasis file tunggal (Single-File Application) untuk layanan tanya jawab dan konsultasi keagamaan masyarakat. Aplikasi ini dirancang agar mudah dide-deploy, cepat diakses, dan responsif di semua perangkat (termasuk mobile).

Dibangun dengan antarmuka modern dan terintegrasi dengan database real-time serta sistem notifikasi otomatis.

✨ Fitur Utama
Form Konsultasi Publik: Pengguna dapat mengirimkan pertanyaan dengan opsi untuk menyembunyikan identitas (Anonim).

Daftar Pertanyaan Terjawab: Masyarakat dapat membaca, mencari, dan memberikan komentar pada pertanyaan yang sudah dijawab oleh penyuluh agama.

Panel Admin KUA: Dasbor khusus untuk mengelola antrian pertanyaan, memposting jawaban, dan mengedit/menghapus data.

Database Real-time: Menggunakan Firebase Firestore untuk penyimpanan dan sinkronisasi data seketika tanpa perlu memuat ulang halaman.

Notifikasi Telegram: Peringatan instan ke HP/Grup Telegram penyuluh setiap kali ada pertanyaan baru yang masuk.

Dukungan Teks Arab (Dalil): Fitur pemformatan khusus untuk menampilkan teks Al-Qur'an dan Hadis dengan rapi (rata kanan, font Noto Naskh Arabic).

🛠️ Prasyarat
Sebelum menginstal dan memodifikasi aplikasi ini, pastikan Anda memiliki:

Akun Google/Firebase (Untuk membuat database Firestore).

Akun Telegram (Untuk membuat Bot notifikasi).

Teks editor (seperti VS Code, Sublime Text, atau editor bawaan sistem).

🚀 Panduan Instalasi & Konfigurasi
Karena aplikasi ini sepenuhnya berbasis Client-Side (HTML/CSS/JS murni), Anda tidak perlu melakukan npm install atau konfigurasi server backend. Anda hanya perlu memasukkan kunci API (API Keys) ke dalam file index.html.

Langkah 1: Pengaturan Firebase (Database)
Buka Firebase Console dan buat proyek baru.

Di menu kiri, pilih Firestore Database dan klik Create database (Pilih lokasi server terdekat, misalnya Singapura).

Buka tab Rules (Aturan) di Firestore dan ubah kodenya menjadi seperti ini (untuk mode uji coba publik):

JavaScript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
Masuk ke Project Settings (ikon gir) > General, gulir ke bawah, dan tambahkan aplikasi web (</>).

Salin objek firebaseConfig yang diberikan oleh Firebase.

Buka file index.html, cari bagian const firebaseConfig = { ... } (sekitar baris 480), dan ganti isinya dengan data konfigurasi milik Anda.

Langkah 2: Pengaturan Notifikasi Telegram
Buka aplikasi Telegram, cari @BotFather, dan ketik /newbot untuk membuat bot baru.

Simpan HTTP API Token yang diberikan oleh BotFather.

Masukkan bot tersebut ke dalam Grup Telegram KUA (atau gunakan chat pribadi), lalu kirim pesan /start atau pesan apapun ke bot tersebut.

Buka URL berikut di browser untuk melihat ID Chat Anda (ganti TOKEN_BOT_KAMU):
https://api.telegram.org/botTOKEN_BOT_KAMU/getUpdates

Cari tulisan "chat":{"id": -123456789.... Angka tersebut adalah Chat ID Anda.

Buka file index.html, cari fungsi kirimNotifTelegram() (di bagian paling bawah).

Masukkan Token dan Chat ID Anda ke dalam variabel botToken dan chatId.

Langkah 3: Pengaturan Akses Admin (Login)
Secara bawaan (default), sistem login admin diproteksi menggunakan teknik encoding Base64 agar tidak terbaca langsung saat di-inspect oleh pengunjung.

Username Default: admin

Password Default: kua2025

Jika Anda ingin mengubahnya:

Buka console browser atau alat generator Base64 online.

Ubah username dan password baru Anda ke dalam format Base64.
(Contoh di console browser: btoa('katasandibaru') akan menghasilkan 'a2F0YXNhbmRpYmFydQ==').

Buka file index.html, cari fungsi doLogin(), dan ganti teks Base64 YWRtaW4= (untuk username) dan a3VhMjAyNQ== (untuk password) dengan kode Base64 Anda yang baru.

🌐 Cara Publikasi (Deployment)
Karena ini adalah file statis murni, Anda bisa mengunggah (hosting) file index.html ini ke berbagai layanan hosting gratis dengan sangat mudah, seperti:

Vercel (Sangat disarankan: Cukup seret dan lepas folder proyek atau hubungkan ke repositori GitHub ini).

GitHub Pages (Aktifkan melalui Settings > Pages di repositori Anda).

Netlify atau hosting CPanel tradisional.

💡 Panduan Penggunaan Khusus (Panel Admin)
Saat Anda menjawab pertanyaan di Panel Admin dan ingin menyisipkan Dalil (Ayat Al-Qur'an atau Hadis), gunakan tag pendek [arab] dan [/arab] untuk mengapit teks Arab tersebut.

Contoh Penulisan di Kolom Jawaban:

Plaintext
Jika muntah terjadi secara tidak sengaja, maka puasanya tetap sah.

[arab] مَنْ ذَرَعَهُ قَيْءٌ وَهُوَ صَائِمٌ فَلَيْسَ عَلَيْهِ قَضَاءٌ [/arab]

Berdasarkan Hadis Riwayat Tirmidzi.
Sistem akan secara otomatis menyulap teks yang diapit tersebut menjadi format Arab khusus dengan rata kanan, ukuran font yang lebih besar, dan garis dekorasi emas.

Dibuat untuk memfasilitasi pelayanan umat secara digital. 🕌
