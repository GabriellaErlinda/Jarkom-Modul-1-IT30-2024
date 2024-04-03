# Lapres-Jarkom-Modul-1-IT30-2024

## Anggota

| Nama                            | NRP          |
| ------------------------------- | ------------ |
| Gabriella Erlinda Wijaya        | `5027221018` |
| Aras Rizky Ananta               | `5027221053` |

- [ATM or ATP or FTP?](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/blob/main/README.md#-atm-or-atp-or-ftp)
- [evidence](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/blob/main/README.md#-evidence)
- [How many packets?](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/blob/main/README.md#-how-many-packets)
- [trace him](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/blob/main/README.md#-trace-him)
- [creds](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/blob/main/README.md#-creds)
- [malwleowleo](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/blob/main/README.md#-malwleowleo)
- [whoami](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/blob/main/README.md#-whoami)
- [secret](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/blob/main/README.md#-secret)
- [fuzz](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/blob/main/README.md#-fuzz)
- [malwaew](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/blob/main/README.md#-malwaew)


## >> ATM or ATP or FTP?
1. Mengecek packet dengan protocol FTP pada `ftp.pcap` menggunakan filter `ftp && ip.src == 10.15.40.20`
2. Mencari response berupa "Login successful" lalu follow stream untuk melihat password mana yang berhasil mendapatkan response tersebut
![Screenshot 2024-03-31 001029](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/assets/128443451/9b12194e-b89e-4a5a-b173-7563864331ee)
![Screenshot 2024-03-31 001132](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/assets/128443451/003c2293-de5d-429f-91d6-39c0b10295f1)

3. Didapatkan password = `m4y_th3_Kn!fe_ch1p_&_sh4tter`
###### Netcat submission
```
nc 10.15.40.20 10004
Jawab pertanyaan-pertanyaan yang telah disediakan:

No 4:
Pertanyaan: Apa password yang berhasil didapatkan oleh hacker setelah melakukan bruteforce login ftp?
Format: strings
Jawaban: m4y_th3_Kn!fe_ch1p_&_sh4tter
Correct

Congrats! Flag: JARKOM2024{Brut3f0rce_FtP_ITfCY7pygRFeC8Y}
```

## >> Evidence
Pada file challenge.pcapng, terlihat bahwa terdapat banyak packets dengan protocol yang berbeda-beda, didapati packet dengan protocol HTTP, lalu kita bisa membuka salah satu HTTP stream. Terlihat pada stream tersebut terdapat informasi seperti server dll.
##### A. Domain korban
1. Menggunakan filter http untuk melihat packet dengan protocol http, setelah di filter, kita bisa melihat beberapa info response. Setelah diurutkan berdasarkan length, didapati ada response yang menyatakan Found, lalu kita buka salah satu packet tersebut.
2. Pada packet yang dibuka, terlihat informasi seperti host, server, dll. Didapatkan domain korban = `nanomate-solutions.com`
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/assets/128443451/b0878bfd-b487-4f7c-8efe-23f187457afb)

##### B. Web server yang digunakan korban
- Sama seperti nomor sebelumnya, pada stream tersebut terlihat web server yang digunakan adalah Apache/2.4.56
##### C. Endpoint yang digunakan untuk login sebagai user biasa
1. Pada stream yang sama, didapati tulisan method POST
2. Pada sebelah kanan method POST tersebut bisa dilihat endpoint yang digunakan user untuk login adalah `/app/includes/process_login.php`
##### D. Email dan password yang berhasil digunakan untuk login sebagai user biasa
1. Dikarenakan pada stream sebelumnya terdapat informasi Invalid Username or Password, maka perlu dicari stream lain
2. Ketika dilakukan navigasi ke packet lain, didapati pada frame di kiri bawah, pada bagian Line-based text data: text/html (1 lines), ada packet yang memiliki response "Login Successful"
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/assets/128443451/cb06327f-f4bf-4a9a-bc8f-da7a6773899a)

4. Buka HTTP stream dari packet tersebut, lalu didapati email dan password yang benar
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/assets/128443451/24a8b7c0-154f-420e-b651-190d7b49c843)

6. Email: `tareq@gmail.com`, password: `tareq@nanomate`
###### Netcat submission
```
nc 10.15.40.20 10002
Jawab pertanyaan-pertanyaan yang telah disediakan:

No 1:
Pertanyaan: Apa domain milik korban?
Format: xxxxxx.xxx: e.g. google.com
Jawaban: nanomate-solutions.com
Correct

No 2:
Pertanyaan: Apa web server yang digunakan oleh korban?
Format: name-version: e.g. nginx-1.18.0
Jawaban: apache-2.4.56
Correct

No 3:
Pertanyaan: Apa endpoint yang digunakan untuk login sebagai user biasa?
Format: /path/to/endpoint
Jawaban: /app/includes/process_login.php
Correct

No 4:
Pertanyaan: Apa email dan password yang berhasil digunakan untuk login sebagai user biasa?
Format: email:password
Jawaban: tareq@gmail.com:tareq@nanomate
Correct

Congrats! Flag: JARKOM2024{m4innya_h3bat_uT8lY7xyQAJt84t}
```

## >> How Many Packets?
1.Diketahui bahwa IP Attacker(yang tercatat) adalah 10.30.3.4 untuk mencari jumlah bruteforce login bisa menggunakan IP itu sebagai destinasi(ip.dst)
2.Menghitung berapa kali sistem memperingati bahwa password keliru(bruteforce login adalah login dengan nama dan password yang salah)
3.Gunakan Ctrl+Shift+LMB untuk memilih log?? yang memiliki info diatas dan lihat jumlah yang terpilih
![a](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/blob/patch-1/Screenshot%202024-04-03%20182608.png)
###### Netcat submission
```
nc 10.15.40.20 10005
Jawab pertanyaan-pertanyaan yang telah disediakan:

No 5:
Pertanyaan: Berapa total attempt login(bruteforce) yang dilakukan hacker?
Format: number
Jawaban: 934
\Correct

Congrats! Flag: JARKOM2024{c0unT_uR_P4cket5_Ih8kYzAf16kel4t}
```

## >> Trace Him
1. Karena terlihat pada packet bahwa terjadi kegiatan mencurigakan untuk login, kita mengecek conversation pada packet dengan Statistics -> Conversations
2. Mengecek pada setiap protocol, didapati bahwa pada protocol `TCP 319` terjadi banyak request/packet yang terkirim dari IP `10.30.3.4`, yang terlihat juga pada laman packets di awal bahwa IP tersebut banyak mengirim request login
![Screenshot 2024-03-31 224057](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/assets/128443451/2a6b77f3-1f90-414a-b46f-9cd24afb311c)
3. Didapatkan IP attacker adalah `10.30.3.4`
###### Netcat submission
```
nc 10.15.40.20 10006
Jawab pertanyaan-pertanyaan yang telah disediakan:

No 6:
Pertanyaan: Alamat IP attacker?
Format: xxx.xxx.xxx.xxx
Jawaban: 10.30.3.4
Correct

Congrats! Flag: JARKOM2024{Wh3re'5_thE_S4uce_9T8lRzAtizFe84B}
```

## >> Creds
1.Ditemukan pada awal bahwa server telah merespons pada sebuah IP address yang masuk(10.30.3.1)
2.Filter dengan ip.src untuk menemukan input login pembobol
3.Ditemui kredensial yang sukses berada diatas Respons 230
![b](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/blob/main/Screenshot%202024-03-30%20231330.png)
###### Netcat submission
```
nc 10.15.40.20 10007
Jawab pertanyaan-pertanyaan yang telah disediakan:

No 1:
Pertanyaan:Apa username FTP yang digunakan oleh Attacker?
Format: USER:username
Jawaban: USER:h3ngk3rTzy

No.2:
Pertanyaan:Apa password FTP yang digunakan oleh Attacker?
Format: PASS:password
Jawaban: PASS:S!l3ncE


Congrats! Flag: JARKOM2024{s3curE_uR_FtP_uhCCXOAHy1dt189}
```

## >> Malwleowleo
1.Untuk mengetahui malware yang dikirim bisa menggunaakan follow TCP stream pada username attacker
2.Diketahui bahwa file yang dikirim adalah m4L1c10us_W4re.c 
![c](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/blob/main/Screenshot%202024-03-30%20235543.png)
###### Netcat submission
```
nc 10.15.40.20 10008
Jawab pertanyaan-pertanyaan yang telah disediakan:

No 8:
Pertanyaan: Apa nama malware yang dikirim oleh attacker ke korban?
Format: strings
Jawaban: m4L1c10us_W4re.c
Correct

Congrats! Flag: JARKOM2024{beC4reful_0f_m4lwAr3_9JCCPcxty6doC8q}
```

## >> Whoami
1.Untuk nama attacker bisa diketahui dengan TCP follow file m4L1c10us_W4re.c,lebih tepatnya FTP-DATA 
2.Disitu pada kode C ketemu komen dengan bahasa Base64
3.Jika ditranslasi akan ketemu nama Paul Atreides
![d](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/blob/main/Screenshot%202024-03-31%20000310.png)
###### Netcat submission
```
No 9:
Pertanyaan: Siapa nama attacker yang sudah melakukan serangan ini?
Format: FirstName_LastName
Jawaban: Paul_Atreides
Correct

Congrats! Flag: JARKOM2024{Duk3_0f_4ra!s_LISAN AL GHAIB!_u688v7AHyRFHCBY}
```
## >> Secret
1. Mengecek packet dengan filter `ftp` pada file `evidence.pcap`
2. Terlihat bahwa pada filter sebelumnya terjadi pengiriman data, menggunakan filter `ftp-data` untuk melihat data apa saja yang ada
3. Terlihat ada 2 ftp-data berupa file `m4L1c10us_W4re.c` dan `mirza.jpg`
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/assets/128443451/de605db2-14d7-4824-a79d-db4dc5b3a64b)

5. Untuk dapat melihat isi data, mengunduh kedua file dengan File -> Export Objects -> FTP-DATA -> Save file yang dipilih
![Screenshot 2024-03-31 213812](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/assets/128443451/d4ce4ce3-c027-4b29-9ff4-7eb8e4e0dfd6)
![Screenshot 2024-03-31 213830](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/assets/128443451/5d8acc76-7a30-4908-a8c0-a2be4540ef07)

6. Dari kedua file terlihat terdapat random text dan foto berisi pesan, lalu ketika dicoba submit pada netcat, didapatkan pesan rahasia attacker adalah `MIO MIRZA`

![Screenshot 2024-03-31 213903](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/assets/128443451/22864c52-919a-4313-a1d9-a59d8d12fcdd)

###### Netcat submission
```
nc 10.15.40.20 10010
Jawab pertanyaan-pertanyaan yang telah disediakan:

No 10:
Pertanyaan: Ternyata attacker menyisipkan file lainnya selain dari file malware, temukan pesan yg dikutip oleh attacker?
Format: strings
Jawaban: MIO MIRZA
Correct

Congrats! Flag: JARKOM2024{l0_Blm_tW_MIO_MIRZA?_uhwCYzAyQ1dHC8B}
```
   
## >> Fuzz
##### A. IP Address attacker 
1. Karena terlihat pada packet bahwa terjadi kegiatan mencurigakan untuk login, terlihat ketika dibuka stream salah satu packet, terlihat banyak kegiatan percobaan login yang gagal
2. Mengecek conversation pada packet dengan Statistics -> Conversations, didapati bahwa pada protocol `TCP 85` terjadi banyak request/packet yang terkirim dari IP `10.33.1.154`
![Screenshot 2024-03-31 224606](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/assets/128443451/b5ae718d-68a8-4d0f-b84b-4e722f4198fc)

3. Maka didapatkan IP attacker adalah `10.33.1.154`
##### B. Port yang digunakan web server korban
Pada nomor sebelumnya, terlihat juga pada conversation, port dari attacker adalah `80`
##### C. Endpoint yang digunakan untuk login
Ketika pertama membuka packet, terlihat endpoint yang digunakan di sebelah kanan POST pada kolom info, atau bisa menggunakan filter `http.request.method == POST`, didapatkan endpoint yang digunakan adalah `/`
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/assets/128443451/a237472b-6aac-4f84-afc7-14cec4dcba5f)

##### D. Tool yang digunakan attacker untuk bruteforce login
Ketika follow HTTP stream dari salah satu packet, langsung terlihat tools yang digunakan adalah Fuzz Faster U Fool v2.0.0-dev pada bagian User-Agent
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/assets/128443451/059ee281-333e-451d-a78d-73225a1bcb94)

##### E. Username dan password yang berhasil digunakan attacker untuk login
1. Berdasarkan HTTP stream, kita bisa melihat bahwa terdapat informasi password is incorrect pada banyak percobaan login, respon tersebut dikirim dari IP 172.20.0.2 dan dikirimkan ke attacker, maka bisa menggunakan filter `http && ip.src == 172.20.0.2 && ip.dst == 10.33.1.154 && http contains "Found"`
2. Didapati ada 1 packet yang terlihat response nya berupa `Found` dimana response tersebut menandakan bahwa percobaan login berhasil
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/assets/128443451/f3e67147-5df6-48e0-b5fb-ae2c31cff375)

3. Follow HTTP stream dari packet tersebut, lalu pada kolom Find, bisa dicari kata Found, ditemukan username dan password yang digunakan, diikuti dengan teks html yang **tidak ada** kata Password is incorrect
 ![image](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/assets/128443451/ae198533-a5cd-429f-a29c-88c9d326cc8f)

4. Didapatkan username: admin, dan password: sUp3rSecretp@ssw0rd
###### Netcat submission
```
nc 10.15.40.20 10001
Jawab pertanyaan-pertanyaan yang telah disediakan:

No 1:
Pertanyaan: Apa IP address milik attacker?
Format: xxx.xxx.xxx.xxx
Jawaban: 10.33.1.154
Correct

No 2:
Pertanyaan: Apa port yang digunakan sebagai web server korban?
Format: xxxx: e.g. 22
Jawaban: 80
Correct

No 3:
Pertanyaan: Apa endpoint yang digunakan untuk login?
Format: /path/to/endpoint
Jawaban: /
Correct

No 4:
Pertanyaan: Apa tool yang digunakan oleh attacker untuk bruteforce login?
Format: toolsname-version: e.g. hydra-v9.0-dev
Jawaban: ffuf-v2.0.0-dev
Correct

No 5:
Pertanyaan: Apa username dan password yang berhasil digunakan oleh attacker?
Format: username:password
Jawaban: admin:sUp3rSecretp@ssw0rd
Correct

Congrats! Flag: JARKOM2024{s3m4ng4t_ya_<3_9hfCRbxfi6VH88Y}
```

## >> Malwaew
1. Ketika pertama kali pcap dibuka, terlihat pada protocol TLS, terjadi komunikasi antara web server dan client yang terenkripsi TLS,  ketika digunakan filter http, tidak ada packet yang muncul, maka perlu dilakukan decrypt TLS untuk melihat seluruh komunikasi data yang ada.
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/assets/128443451/b0e52e15-8ff2-4cac-b408-15c3fa5a6cb9)
![image](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/assets/128443451/723d91a7-3414-47c7-b6f2-968aca853d52)

2. Dikarenakan packet TLS masih terenkripsi, dilakukan decrypt TLS dengan menggunakan keylog file yang telah diberikan. Decrypt TLS dapat dilakukan dengan Edit -> Preference
![Screenshot 2024-04-01 110907](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/assets/128443451/64666321-439b-45d5-9857-9b216adcfcc3)

3 Pada Preferences, dapat dilihat pada bagian Protocol -> TLS, lalu kita dapat memasukkan `keylog.txt` yang telah diberikan pada (Pre)-Master-Secret log filename
![Screenshot 2024-04-01 111106](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/assets/128443451/5b5f725a-c338-4b05-a8eb-aaeb4a9530d2)

4. Setelah dilakukan decrypt TLS, kita dapat menggunakan filter `http` untuk melihat kegiatan pengiriman/penerimaan data dari method POST atau GET. Didapati ada 1 file dengan ekstensi .dll yang dimana merupakan executable file dan bisa ditanami malware
![Screenshot 2024-04-01 112122](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/assets/128443451/02451c92-ef9d-4bf6-b8d8-248b44085d14)

5. Kita dapat mencoba membuka file .dll tersebut dengan mengunduhnya dengan File -> Export Objects -> HTTP, lalu kita unduh file invest_20.dll tadi
![Screenshot 2024-04-01 112727](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/assets/128443451/1463ca30-8be6-4bd9-bc33-81f4ffb40c6b)

6. Setelah di download, pada soal diminta untuk mendapatkan SHA-256 hash dari malware yang dimaksud (invest_20.dll), maka perlu dilakukan decrypt SHA-256, disini saya menggunakan terminal parrot dengan command
```
shasum -a 256 invest_20.dll
```
Lalu didapatkan hasil decrypt nya
![Screenshot 2024-04-01 113150](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/assets/128443451/00fc7cdb-9490-44c5-8b9a-028464082f69)

7. Setelah didapatkan hasilnya, dapat disubmit pada netcat
![Screenshot 2024-04-01 113330](https://github.com/GabriellaErlinda/Jarkom-Modul-1-IT30-2024/assets/128443451/1b21a790-53c1-4312-84d6-76bfa883540f)

###### Netcat submission
```
nc 10.15.40.20 10003
Jawab pertanyaan-pertanyaan yang telah disediakan:

No 1:
Pertanyaan: Apa SHA-256 hash dari malware yang dimaksud?
Format: sha256 hash: e.g. c8cf761b68c5f693e5cf2d23acdcca1c0295c1f739805d60df46e54e37ada3da
Jawaban: 31cf42b2a7c5c558f44cfc67684cc344c17d4946d3a1e0b2cecb8eb58173cb2f
Correct

Congrats! Flag: JARKOM2024{wow_how_u_solve_this_IhCRYbxyl6Ftk4Y}
```
