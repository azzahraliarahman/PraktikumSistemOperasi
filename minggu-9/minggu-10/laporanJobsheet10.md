<h4>Nama : Azzahra Aulia Rahman</h4>
<h4>NIM : 254107020227</h4>
<h4>Kelas : 1H</h4>

# Jobsheet 10 : Manajemen Memori & System Call

## Praktikum 10.1 Melihat Penggunaan Memori

Langkah 1: Jalankan free -h untuk melihat ringkasan RAM dan swap.

```
free-h
```
Langkah 2: Lihat detail memori dari kernel melalui /proc/meminfo.

```
cat /proc/meminfo | head-n 20
```
### Studi Kasus 10.1 Server Lambat karena Memori

Langkah 1: Periksa kondisi memori secara keseluruhan.

```
free-h
```
Langkah 2: Pantau proses secara real-time.

```
top
```

### Hasil Studi Kasus 10.1

* free -h
 <img width="974" height="196" alt="Screenshot 2026-04-29 091626" src="https://github.com/user-attachments/assets/ef510fb1-4df7-4916-8e03-c908f7f26223" />

* top

<img width="896" height="1117" alt="Screenshot 2026-04-29 091714" src="https://github.com/user-attachments/assets/16958cf7-0a9f-4b34-ab3b-d2fae8b517c9" />

## Praktikum 10.2 Mengamati Aktivitas Paging

Langkah 1: Jalankan vmstat dengan interval 1 detik, 5 sampel.

```
vmstat 1 5
```

## Praktikum 10.3 Membuat dan Mengonfigurasi Swap

Langkah 1: Buat file berukuran 512 MB sebagai calon swap.

```
sudo fallocate -l 512M /swapfile-week10
```
Langkah 2: Atur permission file menjadi 600 — hanya root yang boleh membaca
dan menulis.

```
sudo chmod 600 /swapfile-week10
```
Langkah 3: Format file sebagai area swap, lalu aktifkan.

```
sudo mkswap /swapfile-week10
sudo swapon /swapfile-week1
```
Langkah 4: Verifikasi swap aktif. Anda akan melihat entri /swapfile-week10
dengan ukuran 512M, dan nilai total pada baris Swap di free-h bertambah 512M

```
swapon --show
free -h
```
Langkah 5: Periksa nilai swappiness, ubah sementara, dan verifikasi perubahan.

```
cat /proc/sys/vm/swappiness
sudo sysctl vm.swappiness=10
cat /proc/sys/vm/swappiness
```

## Praktikum 10.4 Monitoring Memory

Langkah 1: Ambil snapshot proses diurutkan dari penggunaan memori terbesar.

```
ps aux --sort=-%mem | head
```

Langkah 2: Pantau secara real-time dengan top

```
top
```
## Praktikum 10.5 Script Monitor Memori

```
cd ~/praktikum-os/week10-memory
nano monitor-memori.sh
```
Ketik script berikut:

```
#!/bin/bash
set -euo pipefail

THRESHOLD=20

echo "=== Monitor Memori ==="
date
echo

free -h
echo

AVAIL=$(free | awk '/Mem/ {printf "%d", $7/$2*100}')
if [ "$AVAIL" -lt "$THRESHOLD" ]; then
echo "PERINGATAN: Memori tersedia hanya ${AVAIL}%!"
else
echo "Status: Memori tersedia ${AVAIL}% (normal)"
fi
echo
echo "--- 5 Proses Memori Tertinggi---"
ps aux --sort=-%mem | head -n 6 | tail -n 5
```

## Studi Kasus 10.2 Gagal Akses File

Skenario: Program tidak dapat membaca file konfigurasi. Penyebab umum: file
tidak ada, path salah, atau permission tidak sesuai. Kita akan mensimulasikan
kondisi ini dan mengamati pesan error yang dihasilkan.

Langkah 1: Buat direktori dan file konfigurasi contoh.

```
mkdir-p ~/praktikum-os/week10-memory/syscall-case
cd ~/praktikum-os/week10-memory/syscall-case
echo "PORT=8080" > app.conf
ls -l app.conf
cat app.conf
```
Langkah 2: Simulasikan permission bermasalah
```
chmod 000 app.conf
cat app.conf
```
Langkah 3: Kembalikan permission dan verifikasi
```
chmod 644 app.conf
cat app.conf
```

### Analisis:
1. Mengapa cat menghasilkan Permission denied setelah chmod 000? System
call apa yang gagal?
2. Apa perbedaan pesan error Permission denied vs No such file or directory?
Coba rm app.conf lalu cat app.conf untuk melihat perbedaannya.
3. Permission 644 berarti apa untuk owner, group, dan others?

### Jawaban analisis
1. Karena chmod 000 melucuti seliruh permission bit secara absolut menjadi --- --- ---, tidak ada hak akses apa pun untuk siapa pun, bahkan usernya sendiri. System call yang gagal adalah open() atau openat().

   
2. Permission denied muncul karena file tersebut terbukti eksis secara fisik pada storage. Sistem file menemukan direktori entri(dentry) yang valid yang merujuk pada sebuah inocode(struktur data file). Sedangkan,  No such file or directory , saat menjalankan rm app.conf, artinya menghapus tautan(unlink) antara nama "app.conf" dengan inocodenya di dalam harddisk. Ketika menjalankan cat app.conf, system call open() app.conf idak merujuk data manapun. karena file tersebut telah dihapus.

3. 644 merupakan angka desimal representasi oktal dari mask biner (bitmask) 110 100 100.

* Digit pertama : 6 (pemilik)
  4(read) + 2(write) = 6
  - Secara simbolik menjadi rw-. Pemilik file memiliki hak otonom penuh untuk melihat isi file(read) dan memodifikasi atau menghapus isinya(write). tetapi file tersebut tidak executable.

  * digit kedua : 4 (Group)
    4(read)
   - Secara simbolik menjadi r--. Pengguna lain yang tergabung dalam group kepemilikan file tersebut hanya mode read. hanya bisa membaca tanpa hak untuk mengubah satu byte pun.

  * digit ketiga : 4 (Pengguna lain)
    4(read)
   - secara simbolik mejadi r--, ini adalah lapisan terluar. Siapapun pengguna di dalam sistem operasi yang bukan pemilik dan bukan anggota group file tersebut hanya bisa mode read.





