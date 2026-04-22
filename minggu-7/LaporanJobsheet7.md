<h4>Nama : azzahra Aulia Rahman </h4>
<h4>NIM : 254107020227</h4>
<h4>Kelas : 1H</h4>

# laporan Jobsheet 07 Bash Shell & shell Basic

## Tugas Praktikum 1 — Toolkit Bash Administrator Pribadi

### Konteks & Instruksi

*Konteks riil: seorang administrator sering mengulang perintah yang sama setiap
hari. Agar pekerjaan lebih efisien dan konsisten, ia perlu memiliki toolkit Bash pribadi yang otomatis aktif setiap login

* Instruksi :
  1. Tambahkan konfigurasi pada .bashrc untuk:
• menambahkan direktori bin pribadi ke PATH,
• membuat minimal 2 alias yang membantu kerja harian,
• membuat minimal 1 fungsi shell yang berguna untuk administrasi

3. Buat satu script sederhana di direktori bin pribadi, misalnya script untuk
menampilkan ringkasan sistem.
4. Uji dari direktori yang berbeda untuk memastikan script dapat dipanggil tanpa
menuliskan path lengkap.
5. Simpan bukti pengujian ke file toolkit-bash-report.txt.
Minimal luaran:
• isi blok konfigurasi yang ditambahkan ke .bashrc,
• output echo $PATH,
• output type untuk alias, fungsi, dan script,
• file laporan toolkit-bash-report.txt

### Jawaban (Langkah-langkah) :
1. <img width="692" height="328" alt="Screenshot 2026-04-14 111137" src="https://github.com/user-attachments/assets/7e8af400-3527-419c-a231-745372d7a4f0" />

2. <img width="287" height="135" alt="image" src="https://github.com/user-attachments/assets/64ea1afe-830b-4cd2-a323-24a113a922be" />

3. <img width="452" height="314" alt="Screenshot 2026-04-14 114038" src="https://github.com/user-attachments/assets/0d06d58e-a3d2-4564-9891-f06f6360e871" />

4. <img width="710" height="232" alt="Screenshot 2026-04-14 115243" src="https://github.com/user-attachments/assets/bb24386c-2411-4cc5-938d-89b6c55bba3b" />

5.<img width="948" height="171" alt="Screenshot 2026-04-14 142343" src="https://github.com/user-attachments/assets/b10b376a-255d-4636-9287-1d51077ab665" />

## Tugas Praktikum 2 — Audit File Konfigurasi dan Logging Aman

### Konteks & Instruksi

Konteks riil: saat troubleshooting, administrator sering perlu menginventarisasi
file konfigurasi dan memisahkan output normal dari pesan error.

Instruksi tugas:
1. Buat file laporan bernama audit-konfigurasi-$(date +%F).txt.
2. Cari file *.conf di dalam /etc dan simpan hasilnya ke file laporan.
3. Catat jumlah total file konfigurasi yang ditemukan.
4. Jika ada pesan error, simpan ke file terpisah, misalnya audit-error.log.
5. Tampilkan isi laporan ke terminal dan sekaligus simpan menggunakan tee.
6. Tambahkan ringkasan singkat 3–5 baris yang menjelaskan mengapa pemisahan
stdout dan stderr penting dalam audit sistem.

* Syarat konsep yang harus muncul:
1.8 Tugas Praktikum
• redirection >, 2>, atau &>,
• pipeline,
• tee,
• penggunaan variabel atau command substitution.
* Minimal luaran:
• file laporan audit,
• file log error,
• perintah yang digunakan,
• analisis singkat hasil audit.

### Jawaban :

1. <img width="896" height="576" alt="Screenshot 2026-04-14 180937" src="https://github.com/user-attachments/assets/22e3ea53-ddd6-483c-b0a6-13d42d882bc6" />

2. <img width="762" height="173" alt="Screenshot 2026-04-15 075052" src="https://github.com/user-attachments/assets/b81e6a21-7d62-4118-8400-7b70964de79a" />

3. <img width="796" height="152" alt="Screenshot 2026-04-15 075541" src="https://github.com/user-attachments/assets/065e4c9c-f457-4048-9178-6b39c0ca927c" />

4. nomor 4 sudah ada di no 2 _"2> audit-error.log_"

5. <img width="533" height="794" alt="Screenshot 2026-04-15 080203" src="https://github.com/user-attachments/assets/560b32b8-e2b9-4f2d-92a5-cfaaf558d8a1" />

6. <img width="933" height="106" alt="Screenshot 2026-04-15 081414" src="https://github.com/user-attachments/assets/1c0f4b23-40d6-47f1-a856-a353e86dc623" />

## Tugas Praktikum 3 — Mini Health Check Harian Server


### Konteks & Instruksi

Konteks riil: administrator perlu membuat pemeriksaan cepat (health check) untuk
mengetahui kondisi dasar server sebelum dan sesudah maintenance.

Instruksi tugas:
1. Buat script Bash bernama daily-healthcheck pada direktori bin pribadi.
2. Script minimal harus menampilkan:
• tanggal dan waktu,
• hostname,
• user aktif,
• shell aktif,
• uptime,
• penggunaan memori,
• penggunaan filesystem root,
• 10 baris terakhir history command yang relevan dengan pengecekan.
3. Simpan hasil ke file log harian, misalnya healthcheck-$(date +%F).log.
4. Tampilkan hasil ke terminal dan ke file secara bersamaan.
5. Jika Anda menggunakan pipeline dengan tee, cek juga status exit command utama.

Syarat konsep yang harus muncul:

• environment variable,
• PATH,
• alias atau fungsi pendukung,
• history,
• tee,
• penanganan error dasar.
Minimal luaran:
• file script yang executable,
• contoh isi file log hasil eksekusi,
• penjelasan singkat fungsi tiap bagian script.


Jawaban :

<img width="947" height="496" alt="Screenshot 2026-04-15 191459" src="https://github.com/user-attachments/assets/b677373d-070e-4dcd-a50c-4390f04d1bcc" />

## Tugas Praktikum 4 — Penanganan File dengan Nama Kompleks dan Arsip Aman

### Konteks & Instruksi

* Konteks riil: file hasil backup, ekspor, atau laporan sering memiliki nama yang
mengandung spasi atau karakter khusus. Administrator harus tetap dapat memproses
file-file tersebut tanpa salah target.

* Instruksi tugas:
1. Buat minimal 4 file contoh dengan nama yang bervariasi, termasuk:
• nama file yang mengandung spasi,
• nama file yang mengandung tanda kurung siku atau karakter khusus,
• file dengan pola nama serupa untuk diuji dengan wildcard.
2. Tunjukkan perbedaan hasil jika file diakses tanpa quoting dan dengan quoting
yang benar.
3. Lakukan preview wildcard dengan echo sebelum dipakai untuk operasi nyata.
4. Salin file-file tersebut ke direktori backup dengan nama yang aman.
5. Buat arsip tar.gz dari hasil backup.
6. Simpan riwayat perintah yang Anda gunakan ke file riwayat-arsip.txt.
Syarat konsep yang harus muncul:

• single quote, double quote, dan escaping,
1.9 Rangkuman
• wildcard,
• variabel path,
• history,
• operasi file lanjutan yang aman.
Minimal luaran:
• daftar file awal,
• daftar file hasil backup,
• file arsip tar.gz,
• file riwayat-arsip.txt,
• refleksi singkat tentang pentingnya quoting di Bash

### Jawaban :
1.![Uploading Screenshot 2026-04-15 193457.png…]()

2.
![Uploading Screenshot 2026-04-15 193727.png…]()


3. 
<img width="806" height="207" alt="Screenshot 2026-04-15 193857" src="https://github.com/user-attachments/assets/738b2045-baf3-4f6e-bbdc-82c37599be3f" />

