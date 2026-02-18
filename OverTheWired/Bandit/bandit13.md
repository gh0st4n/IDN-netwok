# OverTheWire

## Bandit Level 12 → 13

---

## Deskripsi Level

Pada level ini kita diberikan file `data.txt` yang berisi **hexdump** dari sebuah file yang telah **dikompres berulang kali**.

Artinya:

* `data.txt` bukan file asli
* Isinya representasi hex
* Harus dikembalikan ke bentuk binary
* Lalu diekstrak berlapis sampai menjadi plaintext

Best practice: kerja di `/tmp` agar tidak mengotori home directory.

Tools yang dipakai:

* `xxd`
* `file`
* `gzip`
* `bzip2`
* `tar`
* `mktemp`
* `cp`
* `mv`

---

## Objective

* Buat directory kerja di `/tmp`
* Convert hexdump → binary
* Identifikasi tipe file
* Extract sesuai tipe
* Ulangi sampai mendapatkan file ASCII (plaintext)
* Ambil password Level 13

---

# Proses Step-by-Step

---

## 1 Login ke Server

```bash
ssh bandit12@bandit.labs.overthewire.org -p 2220
```

---

## 2 Buat Directory Kerja Sementara

```bash
mktemp -d
cd /tmp/tmp.xxxxxx
```

---

## 3 Copy File Target

```bash
cp ~/data.txt .
```

---

## 4 Reverse Hexdump → Binary

```bash
xxd -r data.txt > data.bin
```

---

## 5 Identifikasi Tipe File

```bash
file data.bin
```

Output awal biasanya:

```
gzip compressed data
```

---

# 🔁 Loop Extraction Strategy

Workflow selalu sama:

```
file → rename sesuai tipe → extract → ulangi
```

---

## Layer 1 — GZIP

```bash
mv data.bin data.gz
gunzip data.gz
```

Cek lagi:

```bash
file data
```

Output:

```
bzip2 compressed data
```

---

## Layer 2 — BZIP2

```bash
mv data data.bz2
bunzip2 data.bz2
```

Cek lagi:

```bash
file data
```

Output:

```
gzip compressed data
```

---

## Layer 3 — GZIP

```bash
mv data data.gz
gunzip data.gz
```

Cek lagi:

```bash
file data
```

Output:

```
POSIX tar archive
```

---

## Layer 4 — TAR

```bash
mv data data.tar
tar -xf data.tar
```

Output:

```
data5.bin
```

---

## Layer 5 — TAR Lagi

```bash
mv data5.bin data5.tar
tar -xf data5.tar
```

Output:

```
data6.bin
```

---

## Layer 6 — BZIP2

```bash
mv data6.bin data6.bz2
bunzip2 data6.bz2
```

Cek:

```bash
file data6
```

Output:

```
POSIX tar archive
```

---

## Layer 7 — TAR

```bash
mv data6 data6.tar
tar -xf data6.tar
```

Output:

```
data8.bin
```

---

## Layer 8 — GZIP Terakhir

```bash
mv data8.bin data8.gz
gunzip data8.gz
```

Cek:

```bash
file data8
```

Output:

```
ASCII text
```

---

## 🎯 Final Step

Baca isi file terakhir:

```bash
cat data8
```

Output:

```
The password is FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
```

---

# Kesimpulan Teknis

Yang dipelajari di level ini:

* `xxd -r` untuk reverse hexdump
* `file` sebagai tool analisis tipe file
* Ekstraksi bertingkat (`gzip`, `bzip2`, `tar`)
* Pentingnya workflow sistematis
* Praktik kerja aman di `/tmp`

Level ini bukan soal hafalan command.
Ini soal **disiplin analisis dan pola kerja metodis**.
