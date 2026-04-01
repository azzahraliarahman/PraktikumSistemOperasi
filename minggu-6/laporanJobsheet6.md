# Laporan Praktikum Jobsheet 6 Manajemen Proses

<h4>Nama : Azzahra Aulia Rahman<h4>
<h4>NIM : 254107020227
<h4>Kelas : TI-1H<h4>

## Praktikum 6.1 — Melihat Proses dan Thread

1. Tampilkan semua proses yang berjalan:

```
ps aux
```
2. Tampilkan proses beserta thread-nya, dapat dilihat pada kolom LWP (Light Weight Process ID):

```
ps aux-L
```
3. Lihat PID shell aktif dan detail prosesnya

```
echo $$
ps-p $$-f
```
4. Lihat hierarki proses secara visual:

```
pstree -p
```
### Pertanyaan Latihan 6.1
Jalankan ps aux dan amati outputnya:
1. Berapa total proses yang berjalan? Proses apa yang memiliki PID
terkecil?
2. Jalankan pstree-p dan temukan proses bash Anda. Proses apa yang
menjadi induk (PPID) dari bash tersebut?
3. Bandingkan output ps aux dan ps aux-L. Apa perbedaan yang Anda
lihat?

### Jawaban Latihan 6.1
1. Total proses yang ada, ada 101 baris, tapi yang sebenarnya ada 100 baris karena baris pertama adalah header. Proses yang memiliki PID terkecil yaitu 1 adalah /sbin/init.
2. Induk (PPID) dari proses bash  adalah login dengan nomor PID 994.
3. ps aux menampilkan daftar proses. Sedangkan ps aux -L Menampilkan daftar thread. Dan ada kolom tambahan LWP (Light Weight Process / ID Thread) dan NLWP (Jumlah total thread).


