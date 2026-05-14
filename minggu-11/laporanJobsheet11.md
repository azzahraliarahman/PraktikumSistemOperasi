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

### jawaban Analisis 9.1

1. Karena 6 untuk owner, owner bisa read and write. Sedangkan, angka 0 di posisi kedua (group) dan ketiga (others) berarti tidak ada hak akses sama sekali (no permissions).
2. Jika 600 (-rw-------) maka owner punya hak read and write sedangkan yang lain tidak punya hak akses apapun kecuali owner. Sedangkan 755 (-rwxr-xr-x), Owner memiliki hak akses penuh (read,write & execute), sedangkan group dan others hanya bisa membaca dan mengeksekusi file tersebut tanpa mengubah isisnya.
3. Permission yang dihasilkan adalah -rw-r----- (640). Tidak menghasilan 777 karena permission akhir adalah hasil dari Base Permission dikurangi umask. basenya adalah 666 ** Jadi : 666 - 027 = 640** Itu terjadi karena default linux tidak memberikan izin eksekusi pada file yang baru dibuat demi keamanan. izin eksekusi harus memakai chmod.

### jawaban Tantangan 9.1
* hasil eksekusi ls -l secret.txt sebelum :

<img width="425" height="28" alt="image" src="https://github.com/user-attachments/assets/ed166636-3922-4c8f-abf4-231eaca9c6ee" />

* hasil eksekusi ls -l secret.txt sesudah :

<img width="398" height="34" alt="image" src="https://github.com/user-attachments/assets/3b8c2a68-b9f5-45ea-90f5-677acf9ee6d5" />

* kesimpulan : Sebelumnya pada secret.txt, kolom group tertulis zahra zahra. Artinya pemiliknya zahra dan group zahra. Sesudahnya kolom group berubah menjadi zahra www-data. Oleh karena itu, siapapun yang masuk dalam group www-data akan memiliki hak akses yang ditentukan oleh digit 2 permission.

## Praktikum 9.2—ACL

Langkah 1 :Siapkan file dan lihat permission standar tanpa ACL tambahan

```
mkdir ~/lab-acl && cd ~/lab-acl
echo "Data penting" > confidential.txt
chmod 640 confidential.txt
ls -l confidential.txt
getfacl confidential.txt
```
Pada tahap ini,getfacl hanya menampilkan
tiga entri dasar :owner,group,dan others.Belum ada named user
atau named group.

Langkah 2 :Beri akses baca ke satu user tertentu tanpa mengubah owner atau group.

```
setfacl -m u:userA:r confidential.txt
ls -l confidential.txt
getfacl confidential.txt
```
Perhatikan dua perubahan:
• output ls -l menampilkan tanda +;
• output get facl kini memiliki entri user:userA:r–

Langkah 3 :Buat direktori bersama yang mewariskan ACL ke file baru.

```
mkdir shared
setfacl -d -m u:userA:rwx shared
setfacl -d -m u:userB:r-x shared
getfacl shared
touch shared/inherited.txt
getfacl shared/inherited.txt
```

### Analisis 9.2
1. Mengapa getfacl confidential.txt awalnya tidak menampilkan user tertentu?
2. Setelah setfacl-m u:userA:r confidential.txt, apa perbedaan output ls-l dan getfacl?
3. Mengapa file inherited.txt mewarisi ACL dari direktori shared?
### Tantangan 9.2
Tambahkan satu ACL lagi agar group readonly-group hanya dapat membaca confidential.txt. Setelah itu, hapus ACL untuk userA dan verifikasi hasil akhirnya dengan getfacl











