<h4>Nama          : Azzahra Aulia Rahman<h4>
<h4>NIM           : 2541007020227<h4>
<h4>Kelas         : TI-1H</h4>
<h4>Mata Kuliah   : System Operasi</h4>
<h4>Pertemuan Ke- : 11</h4>

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
1. Perintah ini menampilkan detail masa belaku password untuk userA. Mengandung :
Kapan terakhir kali password diubah.

* Tanggal kedaluwarsa password .

* Jumlah hari minimum dan maksimum sebelum user diperbolehkan atau diwajibkan mengganti passwordnya lagi.

* Masa tenggang peringatan (warning) sebelum password kadaluarsa.

2. Saat dijalankan sudo passwd -l userB, output passwd -S userB memunculkan huruf L. Yang berarti Locked, membuktikan bahwa userB telah dikunci oleh sistem sehingga userB tidak bisa digunakan untuk login.

3.  chage -d 0 : Sebaiknya digunakan untuk manajemen akun jangka panjang. Perintah ini menyetel tanggal pergantian password terakhir menjadi hari ke-0.

   passwd -e: Untuk tindakan cepat dan instan. singkatan dari expire, perintah ini digunakan  oleh sysadmin secara manual ketika mereset password untuk user baru agar mereka langsung dipaksa membuat password pribadi rahasia saat masuk terminal.  


### Jawaban Tantangan 9.3C

<img width="907" height="584" alt="Screenshot 2026-05-19 211514" src="https://github.com/user-attachments/assets/dff0fab6-e314-496b-b7a4-c5961f9ab157" />

## Praktikum 9.4 — Konfigurasi sudo

Langkah 1: Buat file konfigurasi sudo khusus untuk userA.

```
sudo visudo-f /etc/sudoers.d/lab-userA
```
Perintah ini membuka editor aman khusus untuk file sudoers baru. Jika sintaks salah, visudo akan memperingatkan sebelum file disimpan.

Isi file dengan aturan berikut:

```
userA ALL=(root) NOPASSWD: /usr/bin/apt update, /usr/bin/apt
upgrade
userA ALL=(root) /bin/systemctl status *
```
Baris pertama berarti userA boleh menjalankan dua perintah apt tanpa password. Baris kedua berarti userA boleh melihat status service apa pun, tetapi tetap mengikuti kebijakan autentikasi normal.

Langkah 2: Verifikasi aturan yang aktif dan uji hasilnya.

```
sudo -l -U userA
sudo grep "userA" /var/log/auth.log | tail -10
```
sudo -l -U userA dipakai untuk mengecek aturan yang aktif dari sudut pandang akun userA. Log di /var/log/auth.log membantu memverifikasi bahwa pemakaian sudo benar-benar tercatat.

### Analisis 9.4
1. Mengapa aturan disimpan di /etc/sudoers.d//, bukan langsung di /etc/sudoers?
2. Mana perintah yang bisa dijalankan tanpa password, dan mana yang masih perlu autentikasi?
3. Informasi apa saja yang dicatat di log sudo?

### Tantangan 9.4

Tambahkan satu aturan baru agar userA boleh menjalankan /bin/systemctl restart ssh tetapi tidak boleh menjalankan reboot

### Jawaban Analisis 9.4

1. Karena menyimpan konfigurasi dalam direktori /etc/sudoers.d/ membuat manajemen hak akses menjadi lebih rapi. Setiap user atau program memiliki konfigurasi masing-masing. Dan juga mencegah kerusakan sistem, jika mengedit file utama /etc/sudoers dan terjadi kesalahan, fungsi sudo pada sistem linux bisa rusak atau terkunci. Dan juga agar aturan kustom agar tidak terhapus secara tidak sengaja saat ada update sistem operasi.

2. * Tanpa Password : Perintah /usr/bin/apt update dan /usr/bin/apt upgrade.Karena terdapat deklarasi tag NOPASSWD: setelah kedua perintah tersebut.

   * Autentikasi : bin/systemctl status *. Karena tidak diberikan tag khusus, userA harus memasukkan password akunnya sendiri terlebih dahulu saat mengeksekusi perintah  dengan sudo.

3. * Informasi audit keamanan yang mencangkup :
     
Waktu dan Tanggal eksekusi perintah.

Nama User yang menjalankan perintah sudo.

Direktori kerja (Directory) saat perintah dipanggil.

Identitas Target (perintah dijalankan sebagai user apa, biasanya sebagai root).

Nama Perintah Lengkap beserta argumen/opsi yang dieksekusi.

Status Keberhasilan: Apakah perintah tersebut sukses dijalankan atau ditolak (command not allowed/failed).

### jawaban tantangan 9.4

<img width="471" height="174" alt="image" src="https://github.com/user-attachments/assets/317a6321-5a15-43d6-956c-c9b16767666b" />

## Praktikum 9.5 — Disk Quota

Langkah 1: Buat image filesystem kecil dan mount dengan opsi quota.

```
sudo dd if=/dev/zero of=/tmp/quota-test.img bs=1M count=100
sudo mkfs.ext4 /tmp/quota-test.img
sudo mkdir-p /mnt/quota-test
sudo mount-o loop,usrquota,grpquota /tmp/quota-test.img /mnt/
quota-test
```
Image file dipakai agar praktikum aman: Anda tidak perlu memodifikasi filesystem utama seperti /home/. Opsi usrquota,grpquota mengaktifkan dua jenis quota sekaligus.

Langkah 2: Buat database quota dan aktifkan enforcement.

```
sudo quotacheck-cug /mnt/quota-test
sudo quotaon-v /mnt/quota-test
sudo repquota /mnt/quota-test
```
quotacheck-cug membuat database user dan group quota. Setelah itu, quotaon mengaktifkan enforcement, dan repquota menampilkan laporan awal.

Langkah 3: Tetapkan quota untuk user uji dan amati hasilnya.

```
sudo edquota-u userA
# contoh: soft block 5120,  block 10240
sudo repquota /mnt/quota-test
```
Nilai di atas memakai satuan KB. Jadi 5120 berarti sekitar 5 MB, dan 10240 berarti sekitar 10 MB.

Langkah 4: Bersihkan lingkungan uji setelah selesai.

```
sudo quotaoff /mnt/quota-test
sudo umount /mnt/quota-test
sudo rm /tmp/quota-test.img
```

### Analisis 9.5

1. Apa perbedaan soft limit dan hard limit saat quota mulai terlampaui?
2. Mengapa praktikum ini memakai loopback filesystem, bukan langsung /home/?
3. Dari output repquota, informasi apa yang menunjukkan quota sudah aktif?

### Tantangan 9.5

Coba atur quota baru untuk userA dengan batas inode yang sangat kecil, kemudian jelaskan kapan pembatasan inode lebih penting daripada pembatasan block.

### Jawaban Analisis 9.5
1. Soft Limit Berfungsi sebagai batas peringatan (warning). Fakta teknisnya, saat ukuran data user menyentuh angka ini, sistem hanya akan memberikan peringatan, tetapi user masih diizinkan menyimpan data baru. Namun, ini memicu berjalannya grace period (masa tenggang, biasanya 7 hari). Jika masa tenggang habis dan user belum mengurangi datanya di bawah soft limit, maka hak akses menulis (write) akan dicabut sepenuhnya.

   Hard Limit  Berfungsi sebagai batas mutlak (absolute). Sistem akan bertindak tanpa ampun. Jika kapasitas file menyentuh tepat di angka ini, sistem langsung memblokir secara fisik operasi penulisan data selanjutnya. User akan mendapatkan pesan error "Disk quota exceeded" dan tidak bisa lagi menambah file sekecil apa pun.


2. Alasannya keamanan dan isolasi risiko. Partisi /home/ adalah pusat tempat semua profil user dan file penting sistem berjalan. Mengedit sistem kuota langsung di filesystem utama sangat berisiko fatal, terutama jika terjadi salah ketik seperti mengisi limit pada kolom yang salah. Bisa membuat akun utama terkunci dan gagal login.

Dengan loopback filesystem,  disimulasikan sebuah partisi hard disk mandiri di dalam sebuah file .img kosong. Jika konfigurasinya hancur atau berantakan, sistem operasi Ubuntu Server kamu akan tetap hidup sehat; kita tinggal menghapus file .img tersebut tanpa mengorbankan partisi utama.

### tantangan 9.5

<img width="473" height="194" alt="image" src="https://github.com/user-attachments/assets/2be7cefb-c910-4461-8066-bf138163a4f0" />

Penjelasan :

Pembatasan inode lebih penting karena Inode Exhaution (Menangkal Serangan). Melindungi Layanan Server Web dan Cache dll...


## 1.7 Latihan

### Latihan Latihan 9.A — Audit dan Kolaborasi

1. Temukan file SUID aktif dengan find /-perm-4000-type f 2>/dev/null, lalu jelaskan tiga file yang Anda kenali beserta alasannya.
   
Jawaban :

1. /usr/bin/passwd
   Perintah ini bertugas mengubah password akun. Dengan SUID, userA(user biasa) memakai hak root sejenak saat menjalankan perintah agar bisa memperbarui passwordnya sendiri di file shadow tersebut.

2. /usr/bin/sudo
   Ini adalah jantung dari pendelegasian administrator. Perintah ini wajib memiliki SUID agar ketika kamu masuk sebagai user biasa, kamu bisa mengeksekusi perintah lain sebagai root. Tanpa SUID, perintah sudo tidak akan berfungsi sama sekali karena tidak memiliki kekuatan untuk menaikkan level hak akses.

3. /bin/su
Berfungsi untuk berpindah akun (Switch User). Sama seperti passwd, proses memverifikasi apakah akun tujuan valid dan mengecek password sistem memerlukan hak akses administrati


2. Cari direktori world-writable dan tentukan mana yang valid dan mana yang berisiko.

1. Direktori Valid

* /tmp, var/tmp, /dev/shm (shared memory), dan /run/lock. Karena Direktori-direktori ini memang diciptakan oleh sistem operasi Linux agar semua aplikasi dan user bisa menaruh file sementara (temporer).

2. Direktori yang beresiko

* /var/www/html/ atau /home/data_kantor lalu memberi hak akses chmod 777 tanpa menambahkan Stick Bit. Karena anpa pelindung Sticky Bit, folder tersebut menjadi ladang bebas. Siapa pun (termasuk program jahat/penyusup) memiliki kebebasan absolut untuk menghapus data penting milik orang lain atau menimpa sistem dengan file bervirus.


3. Rancang konfigurasi permission standar dan ACL untuk direktori proyek /srv/webapp/ agar group webapp-team dapat menulis, user deploy hanya membaca, dan file baru selalu mewarisi group proyek

Jawaban :

```
# 1. Buat direktori proyek jika belum ada
sudo mkdir -p /srv/webapp/

# 2. Ubah kepemilikan grup direktori menjadi webapp-team
sudo chown :webapp-team /srv/webapp/

# 3. Terapkan Standard Permission tingkat lanjut dengan bit SGID (Angka 2)
sudo chmod 2775 /srv/webapp/

# 4. Berikan akses baca (Read & Execute) secara spesifik kepada user 'deploy' pada direktori saat ini
sudo setfacl -m u:deploy:r-x /srv/webapp/

# 5. Pasang Default ACL agar user 'deploy' SELALU hanya bisa membaca file baru di masa depan
sudo setfacl -d -m u:deploy:r-x /srv/webapp/

# 6. Pasang Default ACL agar group 'webapp-team' SELALU memiliki akses menulis pada file baru
sudo setfacl -d -m g:webapp-team:rwx /srv/webapp/
```

### Latihan Latihan 9.B — Kebijakan Akun dan Quota

Tuliskan langkah untuk membuat user intern, menambahkannya ke group labgroup, memaksa pergantian password tiap 45 hari (warning 7 hari), memberi izin sudo hanya untuk systemctl status, dan menetapkan quota ruang serta inode sederhana pada /home/.

Jawaban :

Langkah 1: Manajemen Akun dan Grup

Fakta teknisnya, sebelum memasukkan user ke dalam sebuah grup, grup tersebut harus sudah ada di dalam sistem. Jika belum, eksekusi pembuatan grup terlebih dahulu.

1. Buat grup labgroup:

```
sudo groupadd labgroup
```

2. Buat user intern beserta direktori /home/-nya dan masukkan ke grup:

```
sudo useradd -m -s /bin/bash -G labgroup intern
```
3. Berikan password awal untuk intern:

```
sudo passwd intern
```

Langkah 2: Kebijakan Kedaluwarsa Password

Untuk memaksa pergantian password sesuai spesifikasi soal, kita menggunakan utilitas chage (Change Age).

* Terapkan batas 45 hari dan peringatan 7 hari:

```
sudo chage -M 45 -W 7 intern
```

Langkah 3: Isolasi Hak Akses Sudo

Memberikan akses sudo penuh kepada user intern (magang) adalah celah keamanan besar. Fakta mutlaknya, kita harus membatasi hak istimewa tersebut langsung dari jantung konfigurasi sudo.

1. Buka file konfigurasi sudoers (Wajib pakai visudo agar tidak merusak sistem jika salah ketik):

```
sudo visudo
```
2. Tambahkan aturan spesifik:

```
intern ALL=(ALL) /bin/systemctl status
```
3. Simpan dan keluar (tekan Ctrl+O, Enter, Ctrl+X).
(Fakta: Aturan ini secara teknis membaca: "User intern di terminal mana pun ALL, boleh mengeksekusi perintah sebagai siapa pun (ALL), TETAPI hanya sebatas perintah /bin/systemctl status. Jika ia mencoba sudo apt update atau sudo systemctl restart, sistem akan langsung menolaknya.)

Langkah 4: Penetapan Quota pada /home/

Karena sebelumnya kamu sudah berpengalaman menggunakan cara instan (setquota), kita akan pakai cara itu lagi untuk menetapkan quota block (kapasitas) dan inode (jumlah file) dalam satu kali ketukan keyboard.

Misalnya, kita tetapkan batas sederhana: maksimal kapasitas 50MB (soft) dan 100MB (hard), serta maksimal jumlah file 1000 (soft) dan 1500 (hard).

* Eksekusi penetapan quota secara langsung:

```
sudo setquota -u intern 51200 102400 1000 1500 /home

```


   












   


















