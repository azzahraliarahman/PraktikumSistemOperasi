# Praktikum Sistem OS 2

<h4>Nama : Azzahra Aulia Rahman</h4>
<h4>NIM  : 254107020227</h4>
<h4>Kelas: TI-1H</h4>


## Praktikum 2.1 — Identifikasi CPU dan Memori

Tujuan: memahami spesifikasi CPU dan kondisi memori pada server/VM.

Langkah-langkah:

1. Tampilkan informasi CPU:
   
*  lscpu

<img width="600" height="913" alt="Screenshot 2026-02-23 175539" src="https://github.com/user-attachments/assets/ef8e58ab-72d7-45f2-ad58-2e88773f2f66" /> 1

2. Tampilkan ringkasan memori:

* free -h

<img width="600" height="162" alt="Screenshot 2026-02-24 082757" src="https://github.com/user-attachments/assets/3c2dbde7-d687-4a22-b61c-f60dbddfaf44" />
ram 1.9 swap 2.00

3. (Opsional) cek informasi hardware dari DMI/BIOS (butuh sudo):

* sudo dmidecode -t system

<img width="676" height="300" alt="Screenshot 2026-02-24 092118" src="https://github.com/user-attachments/assets/435651f7-4fdc-41b4-ac9e-5b18ca5cdb3a" />

### Perintah Latihan 2.1

Catat: (1) jumlah CPU(s), core/thread, (2) total RAM, (3) total swap. Je
laskan perbedaan RAM vs swap dalam 2–3 kalimat

### Jawaban latihan 2.1

1. Informasi Prosesor
   
Jumlah CPU(s): 2.

Core per Socket: 2.

Thread per Core: 1.

Model CPU: Intel(R) Core(TM) i5-10210U CPU @ 1.60GHz.

2. Informasi Memori
   
Total RAM: 1.9 GiB.

Total Swap: 2.0 GiB.

Perbedaan RAM vs Swap :

RAM (Random Access Memory) adalah memori utama yang sangat cepat dan digunakan untuk menyimpan data aktif yang sedang diproses oleh CPU, namun sifatnya sementara (volatil). Sementara itu, Swap adalah ruang cadangan pada disk (HDD/SSD) yang digunakan sistem operasi sebagai "memori tambahan" ketika RAM fisik sudah penuh, meskipun kecepatannya jauh lebih lambat dibandingkan RAM fisik.

## Praktikum 2.2 — Identifikasi Perangkat PCI/USB dan
Driver

Tujuan: mengenali perangkat PCI/USB dan melihat driver/modul yang dipakai.

Langkah-langkah:

1. Lihat daftar perangkat PCI:

* lspci


<img width="1054" height="219" alt="Screenshot 2026-02-24 095647" src="https://github.com/user-attachments/assets/688b9013-0df3-495c-8bb9-2bda353eea52" />

2. Lihat perangkat PCI beserta driver kernel yang digunakan:

* lspci -nnk


<img width="1180" height="526" alt="Screenshot 2026-02-24 095912" src="https://github.com/user-attachments/assets/200eac48-2fa3-46e5-9238-e054234e3aa8" />


3. Fokus pada NIC (Ethernet) untuk mencari modul driver:
   
* lspci-nnk | grep-A3-i ethernet

<img width="965" height="138" alt="Screenshot 2026-02-24 100430" src="https://github.com/user-attachments/assets/836f2452-c43b-4ea9-9de6-314679a662d5" />

4. Lihat perangkat USB:

* lsusb

<img width="555" height="128" alt="Screenshot 2026-02-24 102144" src="https://github.com/user-attachments/assets/3fef1d31-e2a4-4f12-a331-3fb6554c1877" />

5. Lihat topologi USB (tree):

* lsusb -t
  
<img width="696" height="166" alt="Screenshot 2026-02-24 102236" src="https://github.com/user-attachments/assets/832d8458-20ed-40db-9813-e454e6adcce6" />

### Perintah Latihan 2.2

Temukan 1 perangkat PCI (misal NIC) dan tuliskan: Vendor:Device ID (angka
heksadesimal), nama driver/modul kernel, dan deskripsi singkat fungsinya

### Jawaban Latihan 2.2

Identifikasi Perangkat PCI (NIC)

Vendor:Device ID: 8086:100e.

Nama Driver/Modul Kernel: e1000.

Deskripsi : Perangkat ini adalah Intel Corporation 82540EM Gigabit Ethernet Controller yang disimulasikan oleh VirtualBox. Fungsinya adalah sebagai kartu jaringan (Network Interface Card) yang mengelola komunikasi data antara sistem operasi Ubuntu dengan jaringan luar atau internet. Driver e1000 bertugas mengontrol perangkat keras tersebut agar kernel Linux dapat mengirim dan menerima paket data secara efisien.

## Praktikum 2.3 — Identifikasi Storage dan Filesystem

1. Lihat daftar disk/partisi:
* lsblk -f


<img width="1045" height="226" alt="Screenshot 2026-02-24 103830" src="https://github.com/user-attachments/assets/631dab05-c8b0-4615-bbc0-e6c4d318adc6" />


2. Tampilkan UUID dan tipe filesystem:

* sudo blkid
  
<img width="1260" height="175" alt="Screenshot 2026-02-24 104041" src="https://github.com/user-attachments/assets/5d2eb2b2-7513-49e8-b18f-de1b2dac9bc1" />

3. Lihat mount point untuk root filesystem:

* findmnt /

<img width="526" height="114" alt="Screenshot 2026-02-24 104245" src="https://github.com/user-attachments/assets/110f4ac6-0a9f-4027-af49-6d581fdc8161" />

## Praktikum 2.4 — Melihat Modul Aktif dan Informasinya 

Tujuan: mengenal modul aktif dan keterkaitannya dengan perangkat.

Langkah-langkah:

1. Cek versi kernel:

* uname -r


<img width="266" height="93" alt="Screenshot 2026-02-24 161507" src="https://github.com/user-attachments/assets/b4e40dd0-a468-419d-9e43-2f4731f54684" />

2. Tampilkan daftar modul aktif:

* lsmod | head

<img width="449" height="203" alt="Screenshot 2026-02-24 161937" src="https://github.com/user-attachments/assets/c7c4356d-194a-4988-aab8-7adab828456c" />

3. Pilih salah satu modul (contoh aman: loop) dan lihat detailnya:

* modinfo loop

<img width="695" height="203" alt="Screenshot 2026-02-24 162135" src="https://github.com/user-attachments/assets/087a7c10-1f1b-46d4-8913-aeb489dc537a" />

4. Muat modul (jika belum aktif), lalu verifikasi:

* sudo modprobe loop
  
* lsmod | grep -i loop


<img width="341" height="115" alt="Screenshot 2026-02-24 162556" src="https://github.com/user-attachments/assets/8a37860f-568b-43bd-bd12-5d7313bd3a80" />

5. (Opsional) lihat pesan kernel terbaru:

* dmesg -T | tail -n 20

<img width="716" height="180" alt="Screenshot 2026-02-24 165331" src="https://github.com/user-attachments/assets/fa52951c-d60a-4ae8-9b09-04cd3dca0c50" />

## Praktikum 2.5 — Konfigurasi Auto-load dan Blacklist

Tujuan: memahami cara membuat modul otomatis dimuat atau diblokir.

Fungsi konfigurasi:

• /etc/modules-load.d/*.conf: daftar modul yang di-load saat boot.

• /etc/modprobe.d/*.conf: aturan modprobe, termasuk blacklist modul.

Langkah demo (gunakan modul aman, contoh loop):

1. Buat file auto-load:

* echo "loop" | sudo tee /etc/modules-load.d/loop.conf

<img width="716" height="180" alt="Screenshot 2026-02-24 165331" src="https://github.com/user-attachments/assets/2c9679cd-8779-4d60-817a-efdf4c3b6809" />

2. Simulasikan verifikasi (tanpa reboot) dengan memastikan modul sudah aktif:

* lsmod | grep-i loop

<img width="320" height="75" alt="Screenshot 2026-02-24 170014" src="https://github.com/user-attachments/assets/e7c0101e-a0f4-489d-a83d-be507df1c22a" />

3. (Opsional, konsep) blacklist modul:

* #echo "blacklist loop" | sudo tee /etc/modprobe.d/
blacklist-loop.conf

<img width="714" height="44" alt="Screenshot 2026-02-24 170448" src="https://github.com/user-attachments/assets/cdee978c-4ccb-4333-9807-d3aa5fa2c636" />

## Praktikum 2.6 — Mengenali Block vs Character Device

Tujuan: membedakan perangkat disk vs terminal.

Langkah-langkah:

1. Lihat detail salah satu disk (sesuaikan dengan perangkat Anda, misal sda):

* ls -l /dev/sda

<img width="519" height="61" alt="Screenshot 2026-02-24 170951" src="https://github.com/user-attachments/assets/46c12eb8-420d-46b0-942f-d53cec82ba6a" />

2. Lihat detail device terminal:

* ls-l /dev/tty

<img width="462" height="93" alt="Screenshot 2026-02-24 194913" src="https://github.com/user-attachments/assets/92cc7ed7-4031-4b7a-bae0-9babb875b6dc" />

3. Lihat disk dan partisi untuk mengaitkan dengan /dev:

* lsblk
  
<img width="568" height="224" alt="Screenshot 2026-02-24 195112" src="https://github.com/user-attachments/assets/32f87277-23e2-45fd-b422-64f331aca6ef" />

### Pertanyaan Latihan 2.3 

Dari output ls-l, jelaskan perbedaan penanda file untuk block device dan
character device. (Hint: karakter pertama pada permission string)

### Jawaban latihan 2.3

Karakter palimg kiri pada output ls -l. Jika diawali c, maka itu adalah Character Device. Jika diawali dengan b, maka itu adalah block device.

## Praktikum 2.7 — Melihat Informasi udev

Tujuan: melihat metadata yang dipakai udev untuk membuat device node.

Langkah-langkah:

1. Cek atribut udev untuk disk:

* udevadm info--query=all--name=/dev/sda | head-n 30

<img width="709" height="528" alt="Screenshot 2026-02-24 200211" src="https://github.com/user-attachments/assets/21b6e728-e743-46c7-a676-7a4dcd62aaf5" />

2. (Opsional) monitor event udev (jalankan, lalu colok/lepas USB pada mesin
fisik):

* sudo udevadm monitor

<img width="842" height="636" alt="Screenshot 2026-02-24 200641" src="https://github.com/user-attachments/assets/75306952-3500-496b-a1b3-6dcbb24dc252" />

## Praktikum 2.8 — Membuat Workspace Praktikum

Tujuan: membuat area kerja aman untuk semua latihan bab ini.

Langkah-langkah:

1. Buat direktori praktikum dan masuk ke dalamnya:

* mkdir-p ~/praktikum-os/week02
* cd ~/praktikum-os/week02
* pwd

<img width="393" height="112" alt="Screenshot 2026-02-24 202754" src="https://github.com/user-attachments/assets/c11f643a-dd7f-49fb-8c15-d31f1e62503a" />

2. Buat beberapa file contoh:
* touch notes.txt data.log config.txt
* ls-lah

<img width="665" height="400" alt="Screenshot 2026-02-24 203201" src="https://github.com/user-attachments/assets/83ce3ffb-c07c-4ee7-9c0d-b0a8286ce32d" />

3. Isi file log contoh (simulasi):

* echo "INFO: service started" >> data.log
*  echo "WARN: disk usage high" >> data.log
* echo "ERROR: failed to connect" >> data.log
* cat data.log

<img width="622" height="71" alt="Screenshot 2026-02-24 203414" src="https://github.com/user-attachments/assets/f865a2ac-340c-4853-8b4f-98c299f295c4" />

<img width="746" height="212" alt="Screenshot 2026-02-24 203838" src="https://github.com/user-attachments/assets/3b78b14b-5afe-4042-bd9d-7309eb6d1ffe" />

4. Baca file dengan less:

* less data.log

<img width="488" height="128" alt="Screenshot 2026-02-24 203955" src="https://github.com/user-attachments/assets/6f936941-0342-46dc-acfa-6ae9046f71dd" />

## Praktikum 2.9 — Pencarian Pola dengan grep

Langkah-langkah:

1. Cari baris yang mengandung ERROR pada data.log:

* grep "ERROR" data.log

<img width="495" height="68" alt="Screenshot 2026-02-24 204416" src="https://github.com/user-attachments/assets/f0d832ff-e984-49ee-9a02-f052ef834bf5" />

2. Cari tanpa memperhatikan huruf besar/kecil:
* grep-i "error" data.log

<img width="520" height="52" alt="Screenshot 2026-02-24 205530" src="https://github.com/user-attachments/assets/acf1ef1f-eed0-4139-b064-d14d29f4e9e1" />

3. Tampilkan nomor baris:

* grep-n "WARN" data.log

<img width="527" height="54" alt="Screenshot 2026-02-24 204620" src="https://github.com/user-attachments/assets/da31fc0b-4a30-4b6e-999b-a3e3abbcdaf5" />

4. Tampilkan baris yang tidak cocok (invert match):

* grep-v "INFO" data.log

<img width="525" height="76" alt="Screenshot 2026-02-24 205713" src="https://github.com/user-attachments/assets/99733458-c9ee-456b-b7c8-543b4549f5c6" />

### Pertanyaan Latihan 2.4

Gunakan grep untuk menampilkan hanya baris yang mengandung INFO atau
WARN dari data.log. (Hint: gunakan grep-E dengan pola alternatif)

### jawaban Latihan 2.4

* grep -E "INFO|WARN" data.log

<img width="562" height="65" alt="Screenshot 2026-02-24 210103" src="https://github.com/user-attachments/assets/1e13cee6-7d41-4aad-9e9f-d65ded72e4c6" />
