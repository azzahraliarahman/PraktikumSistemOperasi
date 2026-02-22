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

*lsblk

<img width="568" height="168" alt="Screenshot 2026-02-21 141752" src="https://github.com/user-attachments/assets/59ce8893-cac3-4fe9-8019-7a7d43bce9a2" />

*sudo fdisk -l


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

     
    


