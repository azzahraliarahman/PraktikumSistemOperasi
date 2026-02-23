# Praktikum OS 1

<h6>Nama : Azzahra Aulia Rahman</h6>
<h6>NIM : 254107020227</h6>

## 1.6.4. Praktik: Instalasi Ubuntu Server 22.04 LTS

 1.Buka virtual machine 


 <img width="1271" height="866" alt="Screenshot 2026-02-20 223718" src="https://github.com/user-attachments/assets/f3cc44d0-5c24-48ac-b710-4007d51cad6e" /> 2. Jalankan Ubuntu servernya


<img width="1283" height="968" alt="Screenshot 2026-02-20 225120" src="https://github.com/user-attachments/assets/75de34f0-ee7f-4e54-ad88-34866d312d51" />

## 1.7.6. Praktik: Manual Partitioning

### Post-installation file System Commands

1. Check disk usage (df -h)
   
   

<img width="581" height="225" alt="Screenshot 2026-02-20 233451" src="https://github.com/user-attachments/assets/abd191e0-5d3a-4b42-a281-5fdcce0fdc9c" />

2. Check disk partitions

* lsblk

<img width="568" height="168" alt="Screenshot 2026-02-21 141752" src="https://github.com/user-attachments/assets/59ce8893-cac3-4fe9-8019-7a7d43bce9a2" />

* sudo fdisk -l


<img width="770" height="106" alt="Screenshot 2026-02-21 141849" src="https://github.com/user-attachments/assets/1e3299c5-7e8a-4744-a71e-d0bb9e77cff8" />

3. Check filesystem type

 *lsblk -f
 <img width="1109" height="166" alt="Screenshot 2026-02-21 142407" src="https://github.com/user-attachments/assets/a2763abc-f84d-42f1-8277-4b079d23be69" />
 *blkid

<img width="1041" height="103" alt="Screenshot 2026-02-21 142436" src="https://github.com/user-attachments/assets/e0d5e8ae-a89c-4926-b42e-2a2894a6b978" />

4. Mount additional filesystem 
* sudo mkdir /mnt/data
*  sudo mount /dev/sdb1 /mnt/data
  
 <img width="652" height="150" alt="Screenshot 2026-02-21 143335" src="https://github.com/user-attachments/assets/122a68dc-2edc-40b7-aa0d-a3870fdc06f4" />
 
<img width="610" height="144" alt="Screenshot 2026-02-21 192542" src="https://github.com/user-attachments/assets/0e5709e7-b20d-4c79-816b-ba7bc0d99049" />

5.  Permanent mount- edit /etc/fstab
   * cat /etc/fstab
     
<img width="843" height="200" alt="Screenshot 2026-02-21 193020" src="https://github.com/user-attachments/assets/14854f93-dc0b-4fc5-be4c-351793c8eef8" />
6.  UUID-based mount (preferred)

*  UUID=xxx-xxx-xxx /data ext4 defaults 0 2
<img width="824" height="238" alt="Screenshot 2026-02-21 194115" src="https://github.com/user-attachments/assets/3a23b9bd-ba4c-4cc7-bfbc-f502807602d9" />

## 1.10. Latihan

## 1.10.1. Latihan Konseptual

* Latihan 1.1

 5 fungsi utama Sistem Operasi beserta contohnya :
 1. Process management :
    OS bertugas mengatur aplikasi apa yang boleh berjalan, kapan waktunya, dan berapa banyak tenaga CPU yang boleh dipakai.
    * Windows : Menggunakan task manager.
    * Linux : Menggunakan perintah systemctl status ssh untuk mengecek apakah proses layanan SSH sedang berjalan di latar belakang.
  2. Memori management :
     OS mengatur pembagian RAM agar aplikasi tidak saling "bertabrakan" dan memastikan memori dibersihkan saat aplikasi ditutup.
     * macOS : Mempunyai fitur Compressed Memory yang secara otomatis mengecilkan data aplikasi yang tidak terpakai agar RAM memiliki space yang Lega.
     * Linux : Menggunakan Swap space untuk membantu RAM jika sudah penuh.
  3. File Management
     OS mengatur bagaimana data disimpan, diatur dalam folder, dan siapa saja yang boleh membukanya
     * Linux : Menggunakan sistem file seperti ext4 memakai format perintah mkfs.ext4.
     * Windows : Menggunakan sistem file NTFS dan file explorer untuk mengatur dokumen
   4. Device Management
      OS bertugas mengenali dan mengontrol perangkat keras (hardware) yang terhubung ke komputer melalui driver.
      *Windows : menggunakan Device Manager untuk mengecek printer atau webcam sudah terpasang dengan benar
      *Linux : Mengenali harddisk baru sebagai file di folder /dev.
   5. Security Management
      OS melindungi data dari pengguna yang tidak sah dan memberikan hak akses yang berbeda-beda.
      * Linux : Menggunakan perintah sudo (SuperUserDo), untuk memastikan hanya admin yang bisa mengubah file sistem.
      * macOS : Menggunakan fitur Touch ID atau FaceID untuk memverifikasi pengguna sebelum mengizinkan perubahan sistem.
     
  * Latihan 1.2
    
 Analisis use case :
 
1. Windows : Pilihan umum & profesional perkantoran
   Windows adalah OS paling fleksibel untuk pengguna umum dan industri kreatif perkantoran.
   * Gaming : Sangat bagus karena dukungan hardware dan driver paling lengkap, serta kompatibilitas dengan hampir semua judul game besar didunia.
   * Creative work : Pilihan yang tepat untuk desain grafis dan video karena mendukung penuh aplikasi industri seperti Adobe Creative Cloud.
   * Enterprise : Dominan di dunia perkantoran karena integrasi sempurna dengan microsoft office dan kemudahan manajemen perangkat bagi admin IT.
   * Development : Bagus untuk pengembangan aplikasi berbasis .NET/C# atau jika menggunakan fitur WSL (Windows Subsystem for Linux).
   * Server : Terbatas pada perusahaan yang menggunakan ekosistem Microsoft secara maksimal, seperti Active Directory atau SQL server.
  
 2. Linux : Infrastruktur & Spesialis Teknologi.
    Linux adalah fondasi teknologi dunia yang memberikan kontrol penuh bagi penggunanya.
    * Server : Unggul di pasar global untuk server web dan cloud karena stabilitasny,keamanan, dan efisiensinya.
    * Development : OS yang biasa dipakai pengembang karena terminal yang sangat kuat dan fleksibilitas dalam mengelola berbagai bahasa pemograman.
    * Cyber Security : Standar wajib untuk keamanan siber.
    * Gaming : berkembang pesat karena teknologi Steam Deck, namun tetinggal dengan windows dalam dukungan anti-cheat pada game daring tertentu.
    * Creative Work : Kurang disarankan karena perangkat seperti adobe Suite tidak tersedia di Linux.
   
  3. macOS : Kreatif & ekslusif
     macOS dikenal karena keindahan grafis dan stabilitas perangkat kerasnya.

     * Creative work : Standar Emas bagi editor video dan profesional audio karena manajemen warna yang akurat dan peforma rendering yang konsisten.
     * Development : Sistemnya berbasis Unix namun lebih ramah pengguna.
     * Enterprise : Banyak ditemukan pada perusahaan startup  atau agensi kreatif karena durabilitas perangkat kerasnya yang tinggi.
     * Server & Gaming : Memiliki keterbatasan dukungan perangkat keras pihak ketiga.
    
     ## 1.10.2. Latihan Praktikal

* Latihan 1.3

     Install Ubuntu Server 22.04 LTS di VirtualBox :

     <img width="1238" height="1352" alt="Screenshot 2026-02-22 143441" src="https://github.com/user-attachments/assets/f24536d9-87d5-482d-91e8-a24d7ba9b223" />

* Latihan 1.4

  Setelah instalasi Ubuntu Server, lakukan tasks berikut:


   1. Update package list: sudo apt update
      
  <img width="1044" height="879" alt="Screenshot 2026-02-22 144159" src="https://github.com/user-attachments/assets/a5d5d4e7-8a6e-4f84-bea4-826a5866b2ca" />

   2. Upgrade packages: sudo apt upgrade
   
<img width="580" height="197" alt="Screenshot 2026-02-22 144352" src="https://github.com/user-attachments/assets/f24b12f4-698e-4b95-a1b0-e1328cdc889a" />

   3. Install neofetch: sudo apt install neofetch

<img width="549" height="181" alt="Screenshot 2026-02-22 144515" src="https://github.com/user-attachments/assets/c712d277-80a6-460f-9fb9-74796f6139c9" />

   4. Check disk usage dengan df-h

<img width="626" height="206" alt="Screenshot 2026-02-22 144655" src="https://github.com/user-attachments/assets/b4b8bcef-1d64-4a23-ae23-92def3f7aa8e" />

   5. Check memory dengan free-h

  <img width="741" height="105" alt="Screenshot 2026-02-22 144853" src="https://github.com/user-attachments/assets/8695530e-d141-4bdf-9211-16b9ebd69756" />

  * Latihan 1.5 
Eksplorasi sistem yang baru diinstall: 

1. Tampilkan informasi OS: cat /etc/os-release 

<img width="748" height="323" alt="Screenshot 2026-02-22 193640" 
src="https://github.com/user-attachments/assets/489a2894-0c0b-4413-8e44
f3dcab89fdab" />

2. Tampilkan versi kernel: uname -r

<img width="271" height="75" alt="Screenshot 2026-02-22 194241" 
src="https://github.com/user-attachments/assets/4ebc0311-d348-417d-be64
6de7243d2a16" /> 

3. List partisi: lsblk
   
<img width="567" height="192" alt="Screenshot 2026-02-22 194426" 
src="https://github.com/user-attachments/assets/5968fb97-40dd-41db-9723
b63ad83f9c2b" /> 

4. Check network connectivity: ping-c 4 google.com
   
<img width="631" height="91" alt="Screenshot 2026-02-22 194624" 
src="https://github.com/user-attachments/assets/56055db7-a6df-46be-8204
79bf97432ec6" /> 

5.  Install dan jalankan htop untuk melihat resource usage

<img width="1149" height="898" alt="Screenshot 2026-02-22 195039" 
src="https://github.com/user-attachments/assets/64d32279-76b6-4a87-a27a
161f0925d283" /> 

6. Laporan Konfigurasi Sistem - Ubuntu Server
   
Nama User: zahra 

Sistem Operasi: Ubuntu 24.04.4 LTS 

Versi Kernel: 6.8.0-100-generic. 

Penyimpanan: 25 GB 

Konektivitas: Jaringan berfungsi dengan baik. 

Resource Usage: Pemantauan sistem dilakukan menggunakan tool htop.

## 1.10.3. Latihan Refleksi

* Latihan 1.6

 <h4> Saya biasa menggunakan windows, Dari saya sekolah dasar sampai sekarang, saya suka windows karena lebih umum dipakai kebanyakan orang. Dan lebih fleksibel. Tidak ada tantangan yang saya hadapi atau lebih tepatnya saya lupa. saya pernah menggunakan linux dan menurut saya pengoprasiannya lebih susah untuk dimengerti daripada windows. Tidak ada yang ingin saya coba karena menurut saya MacOS adalah sebuah obsesi terhadap apple</h4>
