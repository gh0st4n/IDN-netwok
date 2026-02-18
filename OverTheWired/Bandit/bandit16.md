.# Over The Wired

## OverTheWire — Bandit Level 15 → 16

---

### Deskripsi Level

Pada level ini, kita belajar tentang **SSL/TLS encryption**.

Password untuk level berikutnya bisa didapat dengan:

```
Mengirim password Level 15 ke port 30001 di localhost
```

Namun berbeda dari level sebelumnya:

```
Koneksi harus menggunakan SSL/TLS
```

Artinya:

* Tidak bisa pakai `nc` biasa
* Harus melakukan TLS handshake terlebih dahulu
* Setelah channel terenkripsi terbentuk → baru kirim password

Perintah yang relevan:

* `openssl`
* `s_client`
* `ss`
* `nmap`

---

### Tugas

* Terhubung ke service di port 30001
* Menggunakan SSL/TLS
* Mengirim password Level 15
* Mendapatkan password Level 16

---

### Proses

1. Masuk Server sebagai bandit15:

```bash
$ ssh bandit15@bandit.labs.overthewire.org -p 2220
```

Masukkan password hasil level sebelumnya.

---

2. Coba koneksi dengan `nc` (untuk memahami perbedaannya)

```bash
$ nc localhost 30001
```

Tidak akan menghasilkan output yang benar.

Kenapa?

Karena service ini **bukan plaintext TCP**, melainkan TLS encrypted.

---

3. Gunakan `openssl s_client`

```bash
$ openssl s_client -connect localhost:30001
```

Output akan menampilkan:

```
CONNECTED(00000003)
```

Diikuti informasi sertifikat dan detail handshake.

Setelah handshake selesai, terminal akan menunggu input.

---

4. Kirim password Level 15

Paste password Level 15 lalu tekan Enter.

```
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```

---

5. Output:

```
Correct!
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
```

Itulah password untuk login ke Level 16.

---

### Alternatif Singkat

Gunakan mode quiet agar output lebih bersih:

```bash
$ echo "<password_level_15>" | openssl s_client -connect localhost:30001 -quiet
```

Flag `-quiet` menghilangkan informasi sertifikat dan debug TLS.

---

### Jika Muncul:

```
DONE
RENEGOTIATING
KEYUPDATE
```

Itu bukan error.

Itu bagian dari mekanisme TLS:

* DONE → handshake selesai
* RENEGOTIATING → refresh session
* KEYUPDATE → update kunci enkripsi

Abaikan saja, fokus pada output setelah password dikirim.

---

### Kenapa `nc` Tidak Bisa?

Karena:

* `nc` → kirim plaintext
* Service → mengharapkan TLS ClientHello

Protokol tidak cocok.

TLS berada di atas TCP.

Flow yang benar:

```
TCP connect
→ TLS handshake
→ Encrypted channel
→ Kirim password
→ Terima response
```

---

### Yang Gw Pelajari

* Perbedaan TCP biasa dan TLS
* Cara menggunakan `openssl s_client`
* Konsep handshake SSL/TLS
* Kenapa protokol harus sesuai dengan service
* Dasar komunikasi terenkripsi di jaringan

---

Level ini memperkenalkan konsep komunikasi aman (secure channel),
yang menjadi dasar HTTPS dan banyak service modern lainnya.
