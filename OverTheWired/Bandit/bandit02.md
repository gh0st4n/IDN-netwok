# Over The Wired

## Bandit Level 1 → 2

### Deskripsi Level
Pada level ini, kita akan mempelajari bagaimana cara menggunakan perintah dasar linux, seperti :
- ls
- cat
- file

### Tugas
- Mencari password yang berada di file `readme`

### Proses
1. ls (Menampilkan file & folder)
    ```bash
    $ ls atau $ ls | grep readme
    ```
2. file (Memastikan jenis file)
    ```
    file readme
    ```
    output :
    ```
    readme: ASCII text
    ```
3. cat (Membaca isi file)
    - ```
       $ cat readme
       ```
    - ```
       $ grep -i "password" readme
       ```
    - ```
       $ cat readme | grep "password"
       ```
4. Output :

```The password you are looking for is:```
**ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If**

### Yang Gw Pelajari
- Bagaimana cara menggunakan perintah dasar linux