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

```
nano ~/praktikum-os/week10-memory/memory-audit.sh
```

```
#!/bin/bash
set-euo pipefail
LAPORAN="memory-report.txt"
{
echo "=== LAPORAN MEMORI SISTEM ==="
date
echo
echo "--- Ringkasan free-h---"
free-h
echo
echo "--- /proc/meminfo---"
cat /proc/meminfo | head-n 20
} > "$LAPORAN"
echo "Laporan disimpan ke: $LAPORAN"
cat "$LAPORAN"
```

Simpan: Ctrl+O→Enter→keluar: Ctrl+X.

```
chmod +x ~/praktikum-os/week10-memory/memory-audit.sh
cd ~/praktikum-os/week10-memory
bash memory-audit.sh
```

### Analisis

1.Hitungpersentasememori tersedia(available / total × 100%).Apakah
sistem dalam kondisi normal?
2. Mengapa buff/cache tidak dihitung sebagai memori yang terpakai dari sudut
pandang ketersediaan untuk aplikasi?
3. Dari /proc/meminfo, apakah SwapTotal lebih besar dari 0? Berapa nilai
SwapFree?

### Jawaban

1. Available: 1696104 kB

   Total: 2015312 kB

Perhitungan: (1696104 / 2015312) × 100% = 84.16% (Atau 84.21% jika menggunakan pembulatan 1.6 / 1.9 GiB dari perintah free).

Jadi, ketersediaan memori sebanyak ~84% menunjukkan mesin virtual memiliki space yang sangat lega, dan tidak memiliki beban pemrosesan(idle).

2. Buff/Cache tidak dihitung sebagai Used (Terpakai) karena ia hanya berisi salinan data sementara yang bisa langsung dibuang oleh sistem kapan saja tanpa menyebabkan error. Karena ruang tersebut bisa diambil alih sewaktu-waktu dengan aman, sistem menganggapnya masih berstatus Available (Tersedia) untuk aplikasi lain

3. SwapTotal lebih besar dari 0 yaitu sebanyak SwapTotal: 2097148 kB, Nilai swap free sebanyak 2097148 kB. Karena RAM masih tersisa 84% sehingga sistem tidak perlu memindahkan data apapun dari RAM ke hardisk(Swap). 

### Tugas 10.2 Identifikasi Proses dengan Memori Tertinggi

Instruksi: Simpan daftar 10 proses pengguna memori terbesar ke file.

```
ps aux --sort=-%mem | head -n 10 > top-memory-process.txt
cat top-memory-process.txt
```

### Analisis
1. Proses apa di urutan pertama? Catat nilai %MEM dan RSS.
2. Konversikan RSS ke MB (bagi 1024). Apakah wajar?
3. Jumlahkan %MEM dari 5 proses teratas. Berapa persen RAM yang mereka
gunakan bersama?

### jawaban
1. Proses pertama : /usr/libexec/fwupd/fwupd (program latar belakang yang bertugas mengecek pembaruan firmware/hardware)
* Nilai %Mem : 2.0
* Nilai RSS :42200 KB

2. Konversi : 42200 KB / 1024 = 41.21 MB
* Layanan sistem seperti fwupd yang mengisi ruang sekitar 41 MB adalah batas yang sangat wajar dan efisien.

3. Hitung : 2.0 + 1.3 + 1.1 + 0.6 + 0.6 = 5.6%
* Lima proses terberat di sistemmu secara kolektif hanya mengkonsumsi 5.6% dari total kapasitas RAM fisik.  ini membuktikan dan mengonfirmasi perhitungan free -h ubuntu dalam keadaan tanpa beban berat (idle) dan sangat sehat.

### Tugas 10.3 Membuat dan Memverifikasi Swap File

Instruksi: Buat swap file khusus tugas sebesar 256 MB dan verifikasi

```
sudo fallocate -l 256M /swapfile-tugas-week10
sudo chmod 600 /swapfile-tugas-week10
sudo mkswap /swapfile-tugas-week10
sudo swapon /swapfile-tugas-week10
```
Verifikasi dan simpan hasil
```
{
echo "=== VERIFIKASI SWAP ==="
swapon --show
echo
free -h
} > swap-check.txt
cat swap-check.txt
```
### Analis
1. Identifikasi kolom NAME, TYPE, SIZE, dan USED pada output swapon–show.
2. Apakah nilai total pada baris Swap di free-h bertambah 256 MB?
3. Mengapa permission 600 penting? Apa risiko jika diatur ke 644?

### Jawaban
1. * NAME: /swapfile-tugas-week10
   * TYPE: file
   * SIZE: 256M
   * USED: 0B
  
2. Ya, bertambah. Sebelumnya total swapnya adalah 2.0Gi menjadi 2.2Gi. 0.2GiB merupakan penambahan file swap sebesar 256 Mib yang baru diaktifkan.

3. * Fungsi permission 600(rw-------) penting karena berfungsi untuk menjamin hanya pemilik file yaitu root(sistem inti operasi) yang memiliki hak eklusif untuk membaca dan menulis pada file tersebut. Tidak ada hak akses bagi yang lainnya.
   * Jika diatur ke 644(rw-r--r--) Resiko yang didapat adalah karena angka 4 diakhir memberikan hak Read kepada pengguna lain. Jika ini dilakukan, orang lain bisa berhasil masuk ke server dan bisa langsung membaca isi file swap tersebut, dan dengan mudah memanen kata sandi atau data rahasia dari aplikasi yang sedang berjalan.

### Tugas 10.4 Analisis System Call dengan strace

Instruksi: Analisis system call yang dipanggil perintah ls.
```
strace -c ls 2> strace-summary.txt
strace ls /etc 2> strace-ls-etc.txt
cat strace-summary.txt
```
### Analisis
1. Sebutkan minimal 5 system call dari strace-summary.txt beserta fungsi
singkatnya.
2. System call mana yang paling sering dipanggil? Mengapa?
3. Apakah ada errors lebih dari 0? Apakah program tetap berjalan normal
meskipun ada kegagalan tersebut?

### Jawaban
1. * execve : Berfungsi untuk memulai eksekusi sebuah program baru. Ini adalah pemanggilan pertama yang dilakukan kernel untuk meluncurkan file binary /usr/bin/ls.
   * mmap: Berfungsi untuk memetakan file atau perangkat ke dalam memori virtual. Biasanya digunakan untuk mengalokasikan memori dan memuat shared library (.so) yang dibutuhkan program.
   * openat: Berfungsi untuk meminta izin membuka sebuah file atau direktori dan mendapatkan File Descriptor (kunci aksesnya).
   * read: Berfungsi untuk membaca isi data dari file atau File Descriptor yang sudah berhasil dibuka sebelumnya.
   * getdents64: Berfungsi khusus untuk membaca entri direktori (mengambil daftar nama-nama file dan folder yang ada di dalam direktori target).
   * close: Berfungsi untuk menutup File Descriptor dan melepaskan akses file setelah program selesai membacanya.
  
2. System call yang paling sering dipanggil adalah nmap, callsnya mencapai 18 kali.

* Alasan :
  Program ls tidak berdiri sendiri; ia sangat bergantung pada pustaka eksternal (shared library) seperti libc untuk bisa berjalan. Sebelum ls bisa melakukan tugas utamanya, sistem operasi harus memanggil mmap berkali-kali secara berulang untuk memotong-motong dan memuat berbagai modul pustaka tersebut dari hardisk ke dalam blok-blok memori RAM.

3. Ya, ada error yang lebih dari 0. Di kolom errors, tercatat ada total 4 buah error: 2 error pada system call statfs dan 2 error pada system call access.

* Program masih berjalan dengan normal.Angka error di strace tidak selalu berarti "program crash". Error tersebut (biasanya kode ENOENT atau No such file) adalah respons wajar kernel ketika program mencoba mengecek keberadaan file konfigurasi opsional yang mungkin sengaja tidak dipasang di sistemmu (seperti file ld.so.preload). Program ls sudah dirancang untuk menerima penolakan tersebut, mengabaikannya, dan menggunakan konfigurasi default agar tetap bisa berjalan dengan lancar.

### Tugas 10.5 Studi Kasus Diagnosa Server Lambat
Skenario: Server terasa lambat. Buat script diagnosa yang menggabungkan semua
pemeriksaan dari bab ini menggunakan fungsi Bash.

```
nano ~/praktikum-os/week10-memory/diagnosa-server.sh
```

```
#!/bin/bash
set -euo pipefail

LAPORAN="diagnosa-server-lambat.txt"
WARN_MEM=false
WARN_SWAP=0

cek_memori() {
echo "---Kondisi Memori---"
free -h
echo
AVAIL_PCT=$(free | awk '/Mem/ {printf "%d", $7/$2*100}
')

if [ "$AVAIL_PCT" -lt 20 ]; then
echo "PERINGATAN: Memori tersedia hanya ${
AVAIL_PCT}%"
WARN_MEM=true
fi

}
cek_swap() {
echo "---Penggunaan Swap---"
swapon --show 2>/dev/null || echo "Tidak ada swap
aktif"
echo
WARN_SWAP=$(free | awk '/Swap/ {print $3}')
if [ "$WARN_SWAP" -gt 0 ]; then
echo "INFO: Swap digunakan (${WARN_SWAP} kB)"
fi
}
cek_proses() {
echo "---10 Proses Memori Tertinggi---"
ps aux --sort=-%mem | head -n 11
echo
}
cek_paging() {
echo "---Aktivitas Paging (5 sampel)---"
vmstat 1 5
echo
}
ringkasan() {
echo "=== RINGKASAN ==="
if [ "$WARN_MEM" = true ]; then
echo "-Memori: KRITIS-perlu tindakan segera"
else
echo "-Memori: normal"
fi
if [ "$WARN_SWAP"-gt 0 ]; then
echo "-Swap: aktif-pantau aktivitas paging"
else
echo "-Swap: tidak digunakan"
fi
}
{
echo "=== LAPORAN DIAGNOSA SERVER ==="
date
echo
cek_memori
cek_swap
cek_proses
cek_paging
ringkasan
} | tee "$LAPORAN"
echo
echo "Laporan disimpan ke: $LAPORAN"

```
```
chmod +x ~/praktikum-os/week10-memory/diagnosa-server.sh
cd ~/praktikum-os/week10-memory
bash diagnosa-server.sh
```
### Analisis
1. Jelaskan peran masing-masing fungsi: cek_memori, cek_swap, cek_proses,
cek_paging, dan ringkasan. Mengapa diagnosa dipecah menjadi fungsi
terpisah?
2. Berdasarkan bagian RINGKASAN, apakah kondisi sistem normal atau kritis?
Jelaskan berdasarkan nilai threshold yang digunakan script.
3. Mengapa script menggunakan tee "$LAPORAN" bukan redirection biasa >
"$LAPORAN"? Apa keuntungannya?
4. Dari output cek_paging, apakah ada aktivitas si atau so? Jika ada, apa
implikasinya terhadap performa server?

### Jawaban

1. * cek_memori: Mengeksekusi perintah (seperti free -h) untuk menangkap status kapasitas RAM fisik (Total, Used, Available).

* cek_swap: Mengeksekusi perintah (seperti swapon --show) untuk mengidentifikasi partisi atau file memori virtual mana yang sedang aktif dan berapa yang terpakai.

* : Memanggil utilitas ps untuk menyortir dan menampilkan antrean program yang paling boros memakan RAM saat ini.

* cek_paging: (Biasanya mengeksekusi vmstat) Bertugas mengawasi lalu lintas perpindahan blok data antara RAM fisik dan Hardisk/Swap.

* ringkasan:  Ia berisi logika kondisional (if-else) yang membandingkan data mentah tadi dengan threshold (ambang batas) untuk memutuskan apakah server sedang sehat atau sekarat.

Dipecah Untuk kemudahan pemeliharaan (maintainability) dan pelacakan bug. Jika suatu saat perintah pengecekan swap error, kamu hanya perlu memperbaiki fungsi cek_swap tanpa takut merusak baris kode untuk mengecek proses. Ini adalah standar penulisan kode yang baik di industri.

2. Kondisi keseluruhan dalam keadaan noermal. AM fisikmu yang Available masih 1.6Gi dari total 1.9Gi. Penggunaan Swap-mu mutlak 0B (0 Byte). Jika scriptmu dipasang threshold batas aman sisa RAM minimal 20%, maka sistem lolos dengan mudah karena sisa memori komputer masih di atas 80%.

3. * Jika  menggunakan > (redirection standar), output diagnosa  hanya akan  ke dalam file di balik terminal. Layar terminalmu akan kosong melompong dan kamu tidak akan tahu apakah prosesnya sudah selesai atau belum.

   * Jika menggunakan tee, perintah  membelah aliran output menjadi dua cabang secara bersamaan (simultaneous). Satu aliran ditampilkan ke layar terminal agar  bisa membacanya secara real-time, sementara cabang satunya lagi direkam ke dalam file $LAPORAN untuk disimpan sebagai arsip bukti praktikum.
  
4. Tidak ada aktivitas si dan so nilainya 0. Jika ada aktivitasnya, maka, Implikasinya sangat fatal bagi performa server. Sistem operasi akan menjadi berubah lambat. Terjadi karena CPU menghabiskan waktu hanya untuk menunggu pertukaran data antara RAM dan hard disk yang sangat lambat.
  
   




 
 




