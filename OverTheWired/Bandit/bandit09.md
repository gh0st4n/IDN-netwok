# Over The Wired

## Bandit Level 8 → 9

### Deskripsi Level

Pada level ini, kita belajar tentang **deduplikasi dan frekuensi baris teks**.

Password untuk level berikutnya berada di file:

```
data.txt
```

Password tersebut adalah **satu-satunya baris yang muncul hanya sekali**.
Artinya:

* Banyak baris muncul berulang kali
* Hanya satu baris yang unik (tidak duplikat)

Perintah yang digunakan:

* grep
* sort
* uniq
* strings
* base64
* tr
* tar
* gzip
* bzip2
* xxd

Yang paling relevan:

* `sort`
* `uniq`

---

### Tugas

* Menemukan baris di `data.txt` yang hanya muncul satu kali
* Mengambil baris tersebut sebagai password Level 9

---

### Proses

1. Masuk Server :

```bash
$ ssh bandit8@bandit.labs.overthewire.org -p 2220
```

---

2. Lihat file

```bash
$ ls
```

Akan ada:

```
data.txt
```

---

3. Penting: `uniq` hanya bekerja dengan benar jika input sudah di-`sort`.

Jangan langsung:

```bash
$ uniq data.txt
```

Itu salah.

---

4. Gunakan kombinasi berikut:

```bash
$ sort data.txt | uniq -u
```

Penjelasan:

* `sort` → mengurutkan semua baris
* `uniq -u` → menampilkan hanya baris yang muncul sekali

---

5. Output:

```
**4CKMh1JI91bUIZZPXDqGanal4xvAg0JM**
```

Itulah password untuk Level 9.

---

### Alternatif (Lebih Insight)

Untuk melihat jumlah kemunculan tiap baris:

```bash
$ sort data.txt | uniq -c
```

Output contoh:

```
     10 abcdef
      5 qwerty
      1 zyxwvu
```

Baris dengan angka `1` adalah password.

---

### Konsep Penting: Piping

Simbol `|` disebut pipe.
Artinya: output kiri → jadi input kanan.

Contoh:

```
sort → uniq
```

Tanpa pipe, kita harus simpan file sementara.
Dengan pipe, proses jadi efisien dan bersih.

---

### Yang Gw Pelajari

* `uniq` butuh input terurut
* `sort` + `uniq` adalah kombinasi klasik untuk analisis teks
* Menggunakan pipe untuk chaining command
* Cara membaca frekuensi data teks di CLI