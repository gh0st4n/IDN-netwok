.# Over The Wired

## OverTheWire — Bandit Level 22 → 23

---

## 🧠 Deskripsi Level

Instruksi:

```
Sebuah program berjalan otomatis melalui cron.
Lihat konfigurasi di /etc/cron.d/ untuk mengetahui apa yang dijalankan.
```

Hint tambahan:

> Script shell lain yang ditulis user biasanya sangat membantu.
> Jika kamu tidak paham, jalankan manual untuk melihat debug output.

Artinya:

* Enumerasi cron lagi.
* Analisa script.
* Replikasi logikanya.
* Ambil password.

---

## 🔐 Login

```bash
ssh bandit22@bandit.labs.overthewire.org -p 2220
```

Password sebelumnya:

```
Yk7owGAcWjwVMrwTseJ8w7W0iLLL
```

---

## 1️⃣ Cek Cron

```bash
cd /etc/cron.d
ls
```

Ada file:

```
cronjob_bandit23
```

Buka:

```bash
cat cronjob_bandit23
```

Isi:

```
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh &> /dev/null
```

Artinya:

* Jalan setiap menit
* Sebagai user bandit23
* Menjalankan script tertentu

---

## 2️⃣ Analisa Script

```bash
cat /usr/bin/cronjob_bandit23.sh
```

Isi penting:

```bash
myname=$(whoami)
mytarget=$(echo "I am user $myname" | md5sum | cut -d ' ' -f 1)
cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

---

## 🔎 Breakdown Logika

### 1️⃣ Simpan username

```bash
myname=$(whoami)
```

Karena dijalankan oleh cron sebagai:

```
bandit23
```

Maka:

```
myname = bandit23
```

---

### 2️⃣ Generate Hash

```bash
echo "I am user bandit23" | md5sum
```

Hasil:

```
8ca319486bfbbc3663ea0fbe81326349
```

(ambil bagian hash saja dengan `cut`)

---

### 3️⃣ Simpan Password

Script melakukan:

```bash
cat /etc/bandit_pass/bandit23 > /tmp/8ca319486bfbbc3663ea0fbe81326349
```

Artinya:

Password bandit23 akan tersimpan di:

```
/tmp/8ca319486bfbbc3663ea0fbe81326349
```

---

## 3️⃣ Replikasi Manual

Sebagai bandit22 kita bisa hitung sendiri:

```bash
echo "I am user bandit23" | md5sum | cut -d ' ' -f1
```

Hasil:

```
8ca319486bfbbc3663ea0fbe81326349
```

---

## 4️⃣ Ambil Password

Tunggu maksimal 1 menit (cron interval).

Lalu:

```bash
cat /tmp/8ca319486bfbbc3663ea0fbe81326349
```

Output:

```
jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n
```

## 🧠 Konsep yang Dipelajari

### 1️⃣ Analisa Script Manual

Jika tidak paham, jalankan bagian per bagian:

```bash
whoami
echo "I am user bandit23" | md5sum
```

---

### 2️⃣ Hash Deterministic

Input tetap → hash tetap.

Script tidak menggunakan random value.

Artinya:

File path bisa diprediksi.

---

### 3️⃣ Data Leak via /tmp

Sensitive data disimpan di:

```
/tmp/<predictable_hash>
```

Jika nama file bisa dihitung → bisa diambil.

---

### 4️⃣ Flow Serangan

```
Cron run as bandit23
→ Generate md5 dari string tetap
→ Copy password ke /tmp/hash
→ Kita hitung hash yang sama
→ Baca file
→ Dapat password
```

---

## ⚠ Vulnerability Pattern

Ini contoh klasik:

* Predictable filename
* Insecure temporary storage
* No permission hardening
* No cleanup

Di dunia nyata ini bisa disebut:

> Predictable temporary file disclosure

---

## Ringkasan Brutal

* Enumerasi cron.
* Baca script.
* Pahami logika.
* Hitung hash.
* Ambil file.
* Selesai.

Ini latihan analisa script + prediksi output deterministik.
.