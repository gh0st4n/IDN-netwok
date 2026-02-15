# Over The Wired

## Bandit Level 4 → 5

### Deskripsi Level

Pada level ini, kita belajar membedakan **jenis file** berdasarkan isinya.

Password untuk level berikutnya tersimpan di dalam **satu-satunya file yang human-readable** di directory:

```
inhere
```

Masalahnya:
Directory tersebut berisi banyak file dengan isi acak (binary / non-readable).
Jika langsung `cat` semua file, terminal bisa jadi rusak tampilannya (karakter kontrol).

Itulah kenapa ada tips:

```
reset
```

Untuk mengembalikan terminal ke kondisi normal.

Perintah yang digunakan di level ini:

* ls
* cd
* cat
* file
* du
* find

### Tugas

* Masuk ke directory `inhere`
* Identifikasi file yang bisa dibaca manusia (ASCII text)
* Ambil password dari file tersebut

### Proses

1. Masuk Server :

```bash
$ ssh bandit4@bandit.labs.overthewire.org -p 2220
```

---

2. Lihat isi directory

```bash
$ ls -la
```

Akan terlihat banyak file seperti:

```
-file00
-file01
-file02
-file03
-file04
-file05
-file06
-file07
-file08
-file09
```

---

3. Jangan langsung `cat *`
   Karena sebagian besar adalah binary file.

---

4. Gunakan `file` untuk mengecek tipe semua file

```bash
$ file ./*
```

Output akan menampilkan tipe masing-masing file, contoh:

```
bandit4@bandit:~/inhere$ file ./*
./-file00: data
./-file01: OpenPGP Public Key
./-file02: OpenPGP Public Key
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data
```

Cari yang bertipe:

```
ASCII text
```

---

5. Baca file yang bertipe ASCII text

```bash
$ cat ./-file07
```

---

6. Output:

`The password you are looking for is:`
**4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw**

---

### Jika Terminal Rusak

Kalau sempat `cat` file binary dan terminal jadi kacau:

```bash
$ reset
```

---

### Alternatif Lebih Cepat

Langsung filter dengan `find`:

```bash
$ find . -type f -exec file {} \; | grep ASCII
```

Atau:

```bash
$ file ./* | grep ASCII
```

---

### Yang Gw Pelajari

* Tidak semua file bisa dibaca manusia
* Gunakan `file` untuk identifikasi tipe file
* Jangan sembarang `cat` file binary
* `reset` bisa memperbaiki terminal yang kacau
* Kombinasi `find`, `file`, dan `grep` mempercepat analisis