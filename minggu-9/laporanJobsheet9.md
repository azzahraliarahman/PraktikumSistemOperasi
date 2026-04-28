<h4>Nama : Azzahra Aulia Rahman</h4>
<h4>NIM : 254107020227</h4>

# Jobsheet 9 - Pemrograman Bash

## Praktikum  7.1 Script Pertama: Laporan Sistem

1. Buat workspace praktikum

```
mkdir-p ~/praktikum-os/week09/{scripts,logs,data}
cd ~/praktikum-os/week09/scripts
```
2. Buat script dengan nano:
   
```
nano laporan-sistem.sh
```
3. Ketik isi berikut, simpan ( Ctrl+O Enter), lalukeluar (Ctrl+X ):
```
#!/bin/bash
# Script: laporan-sistem.sh
echo "================================"
echo " LAPORAN SISTEM"
echo "================================"
echo "Tanggal : $(date '+%A, %d %B %Y')"
echo "Jam
: $(date '+%H:%M:%S')"
echo "Hostname : $(hostname)"
echo "User
: $(whoami)"
echo "CPU core : $(nproc)"
echo "RAM bebas: $(free-h | awk '/^Mem/ {print $4}')"
echo "Disk / : $(df-h / | awk 'NR==2 {print $5}')
terpakai"
echo "================================"

```
4. Beri izin dan jalankan:

```
chmod +x laporan-sistem.sh
./laporan-sistem.sh
```

###   Latihan 9.1

Modifikasi laporan-sistem.sh agar menyimpan output ke file
laporan-YYYY-MM-DD.txt sekaligus menampilkannya di terminal. Petunjuk:
gunakan tee yang sudah dipelajari di bab sebelumnya.

### Jawaban Latihan 9.1
<img width="597" height="253" alt="Screenshot 2026-04-22 173728" src="https://github.com/user-attachments/assets/d2b57fb3-fd73-4225-9002-39d7f328cf1a" />

## Praktikum 7.2 Script Info Sistem dengan Argumen

1. Buat script:
   ```
   nano ~/praktikum-os/week09/scripts/info-sistem.sh
   ```
2. Ketik isi berikut:

 ```
#!/bin/bash
# Penggunaan: ./info-sistem.sh [nama-admin] [batas
disk-persen]
ADMIN=${1:-"Tidak dikenal"}
BATAS=${2:-80}
TANGGAL=$(date '+%F %T')
DISK_PERSEN=$(df / | awk 'NR==2 {print $5}' | tr-d '%
')
echo "Admin : $ADMIN"
echo "Tanggal : $TANGGAL"
echo "Disk / : ${DISK_PERSEN}% terpakai"
echo "Batas : ${BATAS}%"
if [ "$DISK_PERSEN"-gt "$BATAS" ]; then
echo "STATUS : PERINGATAN-disk melebihi batas!
"
else
SISA=$((BATAS- DISK_PERSEN))
echo "STATUS : Normal (sisa toleransi ${SISA}%)"
fi
```
3. Simpan,beri izin,uji dengan berbagai kombinasi argumen:

```
   chmod +x ~/praktikum-os/week09/scripts/info-sistem.sh
./info-sistem.sh
./info-sistem.sh "Dian" 50
./info-sistem.sh "Dian" 10
 ```
### Latihan 9.2
Buat script kalkulator.sh yang menerima tiga argumen: <angka1>
<operator> <angka2> dengan operator +,-, *, atau /. Contoh:
./kalkulator.sh20+5menghasilkan25.Gunakancaseuntukmemilih
operasi,danvalidasi jikaargumentidaklengkap.

### Jawaban Latihan 9.2
* Gambar:
  <img width="643" height="820" alt="Screenshot 2026-04-23 223327" src="https://github.com/user-attachments/assets/58ac87c0-2a7b-4986-ad5d-bf9196d0a283" />

## Praktikum 7.3 Script Grading dan Menu Interaktif

1. Buat script grading (menggunakan if dan for):
   
```
   nano ~/praktikum-os/week09/scripts/grading-batch.sh
```
2. Ketik isi berikut:
   
```
   #!/bin/bash
# Script: grading-batch.sh
# Proses daftar nilai mahasiswa
MAHASISWA=("Andi:92" "Budi:73" "Citra:55" "Deni:80" "
Eka:45")
echo "=== HASIL GRADING ==="
for ENTRI in "${MAHASISWA[@]}"; do
NAMA=$(echo "$ENTRI" | cut-d:-f1)
NILAI=$(echo "$ENTRI" | cut-d:-f2)
if [ "$NILAI"-ge 85 ]; then
GRADE="A"
elif [ "$NILAI"-ge 75 ]; then
GRADE="B"
elif [ "$NILAI"-ge 65 ]; then
GRADE="C"
elif [ "$NILAI"-ge 55 ]; then
GRADE="D"
else
GRADE="E"
fi
printf "%-10s %3d %s\n" "$NAMA" "$NILAI" "$GRADE"
done
echo "====================="
 ```
3. Simpan,beri izin, dan jalankan:
   
```
chmod +x ~/praktikum-os/week09/scripts/grading-batch.
sh
./grading-batch.sh
```
4. Buat script menu interaktif (while+case):

```
nano ~/praktikum-os/week09/scripts/menu-sistem.sh
```
5.Ketik isi berikut:

```
#!/bin/bash
# Menu interaktif pemantauan sistem
while true; do
echo ""
echo "===== MENU MONITOR ====="
echo "1) Info disk"
echo "2) Info memori"
echo "3) Proses teratas"
echo "4) Keluar"
echo-n "Pilih [1-4]: "
read PILIHAN

case $PILIHAN in
1) df-h ;;
2) free-h ;;
3) ps aux--sort=-%cpu | head-6 ;;
4) echo "Sampai jumpa!"; exit 0 ;;
*) echo "Pilihan tidak valid." ;;
esac
done
```
6. Beri izin dan jalankan, coba setiap opsi:

```
chmod +x ~/praktikum-os/week09/scripts/menu-sistem.sh
./menu-sistem.sh
```

### Latihan 9.3

Tambahkan ke script grading-batch.sh sebuah ringkasan di bagian bawah
yang menampilkan: jumlah mahasiswa per grade (A, B, C, D, E) menggunakan
perulangan for kedua yang mengiterasi array MAHASISWA.

* Output :
  <img width="696" height="434" alt="Screenshot 2026-04-28 221312" src="https://github.com/user-attachments/assets/a2727738-31fe-41de-bee9-2c34b34c7317" />

  ## Praktikum 7.4 Library Fungsi Validasi
  
  1. Buat file library:
  ```
  nano ~/praktikum-os/week09/scripts/lib-validasi.sh
  ```
  2. Ketik isi berikut:
 ```
  #!/bin/bash
# lib-validasi.sh-Library fungsi validasi
adalah_angka() {
[[ "$1" =~ ^[0-9]+$ ]]
}

file_bisa_dibaca() {
[-f "$1" ] && [-r "$1" ]
}
error_exit() {
echo "ERROR: $1" >&2
exit 1
}
info() { echo "[INFO] $1"; }
sukses() { echo "[OK] $1"; }
```
3. Buat script yang menggunakan library:
```
nano ~/praktikum-os/week09/scripts/pakai-library.sh
```
4.Ketik isi berikut:
```
#!/bin/bash
# Muat library (seperti import di Java)
source ~/praktikum-os/week09/scripts/lib-validasi.sh
ANGKA=$1
FILE=$2
[-z "$ANGKA" ] || [-z "$FILE" ] && \
error_exit "Penggunaan: $0 <angka> <path-file>"
if adalah_angka "$ANGKA"; then
sukses "Input '$ANGKA' adalah angka valid"
else
error_exit "'$ANGKA' bukan angka"
fi
if file_bisa_dibaca "$FILE"; then
sukses "File '$FILE' bisa dibaca"
info "Jumlah baris: $(wc-l < "$FILE")"
else
error_exit "File '$FILE' tidak ada atau tidak bisa
dibaca"
fi
```
5. Beri izin dan uji semua skenario:
```
chmod +x ~/praktikum-os/week09/scripts/pakai-library.
sh
./pakai-library.sh 42 /etc/hostname
./pakai-library.sh abc /etc/hostname
./pakai-library.sh 42 /tidak-ada.txt
./pakai-library.sh
```

###  Latihan 9.4

Tambahkan fungsi konfirmasi() ke lib-validasi.sh.
Fungsi ini
menampilkan pertanyaan, membaca input Y/N dari user, mengembalikan
0 jika Y dan 1 jika N. Buat script demo yang memanggil fungsi ini sebelum
menghapus sebuah file.

### Jawaban :
<img width="912" height="316" alt="Screenshot 2026-04-28 224733" src="https://github.com/user-attachments/assets/f78d4648-5e64-4616-baa3-93e5f5ff8014" />

## Praktikum 7.5 Script Backup dengan Opsi

1. Buat script:
   ```
   nano ~/praktikum-os/week09/scripts/backup-data.sh
   ```
2. Ketik isi berikut:
```
#!/bin/bash
# Penggunaan: ./backup-data.sh [-v] [-c] [-l logfile]
<sumber> <tujuan>
VERBOSE=false
COMPRESS=false
LOG_FILE=""
while getopts "vcl:" OPSI; do
case $OPSI in
v) VERBOSE=true ;;
c) COMPRESS=true ;;
l) LOG_FILE="$OPTARG" ;;
*) echo "Penggunaan: $0 [-v] [-c] [-l logfile]
<sumber> <tujuan>"
exit 1 ;;
esac
done
shift $((OPTIND- 1))
SUMBER=$1
TUJUAN=$2
log() {
local MSG="[$(date '+%T')] $1"
echo "$MSG"
[-n "$LOG_FILE" ] && echo "$MSG" >> "$LOG_FILE"
}
[-z "$SUMBER" ] || [-z "$TUJUAN" ] && {
echo "ERROR: sumber dan tujuan wajib diisi"; exit
1; }
[ !-d "$SUMBER" ] && { log "ERROR: $SUMBER tidak ada"
; exit 2; }
mkdir-p "$TUJUAN"
TANGGAL=$(date '+%F-%H%M%S')
[ "$VERBOSE" = true ] && log "Sumber: $SUMBER | Tujuan
: $TUJUAN"
if [ "$COMPRESS" = true ]; then
ARSIP="$TUJUAN/backup-$(basename "$SUMBER")
$TANGGAL.tar.gz"
tar-czf "$ARSIP"-C "$(dirname "$SUMBER")" "$(
basename "$SUMBER")"
log "Arsip: $ARSIP ($(du-sh "$ARSIP" | cut-f1))"
else
cp-r "$SUMBER" "$TUJUAN/backup-$(basename "
$SUMBER")-$TANGGAL"
log "Backup selesai."
fi
```
3. Beri izin dan uji:

  ```
chmod +x ~/praktikum-os/week09/scripts/backup-data.sh
cd ~/praktikum-os/week09/scripts
# Tanpa opsi
./backup-data.sh ~/praktikum-os/week09/data ~/
praktikum-os/week09/logs
# Dengan verbose dan kompresi + log ke file
./backup-data.sh-v-c-l ../logs/backup.log \
~/praktikum-os/week09/data ~/praktikum-os/week09/
logs
cat ../logs/backup.log
```
## Praktikum 7.6 Debugging Script

1. Buat script untuk dianalisis:
```
nano ~/praktikum-os/week09/scripts/debug-latihan.s
```
2. Ketik isi berikut:
```
#!/bin/bash
# Script: debug-latihan.sh
# Penggunaan: ./debug-latihan.sh <direktori> <batas-MB
>
DIREKTORI=$1
BATAS=$2
if [ $#-ne 2 ]; then
echo "Penggunaan: $0 <direktori> <batas-MB>"
exit 1
fi
UKURAN=$(du-sm "$DIREKTORI" | cut-f1)
echo "Direktori : $DIREKTORI"
echo "Ukuran : ${UKURAN} MB"
echo "Batas : ${BATAS} MB"
if [ "$UKURAN"-gt "$BATAS" ]; then
echo "PERINGATAN: Ukuran melebihi batas!"
echo "Kelebihan: $((UKURAN-BATAS)) MB"
else
echo "Status: Normal (sisa: $((BATAS-UKURAN)) MB
)"
fi
```
3. Cek sintaks, lalu jalankan dengan tracing:
```
chmod +x ~/praktikum-os/week09/scripts/debug-latihan.
sh
bash-n debug-latihan.sh && echo "Sintaks OK"
bash-x debug-latihan.sh /etc 10
./debug-latihan.sh /var 50
./debug-latihan.sh
```

### Latihan 9.5
Script debug-latihan.sh tidak menangani direktori yang tidak ada.Perbaiki
dengan menambahkan:
•set -e di baris kedua
•Pengecekan -d "$DIREKTORI" sebelum memanggil du
•Pesan error yang informatif jika direktori tidak ditemukan
Uji dengan direktori yang tidak ada.

### Jawab Latihan 9.5
<img width="939" height="452" alt="Screenshot 2026-04-28 232357" src="https://github.com/user-attachments/assets/d45429a0-109f-43b1-a36b-7f6aebf00ae2" />

## 1.8 Tugas Praktikum

### Tugas 1 Script Absensi Kelas

<img width="957" height="377" alt="Screenshot 2026-04-28 233314" src="https://github.com/user-attachments/assets/aab49804-33fa-4490-b584-0c3dfaee7e54" />

### Tugas 2 Script Health Check Sistem

<img width="794" height="891" alt="Screenshot 2026-04-28 233721" src="https://github.com/user-attachments/assets/b9108f44-8ef2-43ed-b623-12a5509ad116" />


