.# Over The Wired

## OverTheWire — Bandit Level 20 → 21

---

## 🧠 Deskripsi Level

Instruksi:

```
Ada setuid binary di home directory.
Binary tersebut akan:
- Connect ke localhost pada port yang kita tentukan
- Membaca 1 baris dari koneksi tersebut
- Membandingkan dengan password bandit20
- Jika cocok → mengirim password bandit21
```

Artinya:

* Kita harus membuat server listener sendiri.
* Binary akan menjadi client.
* Kita kirim password level sebelumnya.
* Jika benar → dia kirim password level berikutnya.

Konsep:

* setuid
* local socket communication
* netcat listener
* authentication via network

---

## 🔐 Login

```bash
ssh bandit20@bandit.labs.overthewire.org -p 2220
```

Password level sebelumnya:

```
GbKksEFF4yrVs6il55v6gwY5aVJe5rj0
```

---

## 1️⃣ Lihat Isi Home

```bash
ls -la
```

Ada binary:

```
bandit20-do
```

Jalankan tanpa argumen:

```bash
./bandit20-do
```

Output:

```
Run a command as another user.
```

Binary yang kita butuhkan adalah:

```
suconnect
```

Cek lagi isi directory — ada binary lain:

```
suconnect
```

Jalankan tanpa argumen:

```bash
./suconnect
```

Output:

```
Usage: ./suconnect <port>
```

Jelas.

---

## 🎯 Strategi

1. Buka listener dengan `nc`
2. Jalankan `suconnect` agar connect ke listener
3. Kirim password bandit20
4. Terima password bandit21

Butuh 2 terminal.

---

## 2️⃣ Terminal 1 — Listener

Pilih port bebas, misalnya 1234:

```bash
nc -lvp 1234
```

Listener siap menerima koneksi.

---

## 3️⃣ Terminal 2 — Jalankan suconnect

```bash
./suconnect 1234
```

Binary akan connect ke localhost:1234.

---

## 4️⃣ Kirim Password Bandit20

Di terminal listener (Terminal 1), setelah koneksi masuk,
kirim:

```
GbKksEFF4yrVs6il55v6gwY5aVJe5rj0
```

Tekan Enter.

---

## 5️⃣ Response

Jika password benar, akan muncul:

```
Correct!
<password_level_21>
```

Output yang didapat:

```
gE269g2h3mw3pwgrj0HaUoqen1c9DGr
```

---

## 🔎 Alternatif 1-Line

Bisa juga pakai:

```bash
echo "GbKksEFF4yrVs6il55v6gwY5aVJe5rj0" | nc -lvp 1234 &
./suconnect 1234
```

---

## 🧠 Konsep yang Dipelajari

### 1️⃣ Setuid + Network Interaction

Binary berjalan sebagai bandit21.
Dia membaca input dari socket.

---

### 2️⃣ Localhost Networking

Client → localhost
Server → kita buat sendiri

---

### 3️⃣ Flow Proses

```
Kita buat listener
→ suconnect connect ke listener
→ Kita kirim password bandit20
→ suconnect validasi
→ Jika cocok → kirim password bandit21
```

---

### 4️⃣ Kenapa Ini Aman?

Karena:

* Password hanya dicek secara lokal
* Tidak exposed ke jaringan luar
* Service tidak open ke publik

---

## Ringkasan Brutal

* Buat listener.
* Jalankan suconnect.
* Kirim password lama.
* Terima password baru.
* Selesai.

Ini latihan memahami interaksi network lokal + privilege boundary via setuid.