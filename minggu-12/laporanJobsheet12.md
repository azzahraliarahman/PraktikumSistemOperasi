# Jobsheet 11B :  Manajemen Service

## Praktek 10.1: Amati Layanan Aktif Saat Boot

1. Lihat semua layanan yang sedang berjalan.

```
systemctl list-units --type=service --state=running
# catat berapa banyak layanan yang aktif
```

Amati: Setiap baris menampilkan nama unit, status aktif/tidaknya, status dari sudut pandang
systemd, dan deskripsi singkat.

2. Lihat semua unit service yang ada (aktif maupun tidak).

```
systemctl list-unit-files --type=service | head -30
# enabled = akan start otomatis saat boot
# disabled = tidak start otomatis, bisa dijalankan manual
# static = tidak bisa di-enable/disable, hanya dipanggil oleh layanan
lain
```
3. Analisis waktu boot dan temukan layanan paling lambat.

```
systemd-analyze
systemd-analyze blame | head -15
Kode1.4:Anali
```

### Tantangan 10.1

Identifikasi tiga layanan dengan waktu inisialisasi terlama menggunakan systemd-analyze
blame. Gunakan pipeline dari Bab3(| sort-rh | head-3)untuk mempercepat pencariannya.Untuk setiap layanan,cari tahu fungsinya dengan systemctl cat nama-layanan.
Tuliskan nama layanan,waktu inisialisasinya, dan penjelasan singkat fungsinya.

### Jawaban Tantangan 10.1
* 3 Proses Terlambat :
  1. plymouth-read-write.service memerlukan 941ms (0.94 detik). Layanan ini berfungsi untuk memberi tahu Plymouth (Sistem animasi/peta splash screen saat Ubuntu booting) bahwa partisi disk utama berhasil di
     pindahkan dari mode read-only menjadi read-write.

  2. sys-kernel-config.mount memerlukan 924ms (0.92 detik). Layanan ini perintah mount otomatis dari systemd. Fungsinya adalah menempelkan (mounting) sistem file virtual khusus bernama configfs ke direktori /sys/kernel/config. Direktori ini digunakan oleh kernel Linux untuk mengatur dan mengonfigurasi fitur-fitur kernel secara dinamis dari ruang pengguna (userspace).
  3. sys-fs-fuse-connections.mount memerlukan 908ms (0.90 detik). Layanan ini adalah proses mounting untuk sistem file virtual FUSE (Filesystem in Userspace). Fungsinya adalah menempelkan direktori kontrol FUSE ke /sys/fs/fuse/connections agar sistem atau aplikasi pihak ketiga bisa membuat dan mengelola sistem file kustom mereka sendiri tanpa perlu mengubah kode kernel utama Linux.

## Praktek 10.2 : Kelola Layanan SSH

1. Periksa status SSH secara menyeluruh.

```
systemctl status ssh
systemctl is-active ssh
systemctl is-enabled ssh
```

Output systemctl status menampilkan banyak informasi:status aktif/inactive,PID
prosesutama, waktumulai, penggunaanmemori, dan beberapa baris log terbaru. Ini adalah
perintah pertama yang harus dijalankan ketika ada masalah layanan

2. Kode 1.9: Restart layanan dan pantau status

```
sudo systemctl restart ssh
systemctl status ssh
# perhatikan: Loaded, Active, dan Main PID bisa berubah setelah restart
```

3. Kode 1.10: Melihat dependensi layanan SSH

```
systemctl list-dependencies ssh
# layanan lain yang harus aktif sebelum SSH bisa berjalan
```

4. Cek semua unit yang gagal di sistem

```
systemctl --failed
# jika ada, ini adalah daftar layanan yang butuh perhatian
```

### Tantangan 10.2

Buat skrip Bash (referensi Bab 7) bernama cek-layanan.sh yang memeriksa status daftar layanan dari sebuah berkas teks. Berkas teks daftar-layanan.txt berisi satu nama layanan per baris (isi minimal: ssh, cron, rsyslog). Skrip membaca setiap nama layanan, memeriksa statusnya dengan systemctl is-active, lalu menulis laporan ke berkas laporan-layanan.log dengan format: [TANGGAL] nama-layanan: ACTIVE/INACTIVE. Gunakan date untuk mendapatkan tanggal.

## Praktek 10.3: Buat Layanan Sederhana dari Skrip Bash

1. Kode 1.13: Menyiapkan direktori dan konten layanan

```
cd ~/lab-os/chapter10-services
mkdir -p situs-demo
nano situs-demo/index.html
# Tulis isi berkas berikut
<!DOCTYPE html>
<html>
<body>
<h1>Halo dari layanan systemd kustom!</h1>
<p>Layanan ini dibuat pada praktek Bab 10.</p>
</body>
</html>
```
2. Kode 1.14: Skrip server HTTP untuk layanan

```
nano ~/lab-os/chapter10-services/jalankan-server.sh
# Tulis isi berkas berikut
#!/bin/bash
DIREKTORI="$HOME/lab-os/chapter10-services/situs-demo"
PORT=9090
echo "Memulai server di port $PORT..."
exec python3 -m http.server $PORT --directory "$DIREKTORI"
chmod +x ~/lab-os/chapter10-services/jalankan-server.sh
```
3. Kode 1.15: Membuat berkas unit kustom

```
Description=Demo Web Server Praktek Bab 10
After=network.target

[Service]
Type=simple
User=nama-pengguna-kamu
WorkingDirectory=/home/nama-pengguna-kamu/lab-os/chapter10-services/situs-demo
ExecStart=/usr/bin/python3 -m http.server 9090
Restart=on-failure
RestartSec=3s

[Install]
WantedBy=multi-user.target

# salin ke lokasi unit systemd
sudo cp ~/lab-os/chapter10-services/demo-web.service /etc/systemd/system/demo-web.service

# minta systemd membaca ulang berkas unit yang baru dibuat
sudo systemctl daemon-reload

```
4. Kode 1.16: Menjalankan dan memverifikasi layanan kustom
Bash

```
sudo systemctl start demo-web
systemctl status demo-web

# coba akses layanan
curl http://localhost:9090
```
5. Kode 1.17: Menguji restart otomatis
Bash

```
# lihat PID proses saat ini
systemctl status demo-web | grep "Main PID"

# hentikan proses secara paksa (simulasi crash)
sudo kill -9 $(systemctl show demo-web --property=MainPID --value)

# tunggu beberapa detik lalu cek -- systemd harus menghidupkannya kembali
sleep 5
systemctl status demo-web
# PID akan berubah karena proses baru dijalankan
```
6. Kode 1.18: Bersihkan layanan uji setelah selesai.

```
sudo systemctl disable --now demo-web
sudo rm /etc/systemd/system/demo-web.service
sudo systemctl daemon-reload
```
### Tantangan 10.3

Modifikasi berkas unit demo-web.service sebelum menghapusnya: tambahkan RestartSec=10s agar sistem menunggu 10 detik sebelum mencoba restart, dan tambahkan Environment="PORT=9091" lalu ubah ExecStart agar menggunakan variabel tersebut. Aktifkan layanan dengan enable dan WantedBy=multi-user.target, lalu uji apakah layanan aktif setelah systemctl daemon-reload. Dokumentasikan perbedaan perilaku dibanding versi sebelumnya.

 



