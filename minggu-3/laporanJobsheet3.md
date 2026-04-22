### Jawaban Latihan 2.G

Langkah pertama adalah mengecek apakah terdapat service yang gagal dengan menggunakan perintah berikut:

```bash
systemctl --failed
```

Output:

```bash
0 load ed units listed.
```

Berdasarkan output di atas, tidak terdapat service yang gagal pada sistem. Oleh karena itu, dipilih salah satu service aktif yaitu **SSH** untuk diperiksa statusnya.

---

#### 1. Melihat Status Service

Perintah yang digunakan:

```bash
systemctl status ssh
```

Output:

```bash
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2026-02-24 22:45:10 UTC; 10min ago
       Docs: man:sshd(8)
             man:sshd_config(5)
   Main PID: 742 (sshd)
      Tasks: 1 (limit: 2230)
     Memory: 5.6M
        CPU: 32ms
     CGroup: /system.slice/ssh.service
             └─742 /usr/sbin/sshd -D

Feb 24 22:45:10 ubuntu systemd[1]: Started OpenBSD Secure Shell server.
Feb 24 22:45:10 ubuntu sshd[742]: Server listening on 0.0.0.0 port 22.
Feb 24 22:45:10 ubuntu sshd[742]: Server listening on :: port 22.
```

---

#### 2. Melihat 30 Baris Log Terakhir

Perintah yang digunakan:

```bash
journalctl -u ssh -n 30
```

Output:

```bash
Feb 24 22:44:58 ubuntu systemd[1]: Starting OpenBSD Secure Shell server...
Feb 24 22:45:10 ubuntu sshd[742]: Server listening on 0.0.0.0 port 22.
Feb 24 22:45:10 ubuntu sshd[742]: Server listening on :: port 22.
Feb 24 22:45:10 ubuntu systemd[1]: Started OpenBSD Secure Shell server.
Feb 24 22:46:01 ubuntu sshd[812]: Accepted password for user from 192.168.1.10 port 52314 ssh2
Feb 24 22:46:01 ubuntu sshd[812]: pam_unix(sshd:session): session opened for user
Feb 24 22:47:12 ubuntu sshd[812]: pam_unix(sshd:session): session closed for user
Feb 24 22:48:20 ubuntu sshd[845]: Connection closed by authenticating user user 192.168.1.10 port 52340
...
```

Berdasarkan hasil di atas, service **ssh** berjalan dengan status **active (running)** dan sistem mencatat aktivitas koneksi yang dilakukan melalui protokol SSH pada port 22.

