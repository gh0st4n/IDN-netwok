# Over The Wired

## Bandit Level 7 → 8

### Deskripsi Level

Pada level ini, kita belajar melakukan **pencarian string spesifik di dalam file besar**.

Password untuk level berikutnya tersimpan di file:

```
data.txt
```

Password tersebut berada **di samping kata `millionth`**.

Artinya:

* File kemungkinan besar berisi banyak baris
* Kita harus mencari baris yang mengandung kata tertentu
* Tidak perlu baca manual satu per satu

Perintah yang digunakan:

* man
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

Namun untuk level ini, yang paling relevan adalah:

* `grep`

---

### Tugas

* Mencari baris yang mengandung kata `millionth` di file `data.txt`
* Mengambil password yang berada di baris tersebut

---

### Proses

1. Masuk Server :

```bash
$ ssh bandit7@bandit.labs.overthewire.org -p 2220
```

---

2. Lihat isi directory

```bash
$ ls
```

Akan terlihat:

```
data.txt
```

---

3. Jangan lakukan ini (tidak efisien):

```bash
$ cat data.txt
```

Karena file besar → tidak praktis dibaca manual.

---

4. Gunakan `grep` untuk mencari kata `millionth`

```bash
$ grep millionth data.txt
```

Output akan terlihat seperti:

```
millionth    dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```

---

5. Password berada di sebelah kata `millionth`

`millionth    dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc`

Maka:

```
dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```

adalah password untuk Level 8.

---

### Alternatif Lebih Presisi

Jika ingin hanya mengambil password saja:

```bash
$ grep millionth data.txt | awk '{print $2}'
```

Atau:

```bash
$ grep millionth data.txt | cut -d ' ' -f 2
```

---

### Yang Gw Pelajari

* Menggunakan `grep` untuk pencarian string
* Tidak semua file perlu dibaca manual
* Efisiensi CLI lebih penting daripada brute force
* Filtering adalah skill utama dalam Linux