# Over The Wired

## Bandit Level 0 → 1

### Deskripsi Level
Pada level ini, kita akan mempelajari bagaimana cara menggunakan perintah dasar linux, seperti :
- ls
- cat
- file

### Tugas
* Mencari password yang berada di file `readme`
* Mengambil password untuk login ke level berikutnya

### Proses
1. Masuk Server :
   ```bash
   $ ssh bandit0@bandit.labs.overthewire.org -p 2220
   ```

---

2. ls (Menampilkan file & folder)
    ```bash
    $ ls atau $ ls | grep readme
    ```

---

3. file (Memastikan jenis file)
    ```
    $ file readme
    ```
    output :
    ```
    $ readme: ASCII text
    ```

---

4. cat (Membaca isi file)
    1. ```
       $ cat readme
       ```
    2. ```
       $ grep -i "password" readme
       ```
    3. ```
       $ cat readme | grep "password"
       ```

---

5. Output :

```The password you are looking for is:```
**ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If**

### Yang Gw Pelajari
- Bagaimana cara menggunakan perintah dasar linux