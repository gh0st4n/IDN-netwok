# Over The Wired

## Bandit Level 2 → 3

### Deskripsi Level

Pada level ini, kita belajar bagaimana menangani **nama file yang mengandung spasi**.

Password untuk level berikutnya tersimpan dalam file bernama:

```
--spaces in this filename--
```

File tersebut berada di home directory.

Masalahnya:
Shell memisahkan argument berdasarkan **spasi**.
Jika tidak ditangani dengan benar, nama file akan dianggap sebagai beberapa argument berbeda.

Perintah yang digunakan di level ini:

* ls
* cd
* cat
* file
* du
* find

---

### Tugas

* Membaca isi file bernama `--spaces in this filename--`
* Mengambil password untuk login ke Level 3

---

### Proses

1. Masuk Server :

```bash
$ ssh bandit2@bandit.labs.overthewire.org -p 2220
```

---

2. Lihat isi directory

```bash
$ ls -la
```

Akan terlihat file dengan nama:

```
--spaces in this filename--
```

---

3. Jika kita jalankan tanpa penanganan khusus:

```bash
$ cat --spaces in this filename--
```

Itu akan error, karena shell membaca sebagai beberapa argument:

* --spaces
* in
* this
* filename--

---

4. Cara yang benar untuk membaca file tersebut:

   1. Menggunakan tanda kutip (paling umum)

   ```bash
   $ cat -- "--spaces in this filename--"
   ```

   atau

   ```bash
   $ cat -- '--spaces in this filename--'
   ```

   2. Menggunakan escape character `\`

   ```bash
   $ cat -- --spaces\ in\ this\ filename--
   ```

---

5. Output:

`The password you are looking for is:`
**MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx**

---

### Yang Gw Pelajari

* Shell memisahkan argument berdasarkan spasi
* Nama file dengan spasi harus:

  * Dibungkus tanda kutip
  * Atau di-escape dengan `\`
* Memahami bagaimana shell melakukan parsing argument