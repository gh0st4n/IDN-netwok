# Over The Wired

## OverTheWire — Bandit Level 19 → 20

---

## Deskripsi Level

Instruksi:

```
Untuk mendapatkan akses ke level berikutnya,
gunakan setuid binary di home directory.

Password level berikutnya ada di /etc/bandit_pass
setelah kamu menggunakan setuid binary tersebut.
```

Keyword penting:

* **setuid binary**
* Eksekusi sebagai user lain
* Privilege escalation terbatas

---

## Login

```bash
ssh bandit19@bandit.labs.overthewire.org -p 2220
```

Masukkan password level sebelumnya.

---

## 1. Cek Isi Home Directory

```bash
ls -la
```

Output menunjukkan binary:

```
-rwsr-x--- 1 bandit20 bandit19 ... bandit20-do
```

Perhatikan:

```
s
```

Itu berarti **setuid bit aktif**.

Artinya:

> Program ini akan berjalan dengan UID pemilik file (bandit20),
> bukan UID kita (bandit19).

---

## 2. Jalankan Tanpa Argumen

```bash
./bandit20-do
```

Output:

```
Run a command as another user.
Example: ./bandit20-do id
```

Jelas.

Binary ini menjalankan command sebagai user lain.

---

## 3. Verifikasi UID

```bash
./bandit20-do id
```

Output:

```
uid=11020(bandit20) gid=11019(bandit19)
```

Berarti:

* Effective UID = bandit20
* Kita sekarang bisa akses file milik bandit20

---

## 4. Ambil Password Level 20

Password tersimpan di:

```
/etc/bandit_pass/bandit20
```

Normalnya bandit19 tidak bisa baca.

Gunakan setuid binary:

```bash
./bandit20-do cat /etc/bandit_pass/bandit20
```

Output:

```
GbKksEFF4yrVs6il55v6gwY5aVJe5rj0
```

---

## Konsep yang Dipelajari

### 1. Setuid Bit

Permission:

```
-rwsr-x---
```

`s` pada owner → setuid aktif.

Artinya:

```
Program berjalan dengan effective UID pemilik file.
```

---

### 2. Privilege Escalation Terbatas

Kita tidak menjadi root.

Kita hanya menjalankan binary
dengan privilege user tertentu.

Ini contoh klasik:

* Misconfigured setuid
* Wrapper privilege
* Command proxy

---

### 3. Cara Kerja

Flow:

```
User bandit19
→ Execute setuid binary
→ Effective UID berubah ke bandit20
→ Jalankan command
→ Bisa baca file restricted
```

---

## ⚠ Kenapa Ini Penting di Dunia Nyata?

Setuid binary yang salah konfigurasi
bisa menyebabkan:

* Privilege escalation
* Command injection
* Full system compromise

Makanya binary setuid harus:

* Sangat terbatas
* Tidak menerima input sembarangan
* Tidak vulnerable terhadap injection

---

## Ringkasan

* Lihat permission.
* Temukan setuid.
* Jalankan binary.
* Escalate ke user target.
* Baca file password.
* Selesai.

Ini basic privilege escalation via setuid.