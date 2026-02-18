# OverTheWire

## Bandit Level 13 → 14

---

### Deskripsi Level

Pada level ini, kita **tidak menggunakan password lagi**.

Password level berikutnya **tidak bisa dibaca langsung oleh user bandit13**.

Sebagai gantinya, kita diberikan:

```
sshkey.private
```

File tersebut adalah **private SSH key** yang harus digunakan untuk login sebagai `bandit14`.

---

### Konsep Inti

Selama ini login pakai:

```
username + password
```

Di level ini kita pakai:

```
username + private key
```

Ini adalah metode autentikasi profesional yang lebih aman.

---

## Tugas

* Gunakan private key yang tersedia
* Login sebagai `bandit14`
* Ambil password level berikutnya

---

## Proses

### 1. Login ke Level 13

```bash
$ ssh bandit13@bandit.labs.overthewire.org -p 2220
```

Masukkan password level 13.

---

### 2. Cek File

```bash
$ ls
```

Output:

```
sshkey.private
```

---

### 3. Atur Permission Key

SSH menolak private key jika permission terlalu terbuka.

```bash
$ chmod 600 sshkey.private
```

Penjelasan:

* `600` → hanya owner yang bisa read & write
* SSH mewajibkan key tidak readable oleh user lain

---

### 4. Login Menggunakan Private Key

```bash
$ ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```

Penjelasan:

* `-i` → identity file (private key)
* Tidak perlu password

Jika berhasil → kamu masuk sebagai `bandit14`.

---

### 5. Ambil Password Level Berikutnya

```bash
$ cat /etc/bandit_pass/bandit14
```

Output akan menampilkan password untuk level 14.

---

### 6. Keluar

```bash
$ exit
```

---

## Kenapa Harus chmod 600?

Karena SSH punya strict permission check.

Jika file:

* `644`
* `777`

Maka SSH akan error:

```
UNPROTECTED PRIVATE KEY FILE!
```

Artinya:
Private key tidak boleh bisa dibaca orang lain.

---

## Yang Dipelajari

* Autentikasi SSH berbasis private key
* Opsi `-i` pada ssh
* Permission file & keamanan
* Konsep dasar asymmetric cryptography dalam praktik
* Password tidak selalu metode terbaik

---

## Inti Level Ini

Password tidak selalu dipakai langsung.
Kadang kita harus menggunakan **credential lain** untuk mengakses sistem.

Ini mindset penting di dunia real-world security.