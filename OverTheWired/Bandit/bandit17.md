# Over The Wired

## OverTheWire — Bandit Level 16 → 17

---

### Deskripsi Level

Level ini fokus ke:

* Port scanning
* Identifikasi service SSL
* TLS handshake
* Ekstraksi private key
* SSH key authentication
* File comparison (`diff`)

Instruksi inti:

```
Submit password level saat ini ke salah satu port di localhost (range 31000–32000)
Beberapa port menggunakan SSL
```

---

## Login Awal

```bash
ssh bandit16@bandit.labs.overthewire.org -p 2220
```

Masukkan password Level 16.

---

## 1. Enumerasi Port

Scan range 31000–32000:

```bash
nmap -p 31000-32000 -sV -A -T4 localhost
```

Hasil penting:

```
31046/tcp open  echo
31518/tcp open  ssl/echo
31691/tcp open  echo
31790/tcp open  ssl/unknown
31960/tcp open  echo
```

Analisis:

* Banyak echo → bukan target
* 31518 = ssl + echo
* **31790 = ssl + unknown service → kandidat**

Target: **31790**

---

## 2. TLS Connection

Gunakan openssl:

```bash
openssl s_client -connect localhost:31790
```

Setelah handshake selesai → paste password Level 16.

Output:

```
-----BEGIN RSA PRIVATE KEY-----
...
-----END RSA PRIVATE KEY-----
```

Server mengembalikan **RSA private key**.

---

## 3. Simpan Private Key

```bash
mktemp -p /tmp bandit17.key
```

Paste key ke file tersebut.

Set permission:

```bash
chmod 600 /tmp/bandit17.key
```

Tanpa permission 600 → SSH akan reject.

---

## 4. Login sebagai bandit17

```bash
ssh -i /tmp/bandit17.key bandit17@localhost -p 2220
```

Login berhasil.

---

## 5. Ambil Password Level 17

Di home directory terdapat:

```
passwords.old
passwords.new
```

Bandingkan:

```bash
diff passwords.old passwords.new
```

Output menunjukkan perbedaan:

```
< (old password)
---
> xLYVMN9WE5zQ5vHacb0sZEVqbrp7nBTn
```

## Konsep yang Dipelajari

### 1. Recon Dulu, Jangan Asumsi

Scan → identifikasi → validasi.

### 2. SSL ≠ Plain TCP

`nc` gagal karena service mengharapkan TLS ClientHello.

### 3. TLS Flow

```
TCP connect
→ TLS handshake
→ Encrypted channel
→ Kirim password
→ Terima private key
```

### 4. SSH Key Auth

```
ssh -i key user@host
chmod 600 key
```

### 5. diff untuk Analisis Perubahan

Tool simpel, powerful untuk cari delta.

