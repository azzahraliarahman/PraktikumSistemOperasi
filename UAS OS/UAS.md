
#Laporan Panduan Membuat Custom Ubuntu ISO dengan Cubic

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








   
