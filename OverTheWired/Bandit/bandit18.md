.# Over The Wired

## OverTheWire — Bandit Level 18 → 19

---

## 🧠 Deskripsi Level

Instruksi level:

```
Password untuk level berikutnya tersimpan dalam file readme di home directory.
Namun .bashrc telah dimodifikasi agar kamu langsung logout saat login via SSH.
```

Artinya:

* File ada.
* Permission benar.
* Masalahnya bukan akses.
* Masalahnya ada di shell startup.

Ini bukan privilege issue.
Ini bypass shell behavior.

---

## 🔐 Login Awal

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220
```

Masukkan password level sebelumnya.

Hasil:

```
Connection closed.
```

Langsung logout.

Kenapa?

Karena `.bashrc` dimodifikasi untuk:

```bash
exit
```

Saat shell interactive berjalan → langsung keluar.

---

## 🎯 Tujuan

Bypass `.bashrc`.

Intinya:

> Jangan biarkan shell interactive berjalan.

---

## 🔥 Solusi 1 — Jalankan Command Langsung Saat SSH

SSH punya fitur:

```
ssh user@host "command"
```

Command akan dijalankan tanpa membuka shell interactive penuh.

Gunakan:

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"
```

Masukkan password level 18.

Output:

```
IueksS7Ubh8G3DCwVzrdRAvoWg3M5x
```

---

## 🧪 Alternatif Metode

### 1. Disable pseudo-terminal

```bash
ssh -T bandit18@bandit.labs.overthewire.org -p 2220 cat readme
```

`-T` → disable TTY allocation.

---

### 2. Gunakan shell berbeda

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 /bin/sh
```

Kadang bisa bypass jika .bashrc hanya untuk bash.

---

### 3. Gunakan `-t` untuk force command

```bash
ssh -t bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"
```

---

## 📌 Alasan

Karena:

* `.bashrc` dijalankan hanya saat interactive shell.
* SSH remote command **tidak menjalankan interactive login shell biasa**.
* Jadi script logout tidak sempat dieksekusi sebelum command berjalan.

Flow normal:

```
SSH connect
→ start interactive bash
→ .bashrc
→ exit
```

Flow bypass:

```
SSH connect
→ execute remote command
→ output
→ disconnect
```

Tidak ada interactive session penuh.

---

## 🧠 Konsep yang Dipelajari

* Cara kerja SSH remote command execution
* Perbedaan interactive vs non-interactive shell
* Startup file bash (.bashrc, .profile)
* Cara bypass restriction berbasis shell

---

## Ringkasan Brutal

Masalahnya bukan permission.
Masalahnya environment.

Solusinya:

Jangan main sesuai aturan yang dibuat.

Eksekusi command langsung → ambil file → selesai.

---

Level ini mengajarkan mindset penting:

> Jika dibatasi di layer tertentu, pindah layer.
