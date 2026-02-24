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

 *uname -r




## Description

An in-depth paragraph about your project and overview of use.

## Getting Started

### Dependencies

* Describe any prerequisites, libraries, OS version, etc., needed before installing program.
* ex. Windows 10

![A beautiful sunset over the ocean](images/sunset.jpeg "Golden Hour matahari")

### Installing

* How/where to download your program
* Any modifications needed to be made to files/folders

### Executing program

* How to run the program
* Step-by-step bullets
```
code blocks for commands
```

## Help

Any advise for common problems or issues.
```
command to run if program contains helper info
```

## Authors

Contributors names and contact info

ex. Dominique Pizzie  
ex. [@DomPizzie](https://twitter.com/dompizzie)

## Version History

* 0.2
    * Various bug fixes and optimizations
    * See [commit change]() or See [release history]()
* 0.1
    * Initial Release

## License

This project is licensed under the [NAME HERE] License - see the LICENSE.md file for details

## Acknowledgments

Inspiration, code snippets, etc.
* [awesome-readme](https://github.com/matiassingers/awesome-readme)
* [PurpleBooth](https://gist.github.com/PurpleBooth/109311bb0361f32d87a2)
* [dbader](https://github.com/dbader/readme-template)
* [zenorocha](https://gist.github.com/zenorocha/4526327)
* [fvcproductions](https://gist.github.com/fvcproductions/1bfc2d4aecb01a834b46)
