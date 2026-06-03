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
tar -tzf arsip-tar/full-$(date +%F).tar.gz | grep "\.conf"

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

## Praktek 12.4: Jadwalkan Skrip Backup Otomatis








