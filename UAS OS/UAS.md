
# Laporan Panduan Membuat Custom Ubuntu ISO dengan Cubic

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


   
