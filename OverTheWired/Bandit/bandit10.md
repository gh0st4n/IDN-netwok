# Over The Wired

## Bandit Level 9 → 10

### Deskripsi Level

Pada level ini, kita belajar mengekstrak **string yang bisa dibaca manusia dari file binary**.

Password untuk level berikutnya tersimpan di:

```
data.txt
```

Password tersebut:

* Berada di antara sedikit string yang human-readable
* Didahului oleh beberapa karakter `=`

Artinya:

* File ini kemungkinan besar berisi data binary
* Tidak bisa langsung `cat`
* Harus mengekstrak string teks dari dalamnya

Perintah yang relevan:

* `strings`
* `grep`

---

### Tugas

* Mengambil string yang didahului beberapa `=`
* Menemukan password Level 10

---

### Proses

1. Masuk Server :

```bash
$ ssh bandit9@bandit.labs.overthewire.org -p 2220
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

3. Jika langsung dijalankan:

```bash
$ cat data.txt
```

Output akan kacau (binary).

---

4. Gunakan `strings` untuk mengambil teks yang bisa dibaca

```bash
$ strings data.txt
```

`strings` mengekstrak karakter printable dari file binary.

---

5. Karena password didahului beberapa `=`, gunakan `grep`

```bash
$ strings data.txt | grep ==
```

Atau lebih spesifik:

```bash
$ strings data.txt | grep "===="
```

---

6. Output akan terlihat seperti:

```
========== FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
```

Password berada setelah deretan `=`.

---

### Alternatif Lebih Presisi

Jika ingin hanya ambil password saja:

```bash
$ strings data.txt | grep "=" | tail -n 1
```

Atau:

```bash
$ strings data.txt | grep "=" | awk '{print $NF}'
```

---

### Yang Gw Pelajari

* File binary tidak bisa dibaca langsung dengan `cat`
* `strings` mengekstrak teks printable dari binary
* Kombinasi `strings` + `grep` sangat powerful
* Pattern filtering mempercepat pencarian