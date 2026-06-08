<h4> Nama : Azzahra Aulia Rahman </h4>
<h4> NIM  : 254107020227</h4>
<h4> Kelas: 1H

# Jobsheet 12 : 1 Backup dan Pemulihan Sistem

## Praktek 12.1: Rencanakan Strategi Backup

1. Buat direktori struktur data simulasi yang akan di-backup:

```
# Berpindah ke direktori kerja bab 12 (pastikan folder chapter12-backup sudah ada)
cd ~/lab-os/chapter12-backup

# Membuat direktori 'data-sumber' dan 3 sub-direktori sekaligus (dokumen, konfigurasi, log) tanpa error jika sudah ada (-p)
mkdir -p data-sumber/{dokumen,konfigurasi,log}

# Membuat file laporan.txt dan mengisinya dengan baris teks pertama (tanda > menimpa/membuat baru)
echo "laporan-keuangan-2026.txt" > data-sumber/dokumen/laporan.txt

# Menambahkan baris teks kedua ke file laporan.txt (tanda >> menambahkan ke baris bawahnya, bukan menimpa)
echo "catatan-penting.txt" >> data-sumber/dokumen/laporan.txt

# Membuat file konfigurasi app.conf dan mengisi pengaturan port
echo "app_port=8080" > data-sumber/konfigurasi/app.conf

# Menambahkan pengaturan host ke baris selanjutnya di app.conf
echo "db_host=localhost" >> data-sumber/konfigurasi/app.conf

# Memulai perulangan (loop) bash dari angka 1 sampai 20
for i in $(seq 1 20); do
    # Menuliskan waktu saat ini (date) beserta nomor urut log ke dalam file app.log sebanyak 20 kali
    echo "$(date) - log entry $i" >> data-sumber/log/app.log
done

# Mengecek ukuran penyimpanan (disk usage) dari setiap folder di dalam data-sumber dengan format yang mudah dibaca (-h)
du -sh data-sumber/*/

# Mencari seluruh file (-type f) di dalam data-sumber, lalu menghitung total jumlah filenya (wc -l)
find data-sumber -type f | wc -l
```
2. Buat dokumen rencana backup menggunakan heredoc:

```
# Menggunakan fitur 'heredoc' (<< 'EOF') untuk menulis teks panjang berbaris-baris langsung ke dalam file.
# Semua teks di bawah perintah ini, sampai sistem menemukan tulisan 'EOF' sendirian di baris akhir, akan dimasukkan ke dalam file rencana-backup.txt
cat > rencana-backup.txt << 'EOF'
====================================================
RENCANA BACKUP SISTEM LAB LINUX
====================================================
Tanggal Dibuat : (isi tanggal kamu)
Nama Sistem    : Lab Ubuntu Server 22.04
====================================================

INVENTARIS DATA
---------------
data-sumber/dokumen/    : laporan dan catatan, KRITIS
data-sumber/konfigurasi/: konfigurasi aplikasi, KRITIS
data-sumber/log/        : log sistem, TIDAK KRITIS

TARGET METRIK
-------------
RPO : 24 jam (data boleh hilang maksimal 1 hari)
RTO : 2 jam  (sistem harus pulih dalam 2 jam)

STRATEGI
--------
Full backup    : setiap Minggu malam (dokumen + konfigurasi)
Incremental    : Senin-Sabtu malam (dokumen + konfigurasi)
Log            : TIDAK dibackup (dapat diregenerasi)
Penyimpanan    : lokal ~/lab-os/chapter12-backup/arsip/

ATURAN 3-2-1
------------
Salinan 1 : direktori data-sumber (data asli)
Salinan 2 : direktori arsip/ lokal (backup lokal)
Salinan 3 : server remote / cloud storage (offsite)
====================================================
EOF

# Membaca dan menampilkan kembali isi file di terminal untuk memastikan tidak ada teks yang terlewat
cat rencana-backup.txt
```
3. Hitung estimasi kebutuhan ruang backup untuk 30 hari:

```
# Menghitung total ukuran dalam byte (-b) dan menjumlahkan baris outputnya menggunakan awk, lalu menyimpannya ke variabel DATA_SIZE
DATA_SIZE=$(du -sb data-sumber/dokumen/ data-sumber/konfigurasi/ \
    | awk '{sum+=$1} END{print sum}')

# Menampilkan total ukuran data asli yang bersifat kritis
echo "Ukuran data kritis: $DATA_SIZE byte"

# Menghitung estimasi jika full backup dilakukan seminggu sekali (4 kali sebulan)
echo "Full backup x4/bulan: $((DATA_SIZE * 4)) byte"

# Menghitung estimasi incremental backup harian (asumsi perubahan 10% per hari, selama 24 hari kerja)
echo "Incremental est. 10% x24/bulan: $((DATA_SIZE * 24 / 10)) byte"

# Menjumlahkan kebutuhan ruang dari strategi Full (4x) + Incremental (24x)
echo "Total estimasi 30 hari: $((DATA_SIZE * 4 + DATA_SIZE * 24 / 10)) byte"
```
### Tantangan 12.1

Buat file teks berisi rencana backup yang lebih lengkap menggunakan teknik heredoc dari Bab 3. Rencana harus mencantumkan: jadwal harian dan mingguan, estimasi ruang untuk setiap jenis backup, lokasi penyimpanan, dan prosedur pengujian restore. Simpan ke file rencana-lengkap.txt. Kemudian gunakan teknik redirection dan pipeline dari Bab 3 untuk menghitung berapa baris rencana yang kamu tulis.

### Jawaban Tantangan 10.1
<img width="477" height="43" alt="image" src="https://github.com/user-attachments/assets/934566da-64b6-49a9-bdb1-39268272d44b" />


## Praktek 12.2: Sinkronisasi Direktori dengan rsync

1. Jalankan sinkronisasi pertama:

```
# Berpindah ke direktori kerja bab 12
cd ~/lab-os/chapter12-backup

# 1. DRY RUN (Simulasi): Menambahkan opsi -n (dry-run) bersama -av.
# Perintah ini HANYA menampilkan apa yang akan terjadi di layar, tanpa memindahkan 1 byte data pun.
rsync -avn data-sumber/ arsip-rsync/

# 2. EKSEKUSI NYATA: Tanpa opsi -n. 
# Barulah di titik ini proses sinkronisasi dan penyalinan data benar-benar terjadi.
rsync -av data-sumber/ arsip-rsync/

# Memeriksa isi direktori tujuan untuk membuktikan data sudah tersalin
ls -la arsip-rsync/
```
Amati: apa perbedaan output dry run (-n) dengan eksekusi sebenarnya?

2. Tambahkan file baru,hapus satu file, lalu amati efek --delete:

```
# Membuat file baru di dalam direktori sumber
echo "file baru penting" > data-sumber/dokumen/baru.txt

# Menghapus salah satu file dari direktori sumber secara permanen
rm data-sumber/log/app.log

# Melakukan sinkronisasi ulang dengan opsi tambahan --delete
rsync -av --delete data-sumber/ arsip-rsync/

# Memverifikasi apakah file baru berhasil masuk ke arsip
ls arsip-rsync/dokumen/

# Memverifikasi apakah file yang dihapus di sumber ikut lenyap di arsip
ls arsip-rsync/log/
```
Amati: apakah app.log ikut terhapus dari tujuan? Apakah baru.txt muncul di tujuan?

3. Buat snapshot pertama dan kedua menggunakan --link-dest:

```
# Menyiapkan folder untuk snapshot pertama dan melakukan backup awal
mkdir -p snapshots/snap-1
rsync -av data-sumber/ snapshots/snap-1/

# Memodifikasi isi file di sumber untuk mensimulasikan adanya perubahan data seiring waktu
echo "isi baru setelah snap-1" > data-sumber/dokumen/baru.txt

# Menyiapkan folder untuk snapshot kedua
mkdir -p snapshots/snap-2

# Melakukan backup incremental cerdas menggunakan --link-dest.
# Perintah ini menyuruh rsync membandingkan data dengan snap-1.
rsync -av --link-dest=snapshots/snap-1 \
    data-sumber/ snapshots/snap-2/
```
4. Verifikasi hard link dengan membandingkan inode:

```
# Menampilkan detail file tersembunyi (-a), format panjang (-l), dan nomor inode (-i) pada direktori snap-1
ls -lai snapshots/snap-1/dokumen/

# Menampilkan informasi yang sama persis untuk direktori snap-2
ls -lai snapshots/snap-2/dokumen/

# Menampilkan estimasi ukuran masing-masing folder snapshot
du -sh snapshots/snap-1/ snapshots/snap-2/

# Menampilkan total ukuran sebenarnya dari seluruh folder snapshots (membuktikan efisiensi hard link)
du -sh snapshots/
```
5. Bersihkan snapshot setelah praktek:

```
# Menghapus paksa secara rekursif (-rf) direktori arsip dan snapshots beserta seluruh isinya
rm -rf arsip-rsync/ snapshots/
```

### Tantangan 12.2

Buat skrip Bash (mengacu ke Bab 7) bernama rsync-backup.shdidirektori lab yang menjalankan
rsync dari direktori data-sumber/ ke direktori bernama sesuai tanggal hari ini dalam format
YYYY-MM-DD. Gunakan tee dari Bab 3 untuk menulis output rsync ke layar sekaligus ke file log bernama backup-TANGGAL.log. Jadikan skrip ini executable dengan chmod +x dan jalankan
untuk memverifikasi hasilnya.

### Jawaban tantangan 12.2
<img width="478" height="232" alt="image" src="https://github.com/user-attachments/assets/675955b5-0161-4d00-9c11-5e920537a257" />

## Praktek 12.3 : Buat Arsip Backup dan Verifikasi Integritasnya

Tujuan praktek ini adalah membuat arsip backup penuh dan incremental, kemudian memverifikasi integritas arsip menggunakan checksum

1. Buat arsip full backup dan simpan checksumnya:

```
cd ~/lab-os/chapter12-backup
mkdir -p arsip-tar
tar -czf arsip-tar/full-$(date +%F).tar.gz data-sumber/
ls -lh arsip-tar/
md5sum arsip-tar/full-$(date +%F).tar.gz > arsip-tar/full-$(date +%F).md5
sha256sum arsip-tar/full-$(date +%F).tar.gz > arsip-tar/full-$(date +%F).sha256
cat arsip-tar/full-$(date +%F).md5
```
2. Periksa isi arsip dan coba ekstrak satu file:

```
# Menampilkan seluruh daftar isi file di dalam arsip tar.gz tanpa mengekstraknya (-t)
tar -tzf arsip-tar/full-$(date +%F).tar.gz

# Menampilkan isi arsip, tetapi difilter hanya untuk file yang mengandung kata ".conf"
# Membuat direktori sementara untuk uji coba ekstrak
mkdir -p /tmp/tar-coba

# Mengekstrak (-x) spesifik SATU file saja (app.conf) dan mengarahkannya (-C) ke folder /tmp/tar-coba/
tar -xzf arsip-tar/full-$(date +%F).tar.gz -C /tmp/tar-coba \
    data-sumber/konfigurasi/app.conf

# Membaca isi file yang baru saja berhasil diekstrak
cat /tmp/tar-coba/data-sumber/konfigurasi/app.conf

# Menghapus direktori uji coba beserta isinya untuk membersihkan sistem
rm -rf /tmp/tar-coba
```
3. Verifikasi integritas arsip menggunakan checksum:

```
# Mengecek (-c) kecocokan file arsip dengan nilai hash MD5 yang disimpan sebelumnya
md5sum -c arsip-tar/full-$(date +%F).md5

# Mengecek kecocokan menggunakan algoritma SHA-256 (lebih aman dari MD5)
sha256sum -c arsip-tar/full-$(date +%F).sha256

# Menguji baca seluruh isi arsip dan membuang output layarnya ke /dev/null (lubang hitam Linux).
# Jika pengujian sukses (kode keluar 0), cetak "Arsip VALID". Jika gagal, cetak "Arsip RUSAK".
tar -tzf arsip-tar/full-$(date +%F).tar.gz > /dev/null && \
    echo "Arsip VALID" || echo "Arsip RUSAK"
```
apa arti pesan OK pada output md5sum -c?

4. Buat arsip incremental setelah menambahkan file baru:

```
# Membuat full backup awal TAPI sekaligus merekam metadata kondisi file ke dalam file snapshot.snar
tar -czf arsip-tar/full-snap-$(date +%F).tar.gz --listed-incremental=arsip-tar/snapshot.snar \
    data-sumber/

# Mensimulasikan adanya file baru dan modifikasi file lama di hari berikutnya
echo "file ditambahkan setelah full backup" > data-sumber/dokumen/tambahan.txt
echo "konfigurasi baru" >> data-sumber/konfigurasi/app.conf

# Membuat arsip TAHAP KEDUA (incremental). Perintah ini akan membaca snapshot.snar
# untuk mengetahui file mana saja yang berubah, lalu HANYA membungkus file yang berubah tersebut.
tar -czf arsip-tar/incr-$(date +%F-%H%M).tar.gz --listed-incremental=arsip-tar/snapshot.snar \
    data-sumber/

# Mengecek dan membandingkan ukuran file arsip full vs incremental
ls -lh arsip-tar/*.tar.gz
```
 ### Tantangan

Buat arsip incremental sesi kedua. Tambahkan beberapa file baru ke data-sumber/dokumen/ dan
ubah isi app.conf. Kemudian jalankan tar dengan file snapshot yang sama untuk menghasilkan
incremental backup kedua. Bandingkan ukuran ketiga arsip (full, incr-1, incr-2). Kemudian
verifikasi isi setiap arsip menggunakan tar-tzf untuk memastikan setiap arsip hanya mengandung file yang sesuai dengan jenisnya 

### Jawaban Tantangan 12.3





## Praktek 12.4: Jadwalkan Skrip Backup Otomatis

Tujuan praktek ini adalah membuat skrip backup, mengujinya secara manual, mendaftarkan ke crontab, dan memverifikasi eksekusi otomatisnya

1. Buat skrip backup yang menggunakan rsync dengan logging:

```
cd ~/lab-os/chapter12-backup

# Membuat skrip otomatis tanpa perlu masuk ke nano
cat > backup-otomatis.sh << 'EOF'
#!/bin/bash
BACKUP_BASE="$HOME/lab-os/chapter12-backup/arsip-cron"
SOURCE="$HOME/lab-os/chapter12-backup/data-sumber"
DATE=$(date +%Y-%m-%d-%H%M)
LOG="$HOME/lab-os/chapter12-backup/cron-backup.log"

mkdir -p "$BACKUP_BASE/$DATE"
echo "[$(date)] Memulai backup ke $BACKUP_BASE/$DATE" >> "$LOG"

rsync -av --delete "$SOURCE/" "$BACKUP_BASE/$DATE/" >> "$LOG" 2>&1

if [ $? -eq 0 ]; then
    UKURAN=$(du -sh "$BACKUP_BASE/$DATE/" | cut -f1)
    echo "[$(date)] OK: $DATE ($UKURAN)" >> "$LOG"
else
    echo "[$(date)] GAGAL: backup $DATE" >> "$LOG"
    rmdir "$BACKUP_BASE/$DATE" 2>/dev/null
fi

# Menghapus otomatis folder backup yang usianya lebih dari 30 menit
find "$BACKUP_BASE" -maxdepth 1 -type d -mmin +30 -exec rm -rf {} \; 2>/dev/null
EOF

# Memberikan hak akses eksekusi pada skrip
chmod +x backup-otomatis.sh
```
2. Uji skrip secara manual sebelum mendaftarkannya ke cron:

```
./backup-otomatis.sh
ls -la arsip-cron/
cat cron-backup.log
```
3. Daftarkan skrip ke crontab untuk berjalan setiap 2 menit (untuk keperluan pengujian):

```
crontab -l 2>/dev/null > /tmp/cron-sementara
echo "*/2 * * * * $HOME/lab-os/chapter12-backup/backup-otomatis.sh" >> /tmp/cron-sementara
crontab /tmp/cron-sementara
rm /tmp/cron-sementara
crontab -l
```
4. Tunggu beberapa menit lalu verifikasi cron telah menjalankan backup:

```
ls -lt arsip-cron/
cat cron-backup.log
grep CRON /var/log/syslog 2>/dev/null | tail -5
```
Amati: berapa direktori backup yang sudah terbuat? Apakah timestamp pada nama direktori sesuai dengan waktu eksekusi di log?

5. Bersihkan crontab dan direktori:

```
crontab -l | grep -v "backup-otomatis" | crontab
crontab -l
rm -rf arsip-cron/ arsip-tar/ cron-backup.log backup.log
```
### Tantangan 12.4

Jadwalkan skrip rsync-backup.sh dari challenge box Praktek 12.2 agar berjalan setiap hari pukul 02:00 menggunakan crontab. Tambahkan ke skrip tersebut agar output rsync disimpan ke /var/log/backup.log menggunakan teknik tee dari Bab 3 (sehingga sekaligus tercetak di terminal saat dijalankan manual dan tersimpan ke log). Tulis cron expression yang tepat dan jelaskan setiap field-nya.

### Jawaban tantangan 12.4
<img width="469" height="539" alt="image" src="https://github.com/user-attachments/assets/0e5b1426-0e47-45b2-8985-fcdd14798e90" />


## Praktek 12.5 : Simulasi Pemulihan dari Backup

1. Siapkan data sumber dan buat backup sebagai persiapan:

```
cd ~/lab-os/chapter12-backup

# Menyiapkan folder dan file simulasi
mkdir -p data-sumber/{dokumen,konfigurasi}
echo "laporan-akhir-tahun" > data-sumber/dokumen/laporan.txt
echo "catatan-rapat" > data-sumber/dokumen/catatan.txt
echo "db_host=localhost" > data-sumber/konfigurasi/app.conf
echo "cache_ttl=300" >> data-sumber/konfigurasi/app.conf

# Membuat direktori tempat menyimpan arsip backup dan snapshot
mkdir -p arsip-pemulihan snapshot-pemulihan

# Membuat full backup berbentuk file kompresi (tar.gz)
tar -czf arsip-pemulihan/full-backup.tar.gz data-sumber/

# Menghitung dan menyimpan nilai hash MD5 dari file backup tar.gz tersebut
md5sum arsip-pemulihan/full-backup.tar.gz > arsip-pemulihan/full-backup.md5

# Membuat backup cerminan (mirror) menggunakan rsync
rsync -av data-sumber/ snapshot-pemulihan/

# Memeriksa isi seluruh direktori beserta sub-direktorinya secara detail
ls -lhR data-sumber/
```
2. Simpan checksum file-file penting untuk verifikasi nanti:

```
# Membuat rekam jejak integritas (sidik jari file) dari data asli sebelum rusak
md5sum data-sumber/dokumen/* data-sumber/konfigurasi/* > checksum-asli.md5

# Melihat daftar hash MD5 yang baru saja dibuat
cat checksum-asli.md5
```
3. Simulasikan kehilangan data: hapus beberapa file secara sengaja:

```
# Menghapus file penting secara permanen
rm data-sumber/dokumen/laporan.txt
rm data-sumber/konfigurasi/app.conf

# Menyusupkan file asing (simulasi jika sistem disusupi/korup)
echo "file tidak sah" > data-sumber/dokumen/tidak-dikenal.txt

# Mengecek kondisi folder yang sudah rusak
ls -la data-sumber/dokumen/
ls -la data-sumber/konfigurasi/
```
4. Pulihkan file yang hilang dari arsip tar ke direktori sementara:

```
# Verifikasi dulu apakah file backup tar.gz kita tidak rusak
md5sum -c arsip-pemulihan/full-backup.md5

# Menyiapkan ruang karantina (folder sementara) untuk mengekstrak file
mkdir -p /tmp/restore-lap14

# Mengekstrak spesifik file laporan.txt dari dalam arsip tar.gz ke folder sementara
tar -xzf arsip-pemulihan/full-backup.tar.gz -C /tmp/restore-lap14 data-sumber/dokumen/laporan.txt

# Verifikasi file berhasil terekstrak
ls /tmp/restore-lap14/data-sumber/dokumen/

# Mengembalikan file yang sudah diekstrak ke habitat aslinya
cp /tmp/restore-lap14/data-sumber/dokumen/laporan.txt data-sumber/dokumen/

# Menghapus folder karantina sementara
rm -rf /tmp/restore-lap14
```
5. Pulihkan file konfigurasi dari snapshot rsync:

```
# Melihat keberadaan file backup di dalam snapshot rsync
ls snapshot-pemulihan/konfigurasi/

# Mengkopi (memulihkan) file app.conf dari snapshot ke direktori utama
cp snapshot-pemulihan/konfigurasi/app.conf data-sumber/konfigurasi/

# Verifikasi file sudah kembali
ls -la data-sumber/konfigurasi/
```
6. Verifikasi integritas setelah pemulihan:

```
# Menghapus file penyusup (simulasi pembersihan anomali)
rm data-sumber/dokumen/tidak-dikenal.txt

# Membandingkan hash MD5 file saat ini dengan data di awal (sebelum dihapus)
md5sum -c checksum-asli.md5

# Memeriksa status exit dari perintah sebelumnya
echo "Exit code: $? (0 berarti semua file valid)"

# Melakukan komparasi biner (diff) secara rekursif (-r) antara sumber dan snapshot
diff -r data-sumber/ snapshot-pemulihan/

# Memeriksa status exit diff
echo "Diff exit code: $? (0 berarti identik dengan snapshot)"
```
7. Bersihkan seluruh direktori lab:

```
rm -rf arsip-pemulihan/ snapshot-pemulihan/
rm -f checksum-asli.md5 rencana-backup.txt backup-otomatis.sh
```
### Tantangan 12.5

Perpanjang skenario pemulihan: hapus seluruh direktori data-sumber/konfigurasi/ sekaligus (bukan hanya satu file). Kemudian pulihkan seluruh direktori tersebut dari snapshot menggunakan rsync dengan arah transfer yang dibalik (sumber adalah snapshot, tujuan adalah direktori yang rusak). Verifikasi hasil pemulihan dengan membandingkan checksum seluruh isi direktori menggunakan find dan md5sum. Catat perbedaan waktu antara restore satu file dan restore satu direktori penuh.

### Jawaban Tantangan 12.5
<img width="477" height="436" alt="image" src="https://github.com/user-attachments/assets/7b6d32cc-b535-414c-b350-b6d6cc6b6cf6" />

kesimpulan :

Siap. Kalau kamu menghindari format tabel, struktur bullet list ini adalah format Markdown yang paling bersih dan rapi untuk di-commit ke repositori GitHub milikmu.

Tinggal copy-paste teks di bawah ini langsung ke file .md atau editor GitHub-mu:

Hasil Pengamatan: Perbandingan Waktu Restorasi Data
1. Pemulihan 1 Direktori Penuh

Utilitas: rsync

Sumber Data: Direktori Snapshot (Uncompressed)

Waktu Eksekusi (Real): 0.136 detik (136 milidetik)

Beban Pemrosesan: Operasi I/O (Input/Output) fisik langsung ke hard disk.

Analisis Teknis: Kecepatan sangat tinggi karena data bersumber dari direktori terbuka. Sistem murni hanya menyalin byte fisik ke lokasi baru tanpa melalui hambatan pemrosesan komputasi CPU.

2. Pemulihan 1 File Spesifik

Utilitas: tar -xzf

Sumber Data: Arsip .tar.gz (Compressed)

Waktu Eksekusi (Real): Lebih lambat (Terkendala proses dekompresi)

Beban Pemrosesan: Overhead komputasi CPU di dalam memori (RAM).

Analisis Teknis: Kecepatan terhambat karena kernel dipaksa bekerja memecah algoritma kompresi arsip terlebih dahulu, memindai seluruh hierarki di dalamnya untuk mencari 1 target spesifik, baru kemudian diekstrak.


## 1.7 Latihan 
nstruksi Umum: Kerjakan seluruh latihan secara mandiri. Catat langkah penting, simpan tangkapan layar bila diperlukan, lalu rangkum hasilnya sebagai dokumentasi pribadi. 

### Latihan 12.1 Implementasi Sistem Backup Lengkap

Rancang dan implementasikan sistem backup untuk direktori simulasi.
1. Buat struktur direktori simulasi dengan minimal 10 file yang tersebar di tiga subdirektori:
dokumen/, konfigurasi/, dan media/.

jawab :
<img width="332" height="126" alt="image" src="https://github.com/user-attachments/assets/d54f16ce-e5e9-421a-9700-d3a8e2370b33" />

2. Buat skrip backup-harian.sh yang menggunakan rsync dengan–link-dest untuk mem
buat snapshot harian. Skrip harus mencatat log dengan timestamp ke backup-harian.log.

jawab :
<img width="465" height="443" alt="image" src="https://github.com/user-attachments/assets/0f05a6a7-081b-4cec-8e8c-3aa038bee307" />

3. Buat skrip backup-mingguan.sh yang menggunakan tar-czf untuk membuat arsip terkom
presi. Skrip harus membuat checksum MD5 dari setiap arsip yang dibuat.

jawab :

<img width="475" height="506" alt="image" src="https://github.com/user-attachments/assets/f3af6014-0017-49f3-b9f9-cb87427b0c9b" />

4. Daftarkan keduanya ke crontab dengan jadwal yang berbeda. Jalankan masing-masing secara
manual dan verifikasi log dan output yang dihasilkan.
5. Simulasikan kehilangan file dan lakukan restore dari kedua jenis backup. Dokumentasikan
langkah-langkah restore dan waktu yang dibutuhkan.

jawab :
<img width="479" height="481" alt="image" src="https://github.com/user-attachments/assets/64db711b-0681-4679-a3e4-350ef0e78787" />


### Latihan 12.2 Analisis Kompresi dan Performa Backup

Analisis trade-off antara kecepatan dan rasio kompresi.
1. Buat direktori dengan tiga jenis file: teks biasa (10 file .txt masing-masing 100 baris), file kon
figurasi .conf, dan file biner simulasi menggunakan dd if=/dev/urandom of=biner.bin
bs=1M count=5.

jawab :

<img width="436" height="490" alt="image" src="https://github.com/user-attachments/assets/0225c10a-8370-47b5-a8ed-b0957e920efd" />

2. Buat tiga arsip dari direktori yang sama menggunakan gzip (-z), bzip2 (-j), dan xz (-J). Ukur
waktu setiap proses menggunakan time.

jawab :

<img width="474" height="398" alt="image" src="https://github.com/user-attachments/assets/0123fee4-a4c3-4200-9f1d-7bc94ce43f79" />

3. Bandingkan ukuran ketiga arsip dengan ls-lh dan hitung rasio kompresi masing-masing
terhadap ukuran asli.

jawab : 

<img width="480" height="63" alt="image" src="https://github.com/user-attachments/assets/d2ae8541-6785-4309-9780-6e910e0bbab2" />

Jadi, berdasarkan perintah ls -lh file arsip(.tar.gz, .tar.bz2, dan .tar.xz) menghasilkan ukuran 5.1M. Rasio komperasi menggunakan perhitungan 5.1M/5.1M = 1. Jadi, rasio untuk ketiga utilitas tersebut adalah 1:1. Membuktikan bahwa algoritma pemampatan data tifak bekerja.

4. Buat tabel di file analisis-kompresi.txt yang merangkum: jenis kompresi, waktu kompres,
ukuran hasil, dan rasio kompresi.

jawab :

<img width="475" height="201" alt="image" src="https://github.com/user-attachments/assets/37527414-7773-4a8f-a5c2-cb32d3bd4387" />


5. Berdasarkan data tersebut, rekomendasikan kompresi yang paling tepat untuk: backup harian
otomatis, arsip jangka panjang, dan backup file biner. Berikan alasan untuk setiap rekomendasi.

jawab :

A. Backup Harian Otomatis

    Rekomendasi : gzip (.tar.gz)

    Alasan Faktual: Backup harian berjalan sangat rutin. Oleh karena itu, server membutuhkan utilitas dengan eksekusi secepat kilat agar beban CPU tidak melonjak dan mengganggu layanan (services) lain yang sedang berjalan. Berdasarkan data, gzip mutlak menjadi yang tercepat dan memberikan keseimbangan paling efisien antara kecepatan performa dan hasil kompresi.

B. Arsip Jangka Panjang (Archiving)

    Rekomendasi : xz (.tar.xz)

    Alasan Faktual: Arsip tahunan atau bulanan sangat jarang diakses kembali. Di skenario ini, efisiensi ruang penyimpanan fisik (storage) adalah prioritas utama. Meskipun data menunjukkan proses xz adalah yang paling lambat dan memakan banyak memori, trade-off (pengorbanan waktu) ini sepenuhnya bisa ditoleransi demi menekan ukuran file menjadi sekecil mungkin di ruang penyimpanan.

C. Backup File Biner (Video, Gambar, Data Acak)

    Rekomendasi : tar murni (tanpa kompresi) atau batas maksimal gzip.

    Alasan Faktual: Data eksperimen membuktikan secara nyata bahwa file biner (seperti simulasi /dev/urandom tadi) bersifat incompressible (menghasilkan rasio 1:1). Memaksa penggunaan algoritma kompresi tingkat tinggi seperti xz atau bzip2 pada sekumpulan file biner adalah pemborosan komputasi (CPU overhead). Waktu eksekusi akan melonjak drastis, namun ukuran filenya mutlak tidak akan mengecil.


### Latihan 12.3 Disaster Recovery Drill

Lakukan simulasi pemulihan bencana secara menyeluruh.
1. Buat direktori “produksi” dengan struktur lengkap: file konfigurasi, dokumen, dan skrip. Buat
backup penuh menggunakan tar dan simpan checksumnya.

jawab :

<img width="479" height="234" alt="image" src="https://github.com/user-attachments/assets/121a377e-10fe-459b-a467-65ee5f26554d" />


2. Dokumentasikan kondisi awal: daftar file, ukuran, dan checksum semua file menggunakan find
dan md5sum.

jawab : 

<img width="478" height="152" alt="image" src="https://github.com/user-attachments/assets/d0537508-4284-4e00-8e81-61273c362570" />

3. Simulasikan bencana: hapus seluruh direktori produksi dengan rm-rf.

jawab :

<img width="479" height="77" alt="image" src="https://github.com/user-attachments/assets/03a4cfd9-403b-49c5-bd6d-a5eb4bf0f83f" />

4. Catat waktu mulai pemulihan, lakukan restore lengkap dari backup, dan catat waktu selesai.

jawab :

<img width="438" height="122" alt="image" src="https://github.com/user-attachments/assets/79b329d4-42f0-44f9-880b-793bcef6ae3b" />

5. Verifikasi semua file pulih dengan benar menggunakan checksum yang disimpan di langkah 2.

jawab : 

<img width="468" height="65" alt="image" src="https://github.com/user-attachments/assets/2419dfbc-5afa-4ce7-a3fe-9489b5aee807" />

6. Bandingkan RTO aktual dengan target yang kamu tentukan. Jika lebih lama, identifikasi
bottleneck dan usulkan cara mempercepatnya

jawab :

<img width="218" height="69" alt="image" src="https://github.com/user-attachments/assets/166cfed9-04e2-4a2a-aede-6132fefdc3ef" />

Target RTO : 2 detik
RTO aktual : 0.063 detik














