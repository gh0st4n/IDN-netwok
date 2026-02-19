.# Over The Wired

## OverTheWire — Bandit Level 21 → 22

---

## 🧠 Deskripsi Level

Instruksi:

```
Sebuah program dijalankan otomatis secara berkala oleh cron.
Lihat konfigurasi di /etc/cron.d/ untuk mengetahui command yang dieksekusi.
```

Keyword:

* cron
* scheduled task
* privilege separation
* script automation

Artinya:

Kita tidak menjalankan programnya.
Program dijalankan otomatis oleh sistem.

Tugas kita:

* Cari script yang dijalankan
* Analisa apa yang dilakukan
* Ambil password

---

## 🔐 Login

```bash
ssh bandit21@bandit.labs.overthewire.org -p 2220
```

Password level sebelumnya:

```
gE269g2h3mw3pwgrj0HaUoqen1c9DGr
```

---

## 1️⃣ Cek Konfigurasi Cron

Masuk ke directory cron:

```bash
cd /etc/cron.d
ls -la
```

Ada file:

```
cronjob_bandit22
```

Buka:

```bash
cat cronjob_bandit22
```

Isi:

```
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```

Analisis:

```
* * * * *
```

Artinya:

* Jalan setiap menit
* Sebagai user bandit22
* Menjalankan script `/usr/bin/cronjob_bandit22.sh`

---

## 2️⃣ Analisa Script

Buka script:

```bash
cat /usr/bin/cronjob_bandit22.sh
```

Isi kurang lebih:

```bash
#!/bin/bash
cat /etc/bandit_pass/bandit22 > /tmp/<random_string>
```

Artinya:

* Script membaca password bandit22
* Menyimpannya ke file di /tmp
* File name statis (hardcoded)

Misalnya:

```
/tmp/t0odsS90RqQh9aMc25hpAoZKf7F9
```

---

## 3️⃣ Ambil Password

Tunggu maksimal 1 menit (karena cron tiap menit).

Lalu:

```bash
cat /tmp/t0odsS90RqQh9aMc25hpAoZKf7F9
```

Output:

```
Yk7owGAcWjwVMrwTseJ8w7W0iLLL
```

---

## 🧠 Konsep yang Dipelajari

### 1️⃣ Cron Job Enumeration

Lokasi penting:

```
/etc/cron.d/
/etc/crontab
/var/spool/cron
```

---

### 2️⃣ Privilege Context

Script dijalankan sebagai:

```
bandit22
```

Artinya dia bisa membaca:

```
/etc/bandit_pass/bandit22
```

Tapi kita (bandit21) tidak bisa.

---

### 3️⃣ Data Leak via World-Readable Temp File

Script menyimpan output ke:

```
/tmp/
```

Directory ini:

* World readable
* World writable

Jika file permission default → bisa dibaca user lain.

---

### 4️⃣ Flow Eksploitasi

```
Cron jalan tiap menit
→ Script baca password bandit22
→ Simpan ke /tmp
→ Kita baca file itu
→ Ambil password
```

---

## ⚠ Kenapa Ini Vulnerable?

Karena:

* Sensitive data disimpan di /tmp
* Tanpa permission hardening
* Tanpa randomisasi per-execution
* Tanpa cleanup

Di dunia nyata ini disebut:

> Insecure temporary file handling

---

## Ringkasan Brutal

* Enumerasi cron.
* Temukan script.
* Baca script.
* Identifikasi output file.
* Tunggu cron run.
* Ambil password.

Ini latihan reconnaissance + privilege boundary via automation leak.
