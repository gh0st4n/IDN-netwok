# Over The Wired

## Bandit Level 10 → 11

### Deskripsi Level

Pada level ini, kita belajar tentang **encoding Base64**.

Password untuk level berikutnya tersimpan di file:

```
data.txt
```

Namun isi file tersebut bukan plaintext biasa.
Isinya adalah **data yang di-encode menggunakan Base64**.

Artinya:

* Harus di-decode terlebih dahulu
* Setelah decode → akan muncul password asli

Perintah yang relevan:

* `base64`

---

### Tugas

* Decode isi file `data.txt`
* Mendapatkan password Level 11

---

### Proses

1. Masuk Server :

```bash
$ ssh bandit10@bandit.labs.overthewire.org -p 2220
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

3. Coba lihat isi file

```bash
$ cat data.txt
```

Output akan terlihat seperti string panjang berisi karakter:

```
VGhlIHBhc3N3b3JkIGlzIGR0UjE3M2ZaS2IwUlJzREZTR3NnMlJXbnBOVmozcVJyCg==
```

Itu ciri khas Base64.

---

4. Decode menggunakan `base64`

```bash
$ base64 -d data.txt
```

atau

```bash
$ cat data.txt | base64 -d
```

---

5. Output:
**The password is dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr**

Itulah password untuk login ke Level 11.

---

### Kenapa Perlu Decode?

Base64 bukan enkripsi.
Itu hanya encoding untuk:

* Mengubah binary → teks ASCII
* Aman dikirim lewat protokol teks

---

### Ciri-ciri Base64

* Karakter: A–Z, a–z, 0–9, +, /
* Biasanya diakhiri dengan `=` atau `==`
* Panjang kelipatan 4

---

### Yang Gw Pelajari

* Perbedaan encoding vs encryption
* Cara decode Base64 di Linux
* Mengenali pola Base64 secara visual
* Menggunakan pipe untuk chaining command