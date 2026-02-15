# Over The Wired

## Bandit Level 11 → 12

### Deskripsi Level

Pada level ini, kita belajar tentang **ROT13 cipher**.

Password untuk level berikutnya tersimpan di file:

```
data.txt
```

Namun isi file tersebut telah mengalami:

```
Rotasi 13 posisi pada huruf a–z dan A–Z
```

Artinya:

* a ↔ n
* b ↔ o
* c ↔ p
* dst…
* Berlaku untuk huruf kecil & besar
* Angka dan simbol tidak berubah

Ini disebut **ROT13** (Caesar cipher dengan shift 13).

Perintah yang relevan:

* `tr`

---

### Tugas

* Mengembalikan teks ROT13 menjadi teks asli
* Mendapatkan password Level 12

---

### Proses

1. Masuk Server :

```bash
$ ssh bandit11@bandit.labs.overthewire.org -p 2220
```

---

2. Lihat file

```bash
$ ls
```

Akan terlihat:

```
data.txt
```

---

3. Lihat isi file

```bash
$ cat data.txt
```

Isinya akan terlihat seperti teks aneh tapi masih readable (karena hanya huruf yang digeser).
```bash
Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4
```

---

4. Gunakan `tr` untuk melakukan rotasi balik

```bash
$ cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

Penjelasan:

* `A-Za-z` → semua huruf
* `N-ZA-Mn-za-m` → rotasi 13 posisi
* ROT13 simetris → perintah yang sama bisa encode/decode

---

5. Output:
**The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4**

Itulah password untuk login ke Level 12.

---

### Alternatif Singkat

```bash
$ tr 'A-Za-z' 'N-ZA-Mn-za-m' < data.txt
```

---

### Kenapa ROT13 Bisa Dibalik dengan Perintah Sama?

Karena:

* 13 adalah setengah dari 26 huruf alfabet
* Jika digeser 13 dua kali → kembali ke huruf awal

Contoh:

```
a → n
n → a
```

---

### Yang Gw Pelajari

* Konsep Caesar cipher
* Cara menggunakan `tr` untuk translasi karakter
* ROT13 bersifat simetris
* Manipulasi teks sederhana di CLI