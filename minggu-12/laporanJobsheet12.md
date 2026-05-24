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
prosesutama,waktumulai,penggunaanmemori,danbeberapabarislogterbaru. Iniadalah
perintahpertamayangharusdijalankanketikaadamasalahlayanan

2. 


 



