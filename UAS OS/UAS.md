
# Laporan UAS Remastering


## 1 Persiapan Sistem dasar

### Download Ubuntu dekstop 24.04 LTS pada https://releases.ubuntu.com/24.04/

<img width="493" height="413" alt="image" src="https://github.com/user-attachments/assets/c5d2141d-ef39-4298-bc4b-01a9a8a0425a" />



### Instal Aplikasi Cubic

<img width="607" height="472" alt="image" src="https://github.com/user-attachments/assets/09d46f74-ed4c-4c06-be09-2dfe32e8c58b" />


```
sudo apt update
sudo apt install software-properties-common -y
sudo add-apt-repository ppa:cubic-wizard/release
sudo apt update
sudo apt install cubic -y
```



###  Memulai proyek Remastering Baru

1. Buka aplikasi cubic

```
cubic
```
2. Memilih Folder Proyek (Project Directory)

* Cubic memerlukan sebuah folder khusus untuk menyimpan file kerja sementara dan hasil akhir ISO nanti.

Pada kotak isian di layar awal Cubic, klik tombol browse (ikon folder/titik tiga).  Lalu, Buat folder baru kosong di dalam direktori Home kamu, beri nama Remaster-UAS. Lalu, Pilih folder tersebut, lalu klik tombol Next di pojok kanan atas aplikasi Cubic.



3. Memilih File ISO Ubuntu Asli

Pada kolom Original ISO, klik dan arahkan ke file ISO Ubuntu asli yang sudah kamu siapkan.  Pada kolom nama file keluaran (Output ISO volume ID/Filename), ubah namanya menjadi format tugasmu:
Ubuntu-Custom-[NIM].iso (Ganti [NIM] dengan Nomor Induk Mahasiswamu yang asli).  Jika sudah, klik Next lagi dan tunggu sampai Cubic selesai mengekstrak isi ISO tersebut. Setelah selesai diekstrak, Cubic akan otomatis membukakan sebuah kotak terminal hitam khusus (lingkungan chroot).  


### PIlih File iso

<img width="470" height="374" alt="image" src="https://github.com/user-attachments/assets/fffa9c94-5ab2-4a00-a68b-436d3683dfcc" />

### Proses Mengekstrak File

<img width="454" height="340" alt="image" src="https://github.com/user-attachments/assets/60afd467-00b9-4049-b5d9-7d84bf7471c9" />

### Layar Hitam yang muncul setelah ekstrak file

<img width="457" height="355" alt="image" src="https://github.com/user-attachments/assets/90ce2e27-0b4b-4a6a-8bcb-bb9b23b9e302" />

## 2. Kustomisasi dan Instalasi Aplikasi

1 Instalasi Aplikasi Dasar dan Web Server

_Dilakukan pembaruan indeks repositori Ubuntu, dilanjutkan dengan instalasi aplikasi multimedia (VLC), editor grafis (GIMP), serta web server (Apache2 dan PHP) secara serentak._

```
apt update
apt install wget vlc gimp apache2 php libapache2-mod-php -y
```

2 Instalasi Visual Studio Code

_file instalasi paket Debian (.deb) ditarik langsung dari server Microsoft menggunakan utilitas wget, lalu dieksekusi ke dalam sistem._

```
wget -O vscode.deb "https://code.visualstudio.com/sha/download?build=stable&os=linux-deb-x64"
apt install ./vscode.deb -y
```
<img width="478" height="314" alt="image" src="https://github.com/user-attachments/assets/8525b2fc-cb34-4d5b-9569-d110acab8bb5" />


3 Pembuatan Aplikasi Kustom (Bash Script Informasi Hardware)

_Bash Script otomatis untuk mengecek spesifikasi hardware._

* Langkah Eksekusi Script:

  1. Masuk ke direktori profil sistem:

  ```
  cd /etc/profile.d
  ```
  2. Membuat file dan menyuntikkan baris kode menggunakan perintah cat:

  ```
  cat << 'EOF' > cek_hardware
  #!/bin/bash
  echo "=============================================="
  echo "      SISTEM INFORMASI HARDWARE CUSTOM        "
  echo "=============================================="
  echo "Tanggal Cek   : $(date)"
  echo "Nama Hostname : $(hostname)"
  echo "Versi OS      : $(grep 'PRETTY_NAME' /etc/os-release | cut -d'=' -f2 | tr -d '\"')"
  echo "Model CPU     : $(lscpu | grep 'Model name' | cut -d':' -f2 | sed -e 's/^[ \t]*//')"
  echo "Total Memori  : $(free -h | grep 'Mem:' | awk '{print $2}')"
  echo "Sisa Disk (/) : $(df -h / | awk 'NR==2 {print $4}')"
  echo "=============================================="
  EOF
  ```
  
  3. Memberikan hak akses eksekusi (executable) agar script dapat dijalankan oleh sistem:

  ```
  chmod +x cek_hardware.sh
  ```

  ## 3. Kustomisasi tampilan

  1. Pemasangan Aset Visual Sistem

  File gambar latar belakang (wallpaper) kustom dipindahkan ke dalam direktori background bawaan sistem,  dilanjutkan dengan instalasi paket ikon pihak ketiga (Papirus) melalui repositori.
  <img width="599" height="414" alt="image" src="https://github.com/user-attachments/assets/e44d9f7d-21ac-442f-99b7-271fff58f1cc" />


  ```
  mv latar.jpg /usr/share/backgrounds/wallpaper-uas.jpg
  apt install papirus-icon-theme -y
  ```
  <img width="598" height="380" alt="image" src="https://github.com/user-attachments/assets/12281f20-a375-4331-b25a-7d436ec70601" />
  <img width="602" height="404" alt="Screenshot 2026-06-13 125344" src="https://github.com/user-attachments/assets/f71aecd3-932b-4562-8849-15b8b181a511" />


  2. Konfigurasi GSettings Schema Override

  Dibuat sebuah file modifikasi konfigurasi (99-kustom-uas.gschema.override) pada direktori inti glib-2.0/schemas/ untuk menimpa nilai default dari Desktop Environment Ubuntu (GNOME). Perubahan mencakup pengaturan parameter wallpaper, pengaktifan tema gelap (Dark Mode), dan penggantian paket ikon sistem.

 ```
cat << 'EOF' > /usr/share/glib-2.0/schemas/99-kustom-uas.gschema.override
[org.gnome.desktop.interface]
color-scheme='prefer-dark'
gtk-theme='Yaru-dark'
icon-theme='Papirus-Dark'

[org.gnome.desktop.background]
picture-uri='file:///usr/share/backgrounds/wallpaper-uas.jpg'
picture-uri-dark='file:///usr/share/backgrounds/wallpaper-uas.jpg'
EOF
```
  3. Kompilasi Skema Visual

```
glib-compile-schemas /usr/share/glib-2.0/schemas/
```

## 4. Pembuatan File ISO baru


<img width="482" height="347" alt="image" src="https://github.com/user-attachments/assets/5d6202f4-0d5e-45e0-900b-dab8dd93522e" />
<img width="73" height="86" alt="image" src="https://github.com/user-attachments/assets/7dd67bf5-301e-4f64-9746-0ba08a73b62c" />
<img width="634" height="455" alt="image" src="https://github.com/user-attachments/assets/cd3fb0d9-b50b-479f-b931-80629fc15e75" />





  

 

