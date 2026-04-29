<h4>Nama : Azzahra Aulia Rahman</h4>
<h4>NIM : 254107020227</h4>
<h4>Kelas : 1H</h4>

# Jobsheet 10 : Manajemen Memori & System Call

## Praktikum 10.1 Melihat Penggunaan Memori

Langkah 1: Jalankan free -h untuk melihat ringkasan RAM dan swap.

```
free-h
```
Langkah 2: Lihat detail memori dari kernel melalui /proc/meminfo.

```
cat /proc/meminfo | head-n 20
```
### Studi Kasus 10.1 Server Lambat karena Memori

Langkah 1: Periksa kondisi memori secara keseluruhan.

```
free-h
```
Langkah 2: Pantau proses secara real-time.

```
top
```

### Hasil Studi Kasus 10.1

* free -h
 <img width="974" height="196" alt="Screenshot 2026-04-29 091626" src="https://github.com/user-attachments/assets/ef510fb1-4df7-4916-8e03-c908f7f26223" />

* top

<img width="896" height="1117" alt="Screenshot 2026-04-29 091714" src="https://github.com/user-attachments/assets/16958cf7-0a9f-4b34-ab3b-d2fae8b517c9" />

## Praktikum 10.2 Mengamati Aktivitas Paging

Langkah 1: Jalankan vmstat dengan interval 1 detik, 5 sampel.

```
vmstat 1 5
```

## Praktikum 10.3 Membuat dan Mengonfigurasi Swap

Langkah 1: Buat file berukuran 512 MB sebagai calon swap.

```
sudo fallocate -l 512M /swapfile-week10
```
Langkah 2: Atur permission file menjadi 600 — hanya root yang boleh membaca
dan menulis.

```
sudo chmod 600 /swapfile-week10
```
Langkah 3: Format file sebagai area swap, lalu aktifkan.

```
sudo mkswap /swapfile-week10
sudo swapon /swapfile-week1
```
Langkah 4: Verifikasi swap aktif. Anda akan melihat entri /swapfile-week10
dengan ukuran 512M, dan nilai total pada baris Swap di free-h bertambah 512M

```
swapon --show
free -h
```
Langkah 5: Periksa nilai swappiness, ubah sementara, dan verifikasi perubahan.

```
cat /proc/sys/vm/swappiness
sudo sysctl vm.swappiness=10
cat /proc/sys/vm/swappiness
```

## Praktikum 10.4 Monitoring Memory

Langkah 1: Ambil snapshot proses diurutkan dari penggunaan memori terbesar.

```
ps aux --sort=-%mem | head
```

Langkah 2: Pantau secara real-time dengan top

```
top
```
## Praktikum 10.5 Script Monitor Memori

```
cd ~/praktikum-os/week10-memory
nano monitor-memori.sh
```
Ketik script berikut:

```
#!/bin/bash
set -euo pipefail

THRESHOLD=20

echo "=== Monitor Memori ==="
date
echo

free -h
echo

AVAIL=$(free | awk '/Mem/ {printf "%d", $7/$2*100}')
if [ "$AVAIL" -lt "$THRESHOLD" ]; then
echo "PERINGATAN: Memori tersedia hanya ${AVAIL}%!"
else
