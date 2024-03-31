# Lapres-Jarkom-Modul-1-IT30-2024

## Anggota

| Nama                            | NRP          |
| ------------------------------- | ------------ |
| Gabriella Erlinda Wijaya        | `5027221018` |
| Aras Rizky Ananta               | `5027221053` |

### > ATM or ATP or FTP?
1. Mengecek packet dengan protocol FTP pada `ftp.pcap` menggunakan filter `ftp && ip.src == 10.15.40.20`
2. Mencari response berupa "Login successful" lalu follow stream untuk melihat password mana yang berhasil mendapatkan response tersebut
3. Didapatkan password = `m4y_th3_Kn!fe_ch1p_&_sh4tter`
4. Ketika di submit pada netcat, didapatkan FLAG: `JARKOM2024{Brut3f0rce_FtP_c68CR7AHQRkeRAq}`

### > Evidence

### > How Many Packets?

### > Trace Him
1. Karena terlihat pada packet bahwa terjadi kegiatan mencurigakan untuk login, kita mengecek conversation pada packet dengan Statistics -> Conversations
2. Mengecek pada setiap protocol, didapati bahwa pada protocol `TCP 319` terjadi banyak request/packet yang terkirim dari IP `10.30.3.4`, yang terlihat juga pada laman packets di awal bahwa IP tersebut banyak mengirim request login
3. Didapatkan IP attacker adalah `10.30.3.4`
4. Ketika di submit pada netcat, didapatkan FLAG: `Flag: JARKOM2024{Wh3re'5_thE_S4uce_9JrRRcnfQ1koCAB}`

### > Creds

### > Malwleowleo

### > Whoami


### > Secret
1. Mengecek packet dengan filter `ftp` pada file `evidence.pcap`
2. Terlihat bahwa pada filter sebelumnya terjadi pengiriman data, menggunakan filter `ftp-data` untuk melihat data apa saja yang ada
3. Terlihat ada 2 ftp-data berupa file `m4L1c10us_W4re.c` dan `mirza.jpg`
4. Untuk dapat melihat isi data, mengunduh kedua file dengan File -> Export Objects -> FTP-DATA -> Save file yang dipilih
5. Dari kedua file terlihat terdapat random text dan foto berisi pesan, lalu ketika dicoba submit pada netcat, didapatkan pesan rahasia attacker adalah `MIO MIRZA`
###### Netcat submission
Ketika di submit pada netcat, didapatkan FLAG: `JARKOM2024{l0_Blm_tW_MIO_MIRZA?_chw8vOnyy6FeC8Y}`
   
### > Fuzz
##### A. IP Address attacker 
1. Karena terlihat pada packet bahwa terjadi kegiatan mencurigakan untuk login, terlihat ketika dibuka stream salah satu packet, terlihat banyak kegiatan percobaan login yang gagal
2. Mengecek conversation pada packet dengan Statistics -> Conversations, didapati bahwa pada protocol `TCP 85` terjadi banyak request/packet yang terkirim dari IP `10.33.1.154`
3. Maka didapatkan IP attacker adalah `10.33.1.154`
##### B. Port yang digunakan web server korban
Pada nomor sebelumnya, terlihat juga pada conversation, port dari attacker adalah `80`
##### C. Endpoint yang digunakan untuk login
Ketika pertama membuka packet, terlihat endpoint yang digunakan di sebelah kanan POST pada kolom info, atau bisa menggunakan filter `http.request.method == POST`, didapatkan endpoint yang digunakan adalah `/`
##### D. Tool yang digunakan attacker untuk bruteforce login
Ketika follow HTTP stream dari salah satu packet, langsung terlihat tools yang digunakan adalah Fuzz Faster U Fool v2.0.0-dev pada bagian User-Agent
##### E. Username dan password yang berhasil digunakan attacker untuk login
1. Berdasarkan HTTP stream, kita bisa melihat bahwa terdapat informasi password is incorrect pada banyak percobaan login, respon tersebut dikirim dari IP 172.20.0.2 dan dikirimkan ke attacker, maka bisa menggunakan filter `http && ip.src == 172.20.0.2 && ip.dst == 10.33.1.154`
2. Setelah diurutkan berdasarkan length, didapati ada 1 packet yang terlihat response nya berupa `Found` dimana response tersebut berbeda dari yang lain
3. Follow HTTP stream dari packet tersebut, lalu pada kolom Find, bisa dicari kata Found, ditemukan username dan password yang digunakan, diikuti dengan teks html yang **tidak ada** kata Password is incorrect
4. Didapatkan username: admin, dan password: sUp3rSecretp@ssw0rd
###### Netcat submission
Ketika di submit pada netcat, didapatkan FLAG: `JARKOM2024{s3m4ng4t_ya_<3_uJf8X7nfi1Js84B}`

### > Malwaew

