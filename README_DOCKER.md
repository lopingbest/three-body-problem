README Proyek Laravel
Dokumen ini berisi panduan singkat untuk deployment aplikasi Laravel Anda yang di-container-kan menggunakan Docker Compose.

1. Pull Perubahan Terbaru ke Server
Langkah pertama adalah memastikan Anda memiliki versi kode terbaru dari repository Git Anda di server.

git pull origin main

Pastikan Anda berada di direktori root proyek Laravel Anda di server.

Ganti main jika nama branch utama Anda di GitHub adalah master atau nama lain.

2. Jalankan Aplikasi Menggunakan Docker Compose
Setelah kode terbaru berhasil di-pull, Anda dapat membangun image Docker dan menjalankan semua layanan aplikasi (seperti aplikasi Laravel, Nginx, dan MySQL) menggunakan Docker Compose.

docker compose up -d --build

docker compose up: Perintah ini akan memulai semua layanan yang didefinisikan dalam file docker-compose.yml Anda.

-d: Opsi ini menjalankan kontainer di latar belakang (detached mode), sehingga terminal Anda tetap bisa digunakan.

--build: Opsi ini akan memaksa Docker Compose untuk membangun kembali image dari Dockerfile Anda. Ini sangat penting jika Anda telah melakukan perubahan pada Dockerfile atau jika ada dependensi PHP/Composer baru yang ditambahkan yang memerlukan pembangunan ulang image PHP-FPM.

Catatan Penting:
Pastikan Docker dan Docker Compose sudah terinstal di server Anda.

Periksa file .env di direktori root proyek Anda di server. Pastikan variabel lingkungan seperti koneksi database (DB_HOST, DB_DATABASE, DB_USERNAME, DB_PASSWORD) sudah dikonfigurasi dengan benar
