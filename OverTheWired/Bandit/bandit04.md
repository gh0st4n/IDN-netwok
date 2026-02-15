# Over The Wired

## Bandit Level 3 → 4

### Deskripsi Level

Pada level ini, kita belajar tentang **hidden file (file tersembunyi)** di Linux.

Password untuk level berikutnya tersimpan dalam **file tersembunyi** yang berada di dalam directory:

```
inhere
```

Di Linux, file yang diawali dengan tanda titik (`.`) disebut **hidden file** dan tidak akan terlihat dengan `ls` biasa.

Perintah yang digunakan di level ini:

* ls
* cd
* cat
* file
* du
* find

---

### Tugas

* Masuk ke directory `inhere`
* Menemukan file tersembunyi
* Membaca isinya untuk mendapatkan password Level 4

---

### Proses

1. Masuk Server :

```bash
$ ssh bandit3@bandit.labs.overthewire.org -p 2220
```

---

2. Lihat isi directory

```bash
$ ls
```

Akan terlihat:

```
inhere
```

---

3. Masuk ke directory tersebut

```bash
$ cd inhere
```

---

4. Coba lihat isi directory

```bash
$ ls
```

Kemungkinan tidak muncul apa-apa.
Karena filenya tersembunyi.

---

5. Gunakan `-a` untuk menampilkan hidden file

```bash
$ ls -la
```

Output akan terlihat file seperti:

```
...Hiding-From-You
```

---

6. Baca isi file tersebut

```bash
$ cat ...Hiding-From-You
```

---

7. Output:

`The password you are looking for is:`
**2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ**

---

### Yang Gw Pelajari

* File yang diawali `.` adalah hidden file
* `ls` tidak menampilkan hidden file secara default
* Gunakan `ls -a` atau `ls -la` untuk melihat semuanya
* Struktur directory harus dicek sebelum asumsi kosong