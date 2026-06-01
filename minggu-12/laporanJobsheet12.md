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

### jawaban tantangan 10.2

<img width="478" height="190" alt="image" src="https://github.com/user-attachments/assets/f8eedc7b-f08e-4431-b085-8d274d192721" />


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

### jawaban tantangan 10.3

<img width="470" height="148" alt="image" src="https://github.com/user-attachments/assets/58e819d5-6034-4e9d-9b9c-94bb58f392bc" />

## Praktek 10.4 :Filter dan Analisis Log Layanan

1: Lihat Log SSH dari Satu Jam Terakhir
membongkar buku catatan (log) sistem, khusus mencari kejadian yang terjadi pada layanan ssh atau cron dalam 1 jam terakhir saja

```
# Gunakan ini untuk melihat log SSH 1 jam terakhir
journalctl -u ssh --since "1 hour ago" --no-pager

# JIKA log SSH kosong/tidak ada, gunakan perintah alternatif ini (melihat log cron):
journalctl -u cron --since "1 hour ago" --no-pager
```

2: Filter Log Berprioritas Error ke Atas

Perintah ini gunanya seperti filter penyaring. Dia akan membuang semua log pesan santai (informasi biasa) dan hanya menampilkan catatan yang statusnya gawat atau error semenjak laptop/VM kamu dinyalakan (boot).

```
# Menampilkan semua log status error semenjak komputer dinyalakan
journalctl -b -p err --no-pager
```

3: Ikuti Log Secara Real-Time Sambil Memicu Aktivitas

```
# JALANKAN DI TERMINAL PERTAMA (Terminal ini akan diam mengawasi secara live):
journalctl -u ssh -f

# JALANKAN DI TERMINAL KEDUA (Untuk memicu aktivitas masuk sistem):
ssh localhost
```

4: Masuk ke Folder Kerja

```
cd ~/lab-os/chapter10-services

1. Simpan semua log layanan SSH dari hari ini ke berkas teks:
Bash

journalctl -u ssh --since today --no-pager > log-ssh-hari-ini.txt

2. Hitung jumlah total baris log yang berhasil disimpan:
Bash

wc -l log-ssh-hari-ini.txt

3. Cari baris yang mengandung kata "error" atau "failed" (Maksimal 20 baris pertama):
Bash

grep -i "error\|failed" log-ssh-hari-ini.txt | head -20
```

### tantangan 10.4

Ekstrak semua log dengan prioritas error (-p err) dari 24 jam terakhir untuk layanan SSH, simpan ke berkas error-ssh-24jam.txt. Gunakan pipeline dari Bab 3 untuk menghitung total jumlah baris error dengan wc -l, lalu tampilkan 10 pesan error yang paling sering muncul menggunakan sort | uniq -c | sort -rn | head -10. Tuliskan perintah lengkap yang kamu gunakan.

### jawaban tantangan  10.4

1. Mengambil dan menyaring log error 24 jam terakhir:

  ```
  journalctl -u systemd-journald -p err --since "24 hours ago" --no-pager > error-ssh-
  24jam.txt
  ```
2. Menghitung total jumlah baris error di dalam berkas:

   ```
   wc -l error-ssh-24jam.txt
   ```
3. Menampilkan 10 pesan error yang paling sering muncul menggunakan pipeline:

  ```
  sort error-ssh-24jam.txt | uniq -c | sort -rn | head -10
  ```
   


## Praktek 10.5 : Konfigurasi SSH Server

1. Periksa konfigurasi SSH saat ini.

```
sudo grep -n "^Port\|^#Port" /etc/ssh/sshd_config
ss -tlnp | grep ssh
```

2. Buat backup dan ubah port SSH.

```
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup.lab12

# ubah port dari 22 ke 2222 (atau port lain yang belum dipakai)
sudo sed -i 's/^#Port 22/Port 2222/' /etc/ssh/sshd_config
```

3. Validasi konfigurasi dan restart layanan.

```
# WAJIB: validasi sintaks sebelum restart
sudo sshd -t
echo "Kode keluar sshd -t: $?"
# kode 0 berarti sintaks valid

# restart layanan
sudo systemctl restart ssh
systemctl status ssh
```
Langkah validasi dengan sshd-t adalah kebiasaan penting. Pada server produksi yang
hanya bisa diakses lewat SSH, kesalahan konfigurasi yang menyebabkan SSH tidak bisa restart
berarti kamu terkunci dari server sendiri.

4. Verifikasi port baru dengan ss.

```
ss -tlnp | grep ssh
# seharusnya menampilkan port 2222, bukan 22

# simpan hasil ke berkas bukti
ss -tlnp | grep ssh > ~/lab-os/chapter10-services/bukti-port-ssh.txt
cat ~/lab-os/chapter10-services/bukti-port-ssh.txt
```

5. Kembalikan port SSH ke 22 setelah praktek.

```
sudo cp /etc/ssh/sshd_config.backup.lab12 /etc/ssh/sshd_config
sudo sshd -t
sudo systemctl restart ssh
ss -tlnp | grep ssh
# harus kembali ke port 22
```
### Tantangan 10.5

Ubah konfigurasi SSH untuk menambahkan dua pengaturan keamanan: PermitRootLogin no (larang login root langsung) dan MaxAuthTries 3 (maksimal tiga kali percobaan). Lakukan dengan urutan yang aman: backup, edit, validasi dengan sshd -t, reload. Verifikasi perubahan dengan grep -E "PermitRoot|MaxAuth" /etc/ssh/sshd_config. Kemudian periksa log SSH untuk memastikan tidak ada error setelah perubahan dengan journalctl -u ssh -n 20. Referensi Bab 2 untuk penggunaan ss dan Bab 9 untuk keamanan pengguna.

### jawaban tantangan 10.5

* kode
```
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup.tantangan
echo -e "\nPermitRootLogin no\nMaxAuthTries 3" | sudo tee -a /etc/ssh/sshd_config
sudo sshd -t
sudo systemctl reload ssh
grep -E "PermitRoot|MaxAuth" /etc/ssh/sshd_config
journalctl -u ssh -n 20 --no-pager
```
* screenshot
<img width="463" height="527" alt="image" src="https://github.com/user-attachments/assets/f20d72ab-4f1c-4233-a4fd-169aaf5dfba1" />




## 1.7 Latihan

Instruksi Umum: Kerjakan seluruh latihan secara mandiri. Catat langkah penting, simpan tangkapan layar bila diperlukan, lalu rangkum hasilnya sebagai dokumentasi pribadi.

### Latihan 10.1 Audit Layanan dan Analisis Boot

Lakukan audit menyeluruh terhadap layanan yang berjalan di sistem.

1. Jalankan systemctl list-units -type=service -state=running dan catat semua layanan aktif. Pilih tiga layanan yang kamu kenal, periksa status masing-masing dengan systemctl status, dan jelaskan fungsinya.

2. Jalankan systemd-analyze blame dan identifikasi lima layanan dengan waktu inisialisasi terlama. Tampilkan hasilnya menggunakan pipeline: systemd-analyze blame | head -5.

3. Jalankan systemctl -failed dan dokumentasikan hasilnya. Jika ada layanan yang gagal, cari tahu penyebabnya dengan journalctl -u nama-layanan -n 30.

### Latihan 10.2 Layanan Kustom dengan Restart Otomatis


1. Buat skrip Bash (referensi Bab 7) bernama monitor-disk.sh yang setiap 30 detik menuliskan penggunaan disk ke berkas log. Gunakan df -h dan date.

2. Buat berkas unit /etc/systemd/system/monitor-disk.service untuk menjalankan skrip tersebut dengan konfigurasi: Restart=always, RestartSec=5s, dan berjalan sebagai pengguna kamu sendiri.

3. Aktifkan dan jalankan layanan. Verifikasi dengan systemctl status dan pastikan log masuk ke journal.

4. Simulasikan crash dengan membunuh proses secara paksa (kill -9), tunggu 10 detik, dan verifikasi bahwa layanan hidup kembali secara otomatis.

5. Bersihkan: nonaktifkan layanan dan hapus berkas unit setelah selesai.

### Latihan 10.3 Investigasi Log dan Keamanan SSH
Analisis log sistem dan tingkatkan keamanan konfigurasi SSH.

1. Gunakan journalctl -b -p err untuk menemukan semua error sejak boot terakhir. Simpan hasilnya ke berkas dan hitung jumlah baris dengan wc -l.

2.  tiga perubahan keamanan pada /etc/ssh/sshd_config: tambahkan PermitRootLogin no, MaxAuthTries 3, dan LoginGraceTime 30. Ikuti alur aman: backup, edit, validasi sshd -t, reload.

3. Setelah reload, verifikasi tiga hal: layanan masih berjalan (systemctl status ssh), port masih mendengarkan (ss -tlnp | grep ssh), dan konfigurasi baru terbaca (grep -E "PermitRoot|MaxAuth|GraceTime" /etc/ssh/sshd_config).

4. Kembalikan konfigurasi SSH ke kondisi semula menggunakan berkas backup.
















 



