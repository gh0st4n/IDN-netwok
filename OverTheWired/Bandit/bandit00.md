# Over The Wired

## Bandit Level 0

### Deskripsi
Permainan perang Bandit ditujukan untuk pemula mutlak. Permainan ini akan mengajarkan Dasar-dasar yang dibutuhkan untuk dapat memainkan permainan perang lainnya.

### Catatan
Permainan ini, seperti kebanyakan permainan lainnya, diatur dalam level. Anda memulai dari Mulailah dengan Level 0 dan cobalah untuk "menaklukkan" atau "menyelesaikan" level tersebut. Menyelesaikan level akan menghasilkan... Informasi tentang cara memulai level berikutnya.

Anda akan menghadapi banyak situasi di mana Anda tidak tahu apa yang harus Anda lakukan. Yang seharusnya dilakukan. Jangan panik! Jangan menyerah! Tujuan dari ini Permainan ini dibuat agar Anda mempelajari dasar-dasarnya.

- Pertama, jika Anda mengetahui suatu perintah tetapi tidak tahu cara menggunakannya, cobalah manual ( man page ) dengan memasukkan man <perintah> . Sebagai contoh, gunakan perintah `man ls` untuk mempelajari tentang perintah “ls”. Perintah “man” juga memiliki manual, cobalah! Saat menggunakan man, tekan `q` untuk berhenti.

- Kedua, jika tidak ada halaman manual (man page), perintah tersebut mungkin berupa shell. bawaan . Dalam hal ini gunakan perintah “ help <X> ”. Contoh: help cd. Selain itu, mesin pencari adalah teman Anda. Pelajari caranya Gunakan! Saya merekomendasikan [Google](https://www.google.com/) .

### Deksripsi Level

Tujuan dari level ini adalah agar Anda masuk ke dalam game menggunakan SSH. `Host` yang perlu Anda hubungkan adalah **bandit.labs.overthewire.org** , pada `port` **2220**. `Nama pengguna` adalah **bandit0** dan `kata sandinya` adalah **bandit0**.

### Tugas
* Menghubungkan server dengan sistem kita
* Mengambil password untuk login ke level berikutnya

### Proses

1. Membuka terminal
2. Menghubungkan Server dengan Host Gw, Cara nya :
   ```bash
   ssh bandit0@bandit.labs.overthewire.org -p 2220
   ```
   - Output :
   ```
     _                     _ _ _
    | |__   __ _ _ __   __| (_) |_
    | '_ \ / _` | '_ \ / _` | | __|
    | |_) | (_| | | | | (_| | | |_
    |_.__/ \__,_|_| |_|\__,_|_|\__|
    This is an OverTheWire game server.
    More information on http://www.overthewire.org/wargames
    backend: gibson-1
    bandit0@bandit.labs.overthewire.org's password:
   ```
3. Gw input password(bandit0) nya
    ```
    bandit0@bandit.labs.overthewire.org's password: bandit0
    ```

### Yang Gw Pelajari
#### SSH
- Bagaimana cara menggunakan ssh(Secure Shell)
- Bagaimana cara menghubungkan sistem operasi secara remote

#### Materi
- Dokumentasi ssh menggunakan perintah `man` : `man ssh`
- Dokumentasi Ubuntu-SSH : [OpenSSH](https://ubuntu.com/server/docs/openssh-server)
- Wikipedia : [Website](https://id.wikipedia.org/wiki/Secure_Shell)
- Wikihow : [Website](https://www.wikihow.com/Use-SSH)
- it's-FOSS : [It's FOSS](https://itsfoss.com/ssh-to-port/)