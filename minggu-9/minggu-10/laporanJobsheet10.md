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

 ## Praktikum 10.6 Mengamati System Call dengan strace

Langkah 1: Lihat 30 baris pertama system call dari perintah ls.

```
strace ls 2>&1 | head -n 30
```
Langkah 2: Lihat ringkasan statistik dan bandingkan dua direktori berbeda

```
strace -c ls
strace -c ls /etc 2>&1 | tail -5
```

### Analisis
1. Dari output Langkah 1, identifikasi minimal 4 system call berbeda. Jelaskan
fungsi singkat masing-masing berdasarkan argumen yang terlihat.
2. Dari ringkasan strace-c, system call mana yang paling sering dipanggil?
Mengapa?
3. Apakah ada system call dengan errors lebih dari 0? Apakah itu berarti
program bermasalah, ataukah bagian normal dari logika program?
4. Apakah jumlah system call berbeda antara ls dan ls /etc? Faktor apa yang
menyebabkan perbedaan tersebut?

### Jawaban Analisis

1. Identifikasi 4 System Call (Berdasarkan gambar head -n 30)

Dari log eksekusi, anatomi awal program ls sangat bergantung pada proses dynamic linking. Berikut 4 system call yang bisa langsung diidentifikasi dari argumennya:

* execve("/usr/bin/ls", ["ls"], ...)
    Ini adalah pemicu utamanya. Program meminta kernel untuk mengganti proses saat ini dengan program baru, yaitu mengeksekusi file binary yang berada di path /usr/bin/ls.

* openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|... )
    Meminta izin kernel untuk membuka file cache shared library sistem. Argumen O_RDONLY menegaskan bahwa file ini dibuka murni hanya untuk dibaca, tidak untuk dimodifikasi. Kernel merespons dengan memberikan File Descriptor 3.

* fstat(3, {st_mode=S_IFREG|0644, st_size=22439...})
    Meminta metadata atau informasi detail dari file yang sedang dipegang oleh File Descriptor 3. Argumen outputnya menunjukkan kernel mengembalikan data berupa ukuran file (22439 byte) dan hak aksesnya (0644).

* mmap(NULL, 8192, PROT_READ|PROT_WRITE, ...)
    Meminta kernel untuk memetakan blok memori virtual (alokasi memori). Argumen 8192 menunjukkan sistem meminta ruang sebesar 8 Kilobyte di RAM dengan hak akses untuk dibaca (PROT_READ) dan ditulisi (PROT_WRITE).

2. System Call Paling Sering Dipanggil

system call yang paling sering dipanggil di awal adalah mmap, mprotect, read, atau close. Alasannya: sebelum ls bisa mencetak nama file, ia harus memuat banyak sekali modul shared library (seperti libc.so.6) dari hardisk ke dalam memori secara sepotong-sepotong.
3. Makna Error > 0 pada System Call

Ya, ada error. Pada baris keempat gambar kedua, terekam jelas:
access("/etc/ld.so.preload", R_OK) = -1 ENOENT (No such file or directory)
Di ringkasan strace -c juga tercatat ada 4 hingga 5 akumulasi error.

Faktanya, ini bukan berarti program ls bermasalah atau gagal. Ini adalah bagian normal dari mekanisme fallback logika program. Saat pertama kali berjalan, sistem mencoba mengecek (menggunakan access()) apakah ada file konfigurasi opsional bernama ld.so.preload yang ingin dimuat oleh admin. Karena file itu memang default-nya tidak ada di Ubuntu, kernel melempar error ENOENT. Program ls dirancang untuk mengantisipasi penolakan ini, mengabaikannya dengan tenang, lalu melangkah ke instruksi pemuatan library standar berikutnya.
4. Perbedaan Jumlah System Call: ls vs ls /etc

Untuk membaca isi sebuah direktori, ls harus memanggil system call bernama getdents64 (Get Directory Entries). Jika  menjalankan ls di direktori ~ yang hanya berisi 5 file, kernel hanya butuh sedikit siklus memori. Namun jika kamu menjalankan ls /etc, di mana folder /etc/ biasanya menampung ratusan file konfigurasi, program harus melakukan perulangan system call getdents64 (dan mungkin lstat untuk mengecek warna/tipe masing-masing file) berkali-kali lipat lebih banyak sampai seluruh daftar file habis dibaca.

## 1.6 Tugas Praktikum

Instruksi Umum: Kerjakan seluruh tugas pada direktori berikut.
```
mkdir -p ~/praktikum-os/week10-memory
cd ~/praktikum-os/week10-memory
```

### Tugas 10.1 Audit Penggunaan Memori Sistem

Instruksi:Buat script memory-audit.sh yang menghasilkan laporan kondisi mem
ori sistem secara otomatis.

 





