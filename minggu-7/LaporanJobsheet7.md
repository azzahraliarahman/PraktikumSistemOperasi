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



