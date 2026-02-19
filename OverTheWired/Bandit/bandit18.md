.# Over The Wired

## OverTheWire — Bandit Level 17 → 18

---

## 🧠 Deskripsi Level

Instruksi:

```
Ada 2 file di home directory:
- passwords.old
- passwords.new

Password level berikutnya ada di passwords.new
dan hanya 1 baris yang berbeda antara kedua file tersebut.
```

Artinya:

* Kita tidak perlu brute force.
* Tidak perlu decrypt.
* Cukup cari perbedaannya.

Tool yang dipakai:

```
diff
```

---

## 🔐 Login

```bash
ssh bandit17@bandit.labs.overthewire.org -p 2220
```

Password sebelumnya:

```
xLYVMN9WE5zQ5vHacb0sZEVqbrp7nBTn
```

---

## 1️⃣ Lihat Isi Directory

```bash
ls
```

Output:

```
passwords.old
passwords.new
```

---

## 2️⃣ Bandingkan Kedua File

```bash
diff passwords.old passwords.new
```

Output:

```
< kfBf3Yk5PBRwujdtb8B7S5cYsd
---
> 8SB5aBjkxIrZkJQkD7aQ9nNwQpr
```

Penjelasan:

* `<` = baris dari file pertama (old)
* `>` = baris dari file kedua (new)

Karena password level berikutnya ada di `passwords.new`,
maka yang kita ambil adalah:

```
8SB5aBjkxIrZkJQkD7aQ9nNwQpr
```

---

# ✅ Credential Level Berikutnya

```
Username: bandit18
Password: 8SB5aBjkxIrZkJQkD7aQ9nNwQpr
```

---

## 🧠 Konsep yang Dipelajari

### 1️⃣ diff

Tool sederhana untuk membandingkan file.

Syntax dasar:

```bash
diff file1 file2
```

Simbol:

```
<  → dari file pertama
>  → dari file kedua
```

---

### 2️⃣ Analisa Delta

Kita tidak perlu membaca seluruh file.

Cukup identifikasi perubahan.

Di dunia nyata ini digunakan untuk:

* Audit perubahan konfigurasi
* Review source code
* Detect tampering
* Incident response

---

## ⚠ Catatan

Jika setelah login ke bandit18 muncul:

```
ByeBye
```

Itu bukan error level ini.

Itu bagian dari level berikutnya (Level 18 → 19).

Jadi jangan panik.

---

## Ringkasan

* Gunakan diff.
* Ambil baris yang berbeda di file baru.
* Itulah password.