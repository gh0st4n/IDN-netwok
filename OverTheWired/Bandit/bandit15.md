# OverTheWire

## OverTheWire — Bandit Level 14 → 15

---

## Deskripsi Level

Password level berikutnya bisa didapat dengan:

> Mengirimkan password Level 14 ke **port 30000 di localhost**

Artinya:

* Ada service TCP berjalan di port 30000
* Service tersebut menunggu input
* Jika kita kirim password yang benar → dia balas password Level 15

Ini bukan soal file lagi.
Ini soal komunikasi jaringan.

---

## Konsep Teknis

Beberapa konsep penting:

### 1. Localhost

```
localhost = 127.0.0.1
```

Artinya komunikasi internal dalam mesin yang sama.

---

### 2. Port

Port = endpoint service.

Contoh:

* 22 → SSH
* 80 → HTTP
* 443 → HTTPS
* 30000 → custom service (level ini)

---

### 3. TCP Client

Kita butuh tool untuk:

* Connect ke port
* Kirim string
* Terima response

Tool yang bisa dipakai:

* `nc` (netcat) ← paling simpel
* `telnet`
* `openssl s_client` (kalau TLS, tapi di level ini tidak TLS)

---

# Tugas

* Login sebagai `bandit14`
* Kirim password Level 14 ke port 30000
* Ambil response (password Level 15)

---

# Proses

---

## 1. Login sebagai bandit14

Dari mesin lokal:

```bash
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```

---

## 2. Simpan password Level 14

Password sebelumnya:

```
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
```

---

## 3. Kirim password ke port 30000

Gunakan `nc`:

```bash
nc localhost 30000
```

Terminal akan terlihat seperti hang (menunggu input).

Sekarang paste password:

```
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
```

Tekan Enter.

---

## 4. Output

Service akan membalas:

```
Correct!
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```

Itulah password untuk login ke Level 15.

---

# Alternatif One-Liner

Lebih efisien:

```bash
echo "MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS" | nc localhost 30000
```

Tanpa interactive mode.

---

# Kenapa Ini Berhasil?

Karena service di port 30000 bekerja seperti ini:

```
read(input)
if input == password14:
    print(password15)
```

Tidak ada enkripsi.
Tidak ada hashing.
Plaintext TCP service.

---

# Apa yang Sebenarnya Diuji?

Level ini menguji:

* Pemahaman port
* Pemahaman TCP
* Penggunaan netcat sebagai raw client
* Konsep service listening

Bukan hacking.
Bukan cracking.
Hanya komunikasi jaringan dasar.

---

# Analisis Tambahan

Kalau mau investigasi lebih lanjut:

Cek apakah port terbuka:

```bash
nmap localhost -p 30000
```

Atau lihat proses listening:

```bash
ss -ltnp
```

Tapi untuk solve level ini, tidak perlu.

---

# Yang Dipelajari

* Cara menggunakan `nc`
* Cara berinteraksi dengan TCP service
* Perbedaan service berbasis file vs service berbasis jaringan
* Konsep client-server sederhana

---

Level ini adalah transisi dari:

Filesystem-based challenge → Network-based challenge

Level berikutnya (15 → 16) mulai masuk TLS.

Kalau mau lanjut, kita bedah juga.
