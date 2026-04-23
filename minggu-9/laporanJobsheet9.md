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



