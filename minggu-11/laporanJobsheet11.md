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

### Jawaban Analisis 9.2
1. Karena saat menjalankan setfcl, file belum memiliki Access Control List (ACL). Jadi, getfcl hanya menampilkan informasi dasar seperti ex : owner, group & others.
2. * ls -l : Ada tanda + diakhir permission. Tanda ini menunjukkan bahwa file tersebut memiliki pengaturan ACL, namun tidak bisa merinci siapa user tambahannya.
   * getfacl : memunculkan rincian lengkap semua user atau group yang memiliki akses, ex : user:usera:r--.
3. Karena menggunakan opsi -d (Default) saat mengatur ACL pada direktori shared.

### Jawaban Tantangan 9.2

<img width="464" height="236" alt="image" src="https://github.com/user-attachments/assets/b3551dc1-24d7-449d-8a8a-dc722f1415ee" />

## Praktikum 9.3A — Membuat dan Mengelola User

Tujuan:membuat user baru ,memodifikasi propertinya,dan memahami perbedaan opsi user add dan user mod.

```
# buat dua user
sudo useradd -m -s /bin/bash userA
sudo useradd -m -s /bin/bash userB
sudo passwd userA
sudo passwd userB

# verifikasi
id userA
getent passwd userA

# modifikasi shell userA
sudo usermod -s /bin/zsh userA
getent passwd userA

# lock dan unlock userB
sudo usermod -L userB
sudo passwd-S userB
sudo usermod-U userB
sudo passwd-S userB
```

### Pertanyaan 9.3A
1. Apa perbedaan output id userA sebelum dan sesudah menambah group?
2. Bagaimana status passwd -S userB berubah saat akun di-lock?

### Jawaban Pertanyaan 9.3A
1. Sebelum : Output dari userA -> uid=1004(userA) gid=1005(userA) groups=1005(userA), ini menunjukkan bahwa UserA hanya menjadi anggota dari grup utamanya sendiri yaitu, userA (GID 1005), tidak ada perintah untuk menambah group.
2. Sebelumnya statusnya adalah P (Password usable), maksudnya memiliki password yang sah dan siap digunakan untuk login. Setelah dijalankan sudo usermod -L userB statusnya berubah menjadi L (Locked). Outputnya menjadi userB L 2026-05-19 0 99999 7 -1. tanda L menyatakan bahwa sistem telah mengunci password tersebut sehingga userB tidak bisa login ke server.

## Praktikum 9.3B — Group Management

Tujuan: membuat group, menambahkan user ke group, dan memverifikasi keanggotaan.

```
# buat dua group
sudo groupadd labgroup
sudo groupadd readonly-group

# tambahkan userA ke kedua group
sudo usermod -aG labgroup,readonly-group userA

# tambahkan userB hanya ke readonly-group
sudo usermod -aG readonly-group userB

# verifikasi
id userA
id userB
getent group labgroup
getent group readonly-group
```
### Pertanyaan 9.3B
1. Apa yang ditampilkan id userA vs groups userA?
2. Mengapa -a pada user mod -aG penting?

### Jawaban Pertanyaan 9.3B
1. > id userA : menampilkan informasi identitas dengan detail dan berbasis angka.
* uid: ID unik digital dari user tersebut (1004(userA)).

* gid: ID group utama/primer si user (1005(userA)).

* groups: Daftar semua group (baik primer maupun sekunder) yang diikuti beserta angka ID groupnya (1005(userA),1003(readonly-group),1007(labgroup))

   > groups userA : Menampilkan nama-nama group tempat user bergabung. Tanpa UID atau nomor GID.
   * Output : userA : userA readonly-group labgroup
 
2. -a adalah singkatan dari _append(menambah/menyisipkan)_ Opsi ini menentukan apakah group lama milik user akan dipertahankan atau tidak. Fungsi utamanya untuk memastikan group baru ditambahkan tanpa menghapus user dari group-group sekunder yang sudah ia ikuti sebelumnya.

## Praktikum 9.3C — Password Aging Policy

Tujuan: menerapkan kebijakan umur password dan mengamati efeknya.

```
# set aging policy untuk userA
sudo chage -M 60 -W 7 -m 1 userA
sudo chage -l userA

# paksa userA ganti password saat login pertama
sudo chage -d 0 userA

# kunci password userB
sudo passwd -l userB
sudo passwd -S userB

# unlock kembali
sudo passwd -u userB
sudo passwd -S userB
```
### Pertanyaan 9.3C
1. Apa arti nilai yang ditampilkan chage -l userA?
2. Bagaimana cara membuktikan userB terkunci dari output passwd -S?
3. Kapan sebaiknya menggunakan chage -d 0 vs passwd -e?

### Tantangan 9.3C
Buat user bernama intern yang:
• memiliki shell /bin/bash;
• menjadi anggota labgroup;
• dipaksa ganti password pada login pertama;
• password expired setelah 45 hari dengan warning 7 hari sebelumnya.

### Jawaban Pertanyaan 9.3C
1. 




   


















