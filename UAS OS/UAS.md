
# Laporan UAS Remastering

## Unduh Berkas Checksum Resmi via Terminal

1. Buka Terminal di Ubuntu kamu (tekan Ctrl + Alt + T bersamaan).
2. Salin dan tempel (paste) perintah di bawah ini ke dalam terminal, lalu tekan Enter:

   ```
   cd ~/Downloads && wget https://releases.ubuntu.com/26.04/SHA256SUMS -O checksum-resmi.txt
   ```

## Instal Aplikasi Cubic

1. Perintah 1 (Tambah Repositori Cubic):

```
sudo add-apt-repository ppa:cubic-wizard/release --yes
```
<img width="317" height="244" alt="image" src="https://github.com/user-attachments/assets/6c4e08a9-c9e1-4a43-a4ff-67ec14f98597" />

2. Perintah 2 (Perbarui Daftar Paket):

```
sudo apt update
```
3. Install cubic

```
sudo apt install cubic -y
```

## Bab 7  Memulai proyek Remastering Baru

1. Buka aplikasi cubic

```
cubic
```
2. Memilih Folder Proyek (Project Directory)

* Cubic memerlukan sebuah folder khusus untuk menyimpan file kerja sementara dan hasil akhir ISO nanti.

Pada kotak isian di layar awal Cubic, klik tombol browse (ikon folder/titik tiga).  Lalu, Buat folder baru kosong di dalam direktori Home kamu, beri nama Proyek-Remaster. Lalu, Pilih folder tersebut, lalu klik tombol Next di pojok kanan atas aplikasi Cubic.

<img width="450" height="500" alt="Screenshot 2026-06-09 185301" src="https://github.com/user-attachments/assets/189b4117-5602-4247-9bac-c8497ccdc9fe" />


3. Memilih File ISO Ubuntu Asli

Pada kolom Original ISO, klik dan arahkan ke file ISO Ubuntu asli yang sudah kamu siapkan.  Pada kolom nama file keluaran (Output ISO volume ID/Filename), ubah namanya menjadi format tugasmu:
Ubuntu-Custom-[NIM].iso (Ganti [NIM] dengan Nomor Induk Mahasiswamu yang asli).  Jika sudah, klik Next lagi dan tunggu sampai Cubic selesai mengekstrak isi ISO tersebut. Setelah selesai diekstrak, Cubic akan otomatis membukakan sebuah kotak terminal hitam khusus (lingkungan chroot).  



Ketika file iso ubuntu belum duplikasi :

1. Buka Terminal

2. Salin dan tempel (paste) perintah di bawah ini untuk menduplikasi file ISO dari Windows ke folder Downloads lokal Ubuntu:

```
cp /media/sf_C_DRIVE/Users/Lenovo/Downloads/ubuntu-26.04-desktop-amd64.iso ~/Downloads/
```

* Prosesnya :

1. PIlih File iso

<img width="470" height="374" alt="image" src="https://github.com/user-attachments/assets/fffa9c94-5ab2-4a00-a68b-436d3683dfcc" />

2. Proses Mengekstrak File

<img width="454" height="340" alt="image" src="https://github.com/user-attachments/assets/60afd467-00b9-4049-b5d9-7d84bf7471c9" />

3. Layar Hitam yang muncul setelah ekstrak file

<img width="457" height="355" alt="image" src="https://github.com/user-attachments/assets/90ce2e27-0b4b-4a6a-8bcb-bb9b23b9e302" />

## Kustomisasi Sistem

1. Update Repositori Sistem

```
apt update
```
2. Instal Aplikasi (VLC & GIMP)

```
apt install -y vlc gimp
```

## menginstal VS Code serta paket web server (Apache2 & PHP).

1. Tambahkan Repositori VS Code

```
apt install -y wget gpg && wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > /usr/share/keyrings/packages.microsoft.gpg && echo "deb [arch=amd64 signed-by=/usr/share/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/keys/microsoft.asc main" > /etc/apt/sources.list.dist0/vscode.list && apt update
```
<img width="446" height="39" alt="image" src="https://github.com/user-attachments/assets/6c77dd1a-166b-4b52-9b21-8faca9d4556f" />

2. Instal VS Code, Apache2, dan PHP

```
apt install -y code apache2 php libapache2-mod-php
```
## Membuat Script Informasi Hardware

membuat Bash Script otomatis yang bisa mengecek spesifikasi hardware.

1. Masuk ke Folder Profile Sistem

```
cd /etc/profile.d
```
2. Buat File Script Baru Menggunakan Cat

```
cat << 'EOF' > cek_hardware.sh
```
3. Isi Konten Script-nya

```
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
4. Hak Akses Eksekusi

```
chmod +x cek_hardware.sh
```




   
