# Over The Wired

## Bandit Level 6 → 7

### Deskripsi Level

Pada level ini, kita belajar melakukan **pencarian file secara global di server berdasarkan ownership dan ukuran**.

Password untuk level berikutnya:

* Dimiliki oleh user **bandit7**
* Dimiliki oleh group **bandit6**
* Berukuran **33 bytes**
* Tersimpan di suatu tempat di server

Artinya:

* File tidak berada hanya di home directory
* Harus mencari dari root (`/`)
* Harus memfilter berdasarkan owner, group, dan size

Perintah yang digunakan:

* ls
* cd
* cat
* file
* du
* find
* grep

---

### Tugas

* Menemukan file di seluruh server yang:
  * User owner = bandit7
  * Group owner = bandit6
  * Size = 33 bytes
* Membaca isinya untuk mendapatkan password Level 7

---

### Proses

1. Masuk Server :

```bash
$ ssh bandit6@bandit.labs.overthewire.org -p 2220
```

---

2. Lakukan pencarian dari root directory

```bash
$ find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

Penjelasan:

* `/` → cari dari root
* `-type f` → hanya file
* `-user bandit7` → owner user bandit7
* `-group bandit6` → group bandit6
* `-size 33c` → ukuran tepat 33 bytes
* `2>/dev/null` → sembunyikan error permission denied

---

3. Akan muncul satu path file :

```
/var/lib/dpkg/info/bandit7.password
```

4. Baca isi file tersebut

```bash
$ cat /var/lib/dpkg/info/bandit7.password
```

---

5. Output:

`The password you are looking for is:`
**morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj**

---

### Kenapa Perlu `2>/dev/null` ?

Karena saat mencari dari `/`, akan banyak directory yang tidak bisa diakses.
Tanpa redirect error, output akan penuh dengan:

```
Permission denied
```

`2>/dev/null` membuang error stream (stderr).

---

### Alternatif Tanpa Langsung dari Root

Kadang file berada di `/var` atau `/home`, tapi cara paling efisien tetap dari `/`.

---

### Yang Gw Pelajari

* Menggunakan `find` untuk pencarian global
* Filter berdasarkan:

  * Owner user (`-user`)
  * Owner group (`-group`)
  * Ukuran (`-size`)
* Menggunakan redirection error (`2>/dev/null`)
* Konsep permission di Linux
* Ownership user & group dalam sistem multi-user

---

Intinya:
Level ini melatih **precision search + understanding ownership model Linux**.

Kalau mau lanjut, gw buatkan Level 7 → 8.
