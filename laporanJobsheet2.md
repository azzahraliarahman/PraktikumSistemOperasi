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

### Praktikum 2.10 — Substitusi dengan sed (Aman di File Latihan)

Langkah-langkah:

1. Siapkan file konfigurasi latihan:

* cat > config.txt << ’EOF’
* PORT=8080
* MODE=dev
* SERVICE_NAME=myserver
* EOF
* cat config.txt



  <img width="582" height="210" alt="Screenshot 2026-02-24 210812" src="https://github.com/user-attachments/assets/26615008-387a-48c5-a9aa-3d6de040cf22" />

 2. Ganti dev menjadi prod (tanpa mengubah file asli):

 * sed ’s/MODE=dev/MODE=prod/’ config.txt

<img width="640" height="94" alt="Screenshot 2026-02-24 211137" src="https://github.com/user-attachments/assets/9f1fb54a-15d4-4ec2-ae35-3b7dc602e2fd" />

3. Terapkan perubahan langsung ke file (-i):

* sed-i ’s/MODE=dev/MODE=prod/’ config.txt
* cat config.txt


<img width="656" height="107" alt="Screenshot 2026-02-24 211532" src="https://github.com/user-attachments/assets/655e0892-2293-482e-8f75-09de0857db76" />

4. Ganti semua kemunculan kata (g untuk global), contoh ubah myserver menjadi
node:
* sed-i ’s/myserver/node/g’ config.txt
* cat config.txt


<img width="630" height="116" alt="Screenshot 2026-02-24 211830" src="https://github.com/user-attachments/assets/c8207f4b-b006-488a-8335-7a40b4a1ef61" />

## Praktikum 2.11 — Ekstraksi Kolom dengan awk

1. Lihat output df-h:
   
* df-h


<img width="659" height="191" alt="Screenshot 2026-02-24 215748" src="https://github.com/user-attachments/assets/5fdb6ad6-d55b-4717-aaf3-b281dbfecde8" />

2. Ambil kolom filesystem dan persentase pemakaian:

* df-h | awk ’NR==1 {print $1, $5, $6} NR>1 {print $1,
$5, $6}’
* df-h | awk ’NR==1 || ($5+0) > 80 {print $1, $5, $6}’

<img width="821" height="210" alt="Screenshot 2026-02-24 220152" src="https://github.com/user-attachments/assets/21023d3e-1b51-440d-8238-ec8108473e0c" />

<img width="869" height="179" alt="Screenshot 2026-02-24 220739" src="https://github.com/user-attachments/assets/e85d8c13-9779-45cc-bb5d-8dd3af0554b9" />

### Praktikum 2.12 — Melihat Proses dengan ps

Langkah-langkah:

1. Tampilkan semua proses (format BSD):

* ps aux | head

<img width="869" height="179" alt="Screenshot 2026-02-24 220739" src="https://github.com/user-attachments/assets/f7402cc5-7399-4c79-a6f5-694e3f2f199c" />

2. Cari proses tertentu (misal sshd):

* ps aux | grep-i sshd
<img width="918" height="70" alt="Screenshot 2026-02-24 221805" src="https://github.com/user-attachments/assets/e96113f3-4810-4f7c-82d4-389062a32310" />

## Praktikum 2.13 — Monitoring Real-time dengan top

Langkah-langkah:
1. Jalankan top:

* top

<img width="1112" height="800" alt="Screenshot 2026-02-24 222048" src="https://github.com/user-attachments/assets/d2d1f972-32da-4f03-9ead-7017bb4902f8" />

## Praktikum 2.14 — Menghentikan Proses dengan kill

Langkah-langkah:

1. Jalankan proses dummy di background:

* sleep 300 &

<img width="417" height="48" alt="Screenshot 2026-02-24 222554" src="https://github.com/user-attachments/assets/6ee639ef-2b17-4b31-a10c-d2dad8dc1c2a" />

2. Cari PID proses sleep:
   
* ps aux | grep-E "sleep 300" | grep-v grep
  
<img width="657" height="69" alt="Screenshot 2026-02-24 222853" src="https://github.com/user-attachments/assets/52550187-b710-4764-b60b-60caaea5d385" />

3. Hentikan dengan SIGTERM:

* kill <PID_ANDA>

<img width="437" height="91" alt="Screenshot 2026-02-24 223148" src="https://github.com/user-attachments/assets/73800501-a7c0-4a00-8904-6eb74fc903d7" />


<img width="797" height="200" alt="Screenshot 2026-02-24 224142" src="https://github.com/user-attachments/assets/b9af6198-fd93-4881-8a7e-1df463993dc9" />

4. Verifikasi proses berhenti:

* ps aux | grep-E "sleep 300" | grep-v grep

<img width="793" height="66" alt="Screenshot 2026-02-24 223349" src="https://github.com/user-attachments/assets/cec9424b-208d-4310-9872-cf41ee0928b7" />

5. (Opsional) Jika proses sulit untuk dihentikan dan Anda membutukan untuk
menghentikan proses tersebut, gunakan SIGKILL:

* kill-9 <PID_ANDA>

<img width="445" height="82" alt="Screenshot 2026-02-24 224316" src="https://github.com/user-attachments/assets/c77fd15f-6812-47e1-a1cf-0e30e291c71e" />

## Praktikum 2.15 — Cek Disk, Load, dan Service

Langkah-langkah:

1. Cek penggunaan disk:

* df-h

<img width="627" height="196" alt="Screenshot 2026-02-24 224509" src="https://github.com/user-attachments/assets/cea97505-bddb-4bbe-b4ca-72d464d63d94" />

2. Cari direktori yang besar (contoh pada /var):

* sudo du-sh /var/* 2>/dev/null | sort-h | tail-n 10


<img width="798" height="253" alt="Screenshot 2026-02-24 224737" src="https://github.com/user-attachments/assets/ce262fc6-fb3d-4927-a07e-15378957b6d6" />

3. Cek load dan uptime:

* uptime

<img width="634" height="83" alt="Screenshot 2026-02-24 224854" src="https://github.com/user-attachments/assets/9c35684d-0f44-4e02-9e5d-5c757c8d7948" />

4. Cek service yang gagal:

* systemctl--failed

<img width="541" height="123" alt="Screenshot 2026-02-24 225028" src="https://github.com/user-attachments/assets/452f27d0-0acd-4c6a-8c8c-5d312c066ac7" />

5. Ambil log error terbaru (jika ada indikasi masalah):

* journalctl -xe | tail-n 50

<img width="1236" height="808" alt="Screenshot 2026-02-24 225154" src="https://github.com/user-attachments/assets/d7957b4a-dee5-48a8-ac96-00d2b4fb2537" />

## Praktikum 2.16 — Monitoring Port dan Koneksi
(Network Basics)

Tujuan: melihat interface, routing, dan port yang sedang listen (berguna untuk
troubleshooting service).

Langkah-langkah:
1. Lihat interface dan IP:

* ip a

<img width="846" height="293" alt="Screenshot 2026-02-24 225705" src="https://github.com/user-attachments/assets/852c33f7-5e8f-4761-af54-04ff00916a0c" />

2. Lihat routing table:

* ip r

<img width="644" height="125" alt="Screenshot 2026-02-24 225900" src="https://github.com/user-attachments/assets/3b6b79c6-939b-45b6-8210-4fdc24bc5ab0" />

3. Lihat port yang sedang listening:

* sudo ss-tulpn


<img width="1193" height="252" alt="Screenshot 2026-02-24 230038" src="https://github.com/user-attachments/assets/b691eded-bf93-44ea-827c-503a761273a1" />

### Pertanyaan Latihan 2.5

Pilih satu port yang listening dari output ss-tulpn(misal port 22), lalu
secara singkat.
tuliskan service/proses yang membukanya. Jelaskan kegunaan port tersebut

### Jawaban Latihan 2.5

1. Identitas Proses yang Membuka Port
Layanan yang bertanggung jawab membuka port 22 adalah sshd (Secure Shell Daemon). Dalam struktur sistem operasi Linux, daemon ini berjalan sebagai proses latar belakang yang secara konsisten memantau permintaan koneksi masuk pada antarmuka jaringan. Hal ini dapat diverifikasi pada kolom Process yang menampilkan nama program beserta Process ID (PID) uniknya.

2. Kegunaan dan Fungsi Protokol
Port 22 merupakan port standar yang dialokasikan untuk protokol SSH (Secure Shell). Fungsi utamanya adalah menyediakan jalur komunikasi data yang aman melalui enkripsi tingkat tinggi. Protokol ini memungkinkan administrator untuk melakukan login jarak jauh (remote login) dan mengelola konfigurasi server secara efisien tanpa harus berada di depan konsol fisik mesin. Selain itu, port ini juga memfasilitasi transfer berkas secara aman menggunakan sub-protokol seperti SFTP atau SCP.

## 1.9 Latihan

### Pertanyaan Latihan 2.A

Jalankan lspci-nnk. Pilih 1 perangkat PCI dan tuliskan: nama perangkat,
ID vendor:device, dan kernel driver in use.

### Jawab Latihan 2.A



<img width="1193" height="845" alt="Screenshot 2026-02-24 230952" src="https://github.com/user-attachments/assets/e85dd5bb-c635-4017-80bb-02487e1953e3" />


Nama Perangkat: Ethernet controller: Intel Corporation 82540EM Gigabit Ethernet Controller.

ID Vendor:Device: [8086:100e].

Kernel Driver in Use: e1000.

### Pertanyaan Latihan 2.B

Tentukan device root filesystem dengan findmnt /. Lalu cocokkan dengan
lsblk-f dan tuliskan tipe filesystem serta UUID-nya

### Jawaban Latihan 2.B

1. Identifikasi Device Root Filesystem
Berdasarkan struktur pada output lsblk -f, perangkat yang dikonfigurasi sebagai root filesystem (ditandai dengan mountpoint /) adalah:

Nama Device: ubuntu--vg-ubuntu--lv

Source Path: /dev/mapper/ubuntu--vg-ubuntu--lv

2. Tipe Filesystem dan UUID
Melalui pencocokan data pada kolom FSTYPE dan UUID di output lsblk -f, diperoleh rincian sebagai berikut:

Tipe Filesystem: ext4

UUID: 0df36a76-4aa1-4495-97b0-1236988a8a7e

### Pertanyaan Latihan 2.C

Buat file server.log berisi minimal 10 baris dengan variasi kata: INFO,
WARN, ERROR. Gunakan grep untuk menampilkan hanya baris ERROR

### Jawaban latihan 2.C

<img width="1034" height="159" alt="Screenshot 2026-02-24 232313" src="https://github.com/user-attachments/assets/3fbbd7ea-70f7-4003-ac18-9efda8aa4e3a" />

### Pertanyaan Latihan 2.D

Gunakan sed untuk mengganti semua kata server menjadi node pada file
latihan. Tunjukkan sebelum dan sesudah.

### Jawaban Latihan 2.D

sebelum :

<img width="386" height="67" alt="Screenshot 2026-02-24 233023" src="https://github.com/user-attachments/assets/592cc9d1-18f7-460f-8737-3fab6eeada51" />

sesudah :

<img width="573" height="53" alt="Screenshot 2026-02-24 233114" src="https://github.com/user-attachments/assets/54a36ec4-9b79-450b-8c06-3860bf75314e" />

### Pertanyaan Latihan 2.E

Gunakan df-h lalu awk untuk menampilkan filesystem yang penggunaan disk
di atas 70%

### Jawaban latihan 2.E

Berikut adalah perintah untuk menampilkan filesystem yang penggunaan disknya di atas 70%:


* df -h | awk 'NR==1 || ($5+0) > 70 {print $1, $5, $6}'

### Pertanyaan Latihan 2.F

Jalankan sleep 600 &. Temukan PID-nya dengan ps. Hentikan dengan
SIGTERM. Jelaskan beda SIGTERM vs SIGKILL

### Jawaban Latihan 2.F


<img width="761" height="135" alt="Screenshot 2026-02-24 233951" src="https://github.com/user-attachments/assets/9da693cf-b9e1-4863-a952-fca49a0ce58a" />

Perbedaan  SIGTERM vs SIGKILL :

Sigterm memberhentikan secara perlahan sedangkan Sigkill memberhentikan secara paksa.

### Pertanyaan Latihan 2.G

Gunakan systemctl–failed. Jika tidak ada yang gagal, pilih satu service
aktif (misal ssh) dan tampilkan status serta 30 baris log terakhirnya.

### Jawaban latihan 2.G



<img width="700" height="281" alt="Screenshot 2026-02-24 234606" src="https://github.com/user-attachments/assets/77de4479-c8b9-4f39-adc2-b5c666af9d67" />

