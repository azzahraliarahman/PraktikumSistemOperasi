<h4>Nama : Azzahra Aulia Rahman<h4>
<h4>NIM : 2541007020227<h4>

# Laporan Jobsheet 11 : Manajemen File & User/Group

## Praktikum 9.1 — Permissions

Langkah 1: Buat direktori kerja dan dua file uji

```
mkdir ~/lab-permissions && cd ~/lab-permissions
echo "data rahasia" > secret.txt
echo '#!/bin/bash' > myscript.sh
echo 'echo Hello' >> myscript.sh
ls -la
```
Perhatikan permission awal kedua file. Biasanya file baru tidak memiliki bit execute.

Langkah 2: Jadikan secret.txt privat hanya untuk owner

```
chmod 600 secret.txt
ls -l secret.txt
```
Permission 600 berarti owner dapat membaca dan menulis, sedangkan group dan others tidak memiliki akses.

Langkah 3: Jadikan myscript.sh dapat dijalankan.

```
chmod 755 myscript.sh
ls -l myscript.sh
./myscript.sh
```
Permission 755 memberi hak penuh ke owner, dan hak baca+execute ke group serta others. Tanpa bit execute,
file skrip tidak bisa dijalankan langsung

Langkah 4: Buat direktori bersama dan amati efek SGID sederhana

```
mkdir shared-dir
chmod g+s shared-dir
ls -ld shared-dir
```
Jika output menampilkan huruf s pada posisi group execute, berarti SGID aktif.

Langkah 5: Uji efek umask pada file baru.

```
umask
umask 027
touch testfile -027
ls -l testfile -027
```

### Analisis 9.1
1. Mengapa secret.txt tidak dapat dibaca oleh group dan others setelah chmod 600?
2. Apa perbedaan arti 600 dan 755 terhadap file yang diuji?
3. Setelah umask 027, permission apa yang dihasilkan untuk file baru, dan mengapa bukan 777?

### Tantangan 9.1
Ubah owner atau group salah satu file uji ke akun atau group lain yang tersedia di sistem, kemudian jelaskan
perubahan output ls -l sebelum dan sesudahnya.






