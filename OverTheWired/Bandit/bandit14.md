# OverTheWire

## Bandit Level 13 → 14

---

## Deskripsi Level

Password untuk level berikutnya berada di:

```
/etc/bandit_pass/bandit14
```

Tetapi:

* File tersebut hanya bisa dibaca oleh user `bandit14`
* Kita **tidak diberikan password**
* Kita diberikan **private SSH key**
* Key itu digunakan untuk login sebagai `bandit14`

Intinya:
Level ini menguji pemahaman dasar **SSH key authentication**.

---

## Objective

* Temukan private key di home `bandit13`
* Gunakan key tersebut untuk login sebagai `bandit14`
* Ambil password dari `/etc/bandit_pass/bandit14`

---

# Proses Step-by-Step

---

## 1️⃣ Login sebagai bandit13

```bash
ssh bandit13@bandit.labs.overthewire.org -p 2220
```

---

## 2️⃣ Cek Isi Home Directory

```bash
ls -la
```

Biasanya akan terlihat file seperti:

```
sshkey.private
```

Itulah private key yang dimaksud.

---

## 3️⃣ Periksa Permission File

```bash
ls -l sshkey.private
```

Jika permission terlalu terbuka, SSH akan menolak.

Minimal harus:

```
600
```

Jika perlu, ubah:

```bash
chmod 600 sshkey.private
```

---

## 4️⃣ Login Menggunakan Private Key

Gunakan opsi `-i` pada ssh:

```bash
ssh -i sshkey.private bandit14@localhost -p 2220
```

Kenapa `localhost`?

Karena kita sudah berada di server Bandit.
Kita hanya berpindah user via SSH internal.

Jika berhasil, prompt akan berubah menjadi:

```
bandit14@bandit:~$
```

---

## 5️⃣ Ambil Password Level 14

```bash
cat /etc/bandit_pass/bandit14
```

Output:

```
<password_level_14>
```

Itulah password untuk login berikutnya.

---

# Konsep yang Dipelajari

### 🔐 SSH Key Authentication

Login SSH bisa menggunakan:

* Password
* Private/Public Key Pair

Struktur dasar:

```
ssh -i <private_key> user@host -p <port>
```

SSH akan:

1. Menggunakan private key
2. Mencocokkan dengan public key milik user target
3. Jika valid → login tanpa password

---

# Ringkasan Teknis

Yang diuji di level ini:

* Mengidentifikasi private key
* Mengatur permission file (chmod 600)
* Menggunakan `ssh -i`
* Memahami perbedaan login password vs key-based auth

Level ini bukan tentang eksploitasi.
Ini tentang **mekanisme autentikasi SSH yang benar**.
