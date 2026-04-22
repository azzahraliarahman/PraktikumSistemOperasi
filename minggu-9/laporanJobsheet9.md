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



