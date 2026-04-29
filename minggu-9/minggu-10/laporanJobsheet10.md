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
