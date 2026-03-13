# Praktikum 3 – Operasi File dan Struktur Direktori

**Nama:** Azzahra Aulia Rahman  
**NIM:** 254107020227  
**Kelas:** TI-1H  

---

# Tugas Pendahuluan

## 1. Perintah Direktori

### pwd
Perintah `pwd` digunakan untuk menampilkan direktori kerja saat ini.

```bash
pwd
```

---

### cd
Perintah `cd` digunakan untuk berpindah direktori.

```bash
cd nama_direktori
```

Contoh:

```bash
cd Documents
```

---

### mkdir
Perintah `mkdir` digunakan untuk membuat direktori baru.

```bash
mkdir nama_direktori
```

Contoh:

```bash
mkdir latihan
```

---

### rmdir
Perintah `rmdir` digunakan untuk menghapus direktori kosong.

```bash
rmdir nama_direktori
```

---

# 2. Perintah Manipulasi File

### cp
Digunakan untuk menyalin file.

```bash
cp sumber tujuan
```

Contoh:

```bash
cp file1.txt file2.txt
```

---

### mv
Digunakan untuk memindahkan atau mengganti nama file.

```bash
mv sumber tujuan
```

Contoh:

```bash
mv file1.txt folder/
```

---

### rm
Digunakan untuk menghapus file.

```bash
rm nama_file
```

Contoh:

```bash
rm file1.txt
```

---

# 3. Perbedaan Hard Link dan Soft Link

### Hard Link
- Menghubungkan langsung ke inode file asli
- Jika file asli dihapus, data masih ada
- Harus berada pada partisi yang sama

Contoh:

```bash
ln file_asli file_link
```

---

### Soft Link (Symbolic Link)

- Shortcut menuju file asli
- Jika file asli dihapus maka link rusak
- Bisa berada pada partisi berbeda

Contoh:

```bash
ln -s file_asli file_link
```

---

# 4. Penjelasan Perintah

### file
Menampilkan tipe file.

```bash
file nama_file
```

---

### find
Digunakan untuk mencari file.

```bash
find direktori -name nama_file
```

---

### which
Menampilkan lokasi perintah.

```bash
which ls
```

---

### locate
Mencari file menggunakan database sistem.

```bash
locate nama_file
```

---

### grep
Digunakan untuk mencari teks dalam file.

```bash
grep kata file
```

---

# Praktikum

## Percobaan 1 – Direktori

Melihat direktori home:

```bash
pwd
echo $HOME
```

Output contoh:

```bash
/home/user
/home/user
```

---

Melihat direktori saat ini dan parent:

```bash
pwd
cd .
pwd
cd ..
pwd
cd
```

---

Membuat direktori:

```bash
mkdir A B C A/D A/E B/F A/D/A
ls -l
```

Struktur direktori:

```
.
├── A
│   ├── D
│   │   └── A
│   └── E
├── B
│   └── F
└── C
```

---

Menghapus direktori:

```bash
rmdir B
```

Error terjadi karena direktori tidak kosong.

Solusi:

```bash
rmdir B/F
rmdir B
```

---

## Percobaan 2 – Manipulasi File

Membuat file:

```bash
cat > contoh
Membuat sebuah file
Ctrl + D
```

---

Menyalin file:

```bash
cp contoh contoh1
ls -l
cp contoh A
cp contoh contoh1 A/D
```

---

Memindahkan file:

```bash
mv contoh contoh2
mv contoh1 contoh2 A/D
```

---

Menghapus file:

```bash
rm contoh2
rm -i contoh
rm -rf A C
```

---

## Percobaan 3 – Symbolic Link

```bash
echo "Hallo apa khabar" > halo.txt
ls -l
ln halo.txt z
ls -l
cat z
```

Membuat symbolic link:

```bash
ln -s z bye.txt
```

---

## Percobaan 4 – Melihat Isi File

```bash
file halo.txt
file bye.txt
```

Output contoh:

```bash
halo.txt: ASCII text
bye.txt: symbolic link
```

---

## Percobaan 5 – Mencari File

```bash
find /home -name "*.txt" -print > myerror.txt
cat myerror.txt
```

Menghitung baris file:

```bash
find . -name "*.txt" -exec wc -l {} \;
```

---

## Percobaan 6 – Mencari Text

```bash
grep Hallo *.txt
```

Output:

```bash
halo.txt: Hallo apa khabar
```

---

# Latihan

## Navigasi Direktori

```bash
cd
pwd
ls -al
cd .
pwd
cd ..
pwd
ls -al
cd /etc
ls -al | more
cat passwd
cd -
pwd
```

---

## Menelusuri Direktori Sistem

```bash
cd /bin
ls

cd /usr/bin
ls

cd /sbin
ls

cd /tmp
ls

cd /boot
ls
```

---

## Menelusuri Direktori /dev

```bash
cd /dev
ls
```

Mengetahui terminal:

```bash
who am i
```

---

## Menelusuri Direktori /proc

```bash
cat /proc/interrupts
cat /proc/devices
cat /proc/cpuinfo
cat /proc/meminfo
cat /proc/uptime
```

Direktori `/proc` disebut **pseudo filesystem** karena datanya berasal dari kernel dan RAM.

---

## Membuat Direktori

```bash
mkdir work play
```

---

## Menghapus Direktori

```bash
rmdir work
```

---

## Copy File passwd

```bash
cp /etc/passwd ~
```

---

## Memindahkan ke Direktori play

```bash
mv passwd play/
```

---

## Membuat Symbolic Link ke terminal

```bash
cd play
ln -s /dev/tty terminal
```

---

## Membuat File hello.txt

```bash
echo "hello world" > hello.txt
```

---

## Copy hello.txt ke terminal

```bash
cp hello.txt terminal
```

Teks akan muncul di terminal.

---

## Copy Direktori play ke work

```bash
ln -s play work
```

---

## Menghapus Direktori work

```bash
rm -rf work
```

---

# Analisa

Pada praktikum ini dipelajari bagaimana Linux mengelola file dan direktori dalam struktur hierarki yang dimulai dari root `/`.  
Perintah seperti `cd`, `pwd`, dan `ls` digunakan untuk navigasi direktori.  
Sedangkan perintah `cp`, `mv`, dan `rm` digunakan untuk manipulasi file.

Selain itu, praktikum ini juga memperkenalkan konsep **hard link dan symbolic link** yang memungkinkan satu file memiliki beberapa nama atau shortcut.

Direktori seperti `/proc` dan `/dev` merupakan direktori khusus yang menyimpan informasi kernel dan perangkat sistem.

---

# Kesimpulan

1. Sistem file Linux menggunakan struktur hierarki berbentuk tree yang dimulai dari root `/`.
2. Perintah dasar seperti `cd`, `pwd`, dan `ls` digunakan untuk navigasi direktori.
3. Manipulasi file dapat dilakukan menggunakan `cp`, `mv`, dan `rm`.
4. Hard link dan symbolic link memungkinkan satu file memiliki lebih dari satu referensi.
5. Direktori khusus seperti `/proc` dan `/dev` digunakan untuk informasi kernel dan perangkat sistem.