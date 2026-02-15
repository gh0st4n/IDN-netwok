# Over The Wired

## Bandit Level 5 → 6

### Deskripsi Level

Pada level ini, kita belajar melakukan **filter file berdasarkan properti tertentu**.

Password untuk level berikutnya berada di suatu file di dalam directory:

```
inhere
```

File tersebut memiliki kriteria:

* Human-readable
* Berukuran **1033 bytes**
* Tidak executable

Artinya kita tidak bisa asal `cat`.
Harus melakukan pencarian terfilter.

Perintah yang digunakan di level ini:

* ls
* cd
* cat
* file
* du
* find

---

### Tugas

* Mencari file di dalam `inhere` yang:
  * Size = 1033 bytes
  * Tidak executable
  * Bisa dibaca manusia
* Membaca isinya untuk mendapatkan password Level 6

---

### Proses

1. Masuk Server :

```bash
$ ssh bandit5@bandit.labs.overthewire.org -p 2220
```

---

2. Cek struktur directory

```bash
$ ls -la
```

Biasanya terdapat banyak subdirectory.

---

3. Gunakan `find` untuk mencari file dengan ukuran 1033 bytes

```bash
$ find . -type f -size 1033c
```

```bash
$ ./maybehere07/.file2
```

Penjelasan:

* `-type f` → hanya file
* `-size 1033c` → ukuran tepat 1033 bytes

---

4. Tambahkan filter agar tidak executable

```bash
$ find . -type f -size 1033c ! -executable
```

```bash
$ ./maybehere07/.file2
```

Penjelasan:

* `! -executable` → bukan file executable

---

5. Setelah ditemukan file yang sesuai, cek apakah human-readable

```
$ file ./maybehere07/.file2
```

```
./maybehere07/.file2: ASCII text, with very long lines (1000)
```

---

6. Baca isi file tersebut

```bash
$ cat ./maybehere07/.file2
```

---

7. Output:

`The password you are looking for is:`
**HWasnPhtq9AVKe0dmk45nxy20cvUa6EG**

---

### Cara Lebih Cepat (Langsung Sekali Jalan)

```bash
$ find . -type f -size 1033c ! -executable -exec file {} \; | grep ASCII
```

Lalu `cat` file yang muncul.

---

### Yang Gw Pelajari

* Menggunakan `find` untuk pencarian kompleks
* Filter berdasarkan ukuran file (`-size`)
* Filter berdasarkan permission (`! -executable`)
* Menggabungkan `find`, `file`, dan `grep` untuk efisiensi
* Jangan brute-force manual, gunakan filtering