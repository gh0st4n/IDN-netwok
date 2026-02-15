# Over The Wired

## Bandit Level 1 → 2

### Deskripsi Level

Pada level ini, kita belajar bagaimana menangani **nama file khusus** di Linux.

File password untuk level berikutnya tersimpan dalam file bernama:

```
-
```

Karakter `-` di Linux biasanya digunakan sebagai representasi **stdin (standard input)**.
Jadi kalau tidak hati-hati, perintah bisa salah membaca.

Perintah yang digunakan di level ini:

* ls
* cd
* cat

### Tugas

* Membaca isi file bernama `-` yang berada di home directory
* Mengambil password untuk login ke level berikutnya

### Proses

1. Masuk Server

```bash
$ ssh bandit1@bandit.labs.overthewire.org -p 2220
```

---

2. Melihat isi directory

```bash
$ ls -la
```

Output akan terlihat ada file bernama:

```
-
```

---

3. Jika kita jalankan:

```bash
$ cat -
```

---

Perintah ini **tidak akan membaca file**, karena `-` dianggap sebagai stdin.

4. Cara yang benar untuk membaca file tersebut:

   1. Menggunakan path eksplisit

   ```bash
   $ cat ./-
   ```

---

5. Output:

`The password you are looking for is:`
**263JGJPfgU6LtdEvgfWU1XP5yac29mFx**

---

### Yang Gw Pelajari

* Linux memperbolehkan nama file menggunakan karakter khusus
* Tanda `-` sering dipakai sebagai stdin oleh banyak program
* Menggunakan `./` untuk menangani file dengan nama aneh
* Memahami cara kerja argument parsing pada CLI

