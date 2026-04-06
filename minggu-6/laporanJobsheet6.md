# Laporan Praktikum Jobsheet 6 Manajemen Proses

<h4>Nama : Azzahra Aulia Rahman<h4>
<h4>NIM : 254107020227
<h4>Kelas : TI-1H<h4>

## Praktikum 6.1 — Melihat Proses dan Thread

1. Tampilkan semua proses yang berjalan:

```
ps aux
```
2. Tampilkan proses beserta thread-nya, dapat dilihat pada kolom LWP (Light Weight Process ID):

```
ps aux-L
```
3. Lihat PID shell aktif dan detail prosesnya

```
echo $$
ps -p $$ -f
```
4. Lihat hierarki proses secara visual:

```
pstree -p
```
### Pertanyaan Latihan 6.1
Jalankan ps aux dan amati outputnya:
1. Berapa total proses yang berjalan? Proses apa yang memiliki PID
terkecil?

2. Jalankan pstree-p dan temukan proses bash Anda. Proses apa yang
menjadi induk (PPID) dari bash tersebut?

3. Bandingkan output ps aux dan ps aux-L. Apa perbedaan yang Anda
lihat?

### Jawaban Latihan 6.1
1. Total proses yang ada, ada 101 baris, tapi yang sebenarnya ada 100 baris karena baris pertama adalah header. Proses yang memiliki PID terkecil yaitu 1 adalah /sbin/init.
   
2. Induk (PPID) dari proses bash  adalah login dengan nomor PID 994.

3. ps aux menampilkan daftar proses. Sedangkan ps aux -L Menampilkan daftar thread. Dan ada kolom tambahan LWP (Light Weight Process / ID Thread) dan NLWP (Jumlah total thread).

## Praktikum 6.2 — Mengamati Siklus Hidup Proses

1. Buat proses di background dan amati kondisinya:

```
sleep 60 &
ps aux | grep sleep
```
2. Amati perubahan exit code dari perintah yang berhasil dan gagal:

```
ls /tmp
echo "Sukses: $?"
ls /direktori-tidak-ada
echo "Gagal: $?"
```
### Pertanyaan Latihan 6.2

1. Jalankan sleep 120 & dan amati kolom STAT pada ps aux. Kondisi
apa yang ditampilkan? Mengapa proses sleep berada di kondisi tersebut?

2. Jalankan beberapa perintah yang berhasil dan yang gagal, lalu catat exit
code masing-masing. Pola apa yang Anda temukan?

### Jawaban Latihan 6.2
1. Kodisi yang ditampilkan adalah **S**_ (Interruble Sleep)_. Proses sleep dalam keadaan tersebut karena proses sleep tidak membutuhkan tenaga processor(CPU). Proses ini menunggu 120 detik.

2. Perintah berhasil : ls /tmp menghasilkan exit code 0
   Perintah gagal : ls /direktori-tidak-ada
   - Pola yang ditemukan :
     * Angka 0 menunjukkan kesuksesan tanpa ada kesalahan
     * Angka bukan 0 (1 ++) : Ex : 2, menunjukkan bahwa perintah gagal dieksekusi.
    
## Praktikum 6.3 — Mengatur Prioritas Proses

1. Jalankan proses dengan prioritas rendah:
   
```
nice -n 10 sleep 300 &
```
Kode 1.8: Menjalankan proses dengan nice +10

2. Verifikasi nilai nice pada kolom NI:
   
```
ps aux | grep sleep
```
Kode 1.9: Melihat nilai nice

3. Ubah nilai nice proses yang sudah berjalan:

```
renice -n 15 -p <PID>
ps -p <PID> -o pid,ni,cmd
```
Kode 1.10: Mengubah nice dengan renice

4. Bersihkan proses percobaan:
   
```
kill %1
```
Kode 1.11: Menghentikan proses percobaan

### Pertanyaan Latihan 6.3

1. Jalankan nice -n 5 sleep 200 & dan verifikasi nilai NI-nya dengan
ps.

2. Ubah nilai nice menjadi 10 menggunakan renice, lalu verifikasi kembali.

3. Coba ubah nilai nice menjadi -5 tanpa sudo. Apa yang terjadi? Mengapa
Linux membatasi hal ini untuk user biasa?

### Jawaban Latihan 6.3
1. * Kolom NI:  Terdapat angka 5. Ini adalah Nice Value nya.

   * Kolom STAT: Terdapat huruf N. Dalam kode status Linux, N adalah Low-     priority ( nilai Nice positif).

   * Artinya Sistem berhasil menerima instruksi untuk menaruh proses sleep ini di antrean bawah CPU.
   
2. Terminal akan mencetak pesan old priority 5, new priority 10. Saat dicek dengan ps, angka di kolom NI terbukti sudah berubah dari 5 menjadi 10.
   
3. Perintah gagal dan terminal memunculkan pesan error: Permisssion denied.

 **User biasa dibatasi karena :**
 
*Pencegahan Monopoli CPU: Nilai NI negatif berarti prioritas tinggi. Jika user biasa diizinkan memberi nilai negatif, mereka bisa menyedot seluruh kapasitas CPU untuk program mereka sendiri. Akibatnya, server akan hang karena proses sistem lain tidak mendapat jatah CPU.

*Keamanan Dasar: Ini adalah mekanisme anti-DoS (Denial of Service). Aturan Kernel Linux menetapkan bahwa user biasa hanya boleh mengalah (menaikkan nilai NI ke angka positif), tapi dilarang menyerobot antrean CPU (angka negatif).

*Otoritas Root: Hanya superuser (menggunakan sudo) yang boleh menggunakan prioritas negatif, karena administrator dianggap bertanggung jawab penuh atas stabilitas server.

## Praktikum 6.4 — Mengirim Sinyal ke Proses

1. Buat proses percobaan:
```
sleep 500 &
sleep 600 &
sleep 700 &
ps aux | grep -v grep | grep sleep
```
Kode 1.13: Membuat proses percobaan

2. Hentikan satu proses dengan SIGTERM dan verifikasi:
```
kill <PID-sleep-500>
ps aux | grep -v grep | grep sleep
```
Kode 1.14: Menghentikan proses dengan SIGTERM

3. Jeda dan lanjutkan proses dengan SIGSTOP/SIGCONT:
```
kill-SIGSTOP <PID-sleep-600>
ps aux | grep sleep
# amati kolom STAT: berubah
menjadi T
kill-SIGCONT <PID-sleep-600>
ps aux | grep sleep
# STAT kembali ke S
```
Kode 1.15: Menjeda dan melanjutkan proses

4. Hentikan semua proses sleep sekaligus:
```
pkill sleep
```
Kode 1.16: Menghentikan semua proses sleep

### Pertanyaan Latihan 6.4
1. Jalankan sleep 400 &, kirim SIGSTOP, dan amati perubahan kolom
STAT. Kondisi apa yang muncul?
2. Kirim SIGCONT dan verifikasi proses kembali berjalan.
3. Hentikan proses dengan SIGTERM lalu verifikasi sudah tidak ada. Kapan
Anda memilih SIGKILL daripada SIGTERM?

### Jawaban Latihan 6.4
1. Perubahan STAT yang berubah yang tadinya STATnya adalah S (stop) menjadi T (stopped).
2. ketika SIGCONT berjalan seharusnya STAT berubah menjadi S kembali. Dan benar, STATE berubah kembali menjadi S setelah tadi di stop.
3. Sudah dihentikan da veriikasi sudah tidak ada. Memilih SIGKILL daripada SIGTERM ketika SIGTERM sudah dicoba tapi tidak bisa atau gagal. Barulah kita berhak memakai SIGKILL (kill -9 [PID]).

## Praktikum 6.5 — Manajemen Job Foreground dan Background

1. Jalankan tiga job di background:
```
sleep 200 &
sleep 300 &
sleep 400 &
jobs
```
Kode 1.17: Membuat tiga job di background

2. Bawa job pertama ke foreground, jeda, lalu kembalikan ke background:
```
fg %1
# Tekan Ctrl+Z untuk menjeda
bg %1
jobs
```
Kode 1.18: Memindahkan job antar foreground-background

3. Hentikan semua job:
```
kill %1 %2 %3
jobs
```
Kode 1.19: Menghentikan semua job 

### Pertanyaan Latihan 6.5

1. Jalankan top di foreground. Apa yang terjadi di terminal?
2. Tekan Ctrl+Z dancek statusnya dengan jobs. Kondisi apa yang
ditampilkan?
3. Pindahkan ke background dengan bg. Apakah top dapat berjalan dengan
baik di background? Mengapa?
4. Kembalikan ke foreground dengan fg, lalu keluar dengan q

### Jawaban Latihan 6.5
1. Terminal akan langsung diambil alih oleh antarmuka top. Layar akan menampilkan tabel pemantauan sistem (Penggunaan CPU, RAM, dan daftar proses) yang terus berubah dan diperbarui secara real-time. Selama top menguasai foreground, terminal tidak bisa diketikkan perintah.
2. saat menekan CTRL + Z, proses top dibekukan paksa oleh sistem dan terminal bisa diketik saat mengecek dengan jobs , kondisi yang akan ditampilkan adalah status stopped.
3. Tidak dapat berjalan. Karena top adalah program interaktif yang secara mutlak membutuhkan akses layar utama untuk menampilkan data. Aturan keamanan linux melarang program di background untuk mencetak teks dan mengacaukan layar utama, Sehingga proses tersebut langsung dihentikan lagi dengan status Stopped.
4. Tabel pemnatauan top akan kembali berjalan secara real-time. Ketika menekan tombol q di keyboard , proses akan mati secara normal.

## Praktikum 6.6 — Pemantauan Proses

1. Temukan proses dengan penggunaan CPU dan memori tertinggi:
```
ps aux--sort=-%cpu | head-10
ps aux--sort=-%mem | head-10
```
Kode 1.22: Proses dengan resource tertinggi

2. Jalankan top dan eksplorasi shortcut-nya:
top
```
# Tekan M, P, 1, u secara bergantian
# Tekan q untuk keluar
```
Kode 1.23: Eksplorasi top

3. Instal dan jalankan htop:
```
sudo apt install-y htop
htop
# Tekan F6 untuk pilih kolom pengurutan
# Tekan F10 atau q untuk keluar
```

Kode 1.24: Menggunakan htop

### Pertanyaan Latihan 6.6

1. Gunakan_ ps aux –sort=%mem_ untuk menemukan proses yang menggu
nakan memori paling banyak di VM Anda. Proses apa itu?

2. Di dalam top, tekan 1 .Apayang berubah pada tampilan?
Mengapa informasi ini berguna?

3. Di dalam _htop_, navigasikan ke proses _sshd_ menggunakan tombol panah.
Tekan F9 danamati opsi sinyal yang tersedia.

### Jawaban Latihan 6.6

1. Memori yang paling banyak adalah 1.3 prosesnya adalah /sbin/multipathd -d -s

2. Yang berubah pada tampilan %Cpu (s) terpecah menampilkan setiap core CPU yang ada di VM (ex : %Cpu0, %Cpu1, %Cpu2, sdt). Informasi berguna agar kita tidak hanya melihat rata2 CPU secara umum, tapi secara detail. (ex : kamu bisa melihat Core 0 bekerja 100%, sedangkan Core 1, 2, 3).

3. Sinyal yang tersedia :

*15 SIGTERM: Sinyal bawaan (default) untuk meminta proses berhenti secara normal dan aman.

*9 SIGKILL: Sinyal paksa untuk membunuh proses seketika tanpa ampun (digunakan jika proses hang).

*2 SIGINT: Sinyal interupsi (setara dengan menekan Ctrl+C).

*1 SIGHUP: Sinyal hangup (biasanya untuk menyuruh program me-restart konfigurasi).

## 1.8 Latihan

### Pertanyaan Latihan 6.A
Eksplorasi Proses Sistem

1. Jalankan ps aux –forest dan temukan proses dengan PID 1. Apa
nama dan fungsi proses tersebut dalam sistem Linux modern?

2. Hitung berapa proses yang dimiliki oleh user root dan berapa yang
dimiliki oleh user Anda. Mengapa root memiliki lebih banyak proses?

3. Temukan semua proses yang berada dalam kondisi S. Mengapa sebagian
besar proses di sistem berada dalam kondisi ini?

### Jawaban Latihan 6.A

1. Nama proses dengan PID 1 adalah systemd. fungsinya adalah sebagai induk dari segala proses, yaitu program level pengguna pertamayang dieksekusimutlak oleh kernel linux saat sistem baru menyala (booting).

Tugas utamanya :
1. Menjalankan dan mengelola seluruh layanan sistem (services/daemons).
2. Menjadi nenek moyang (akar dari pohon direktori --forest) bagi semua proses lain yang berjalan di OS.
3. Mengadopsi proses  orphan process yang ditinggal mati oleh proses pembuatnya agar tidak menjadi zombie.

2. proses yang dimiliki oleh root ada 85 (_ps -U root | wc -l_), proses yang dimiliki adalah user adalah 6(_ps -U zalia | wc -l_). root lebih banyak proses karena ia memikul seluruh beban infrastruktur  sistem operasi, sedangkan user hanya menjalankan proses didalam user space yang sangat sempit.

3. Sebagian besar proses berada dalam kondisi S karena secara teknis program lebih banyak menghabiskan waktu untuk menunggu interaksi daripada melakukan pemprosesan data untuk efisiensi energi dan CPU.

### Pertanyaan Latihan 6.B
Simulasi Manajemen Job
1. Jalankan tiga perintah sleep dengan durasi 100, 200, dan 300 detik di
background. Verifikasi ketiganya dengan jobs.
2. Bawa job kedua ke foreground, jeda dengan Ctrl+Z , lalu kembalikan
ke background dengan bg.
3. Hentikan job pertama dengan kill %1. Tampilkan kembali daftar job.
Berapa job yang tersisa?

### Jawaban Latihan 6.B

### Pertanyaan latihan 6.C
Prioritas dan Sinyal
1. Jalankan dua proses sleep: satu dengan nice +5 dan satu dengan nice
+15. Verifikasi nilai NI keduanya dengan ps.
2. Gunakan renice untuk mengubah nice proses pertama menjadi +10.
Proses mana yang kini lebih diprioritaskan scheduler?
3. Kirim SIGSTOP ke salah satu proses, verifikasi kondisi T-nya, lalu kirim
SIGCONT. Akhiri semua proses percobaan dengan pkill sleep.

### jawaban Latihan 6.C










