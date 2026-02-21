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
    * Linux : Menggunakan perintah "systemctl status ssh" untuk mengecek apakah proses layanan SSH sedang berjalan di latar belakang.
  2. Memori management :
     OS mengatur 
    


