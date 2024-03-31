# Lapres-Jarkom-Modul-1-IT30-2024

## Anggota

| Nama                            | NRP          |
| ------------------------------- | ------------ |
| Gabriella Erlinda Wijaya        | `5027221018` |
| Aras Rizky Ananta               | `5027221053` |

### ATM or ATP or FTP?
1. Mengecek packet dengan protocol FTP pada `ftp.pcap` menggunakan filter `ftp && ip.src == 10.15.40.20`
2. Mencari response berupa "Login successful" lalu follow stream untuk melihat password mana yang berhasil mendapatkan response tersebut

### Evidence

### How Many Packets?

### Trace Him

### Creds

### Malwleowleo
### Whoami
### Secret
1. Mengecek packet dengan filter ftp pada file `evidence.pcap`
2. Terlihat bahwa pada filter sebelumnya terjadi pengiriman data, menggunakan filter `ftp-data` untuk melihat data apa saja yang ada
3. Terlihat ada 2 ftp-data berupa file m4L1c10us_W4re.c dan mirza.jpg
4. Untuk dapat melihat isi data, mengunduh kedua file dengan File -> Export Objects -> FTP-DATA -> Save file yang dipilih
5. Dari kedua file terlihat terdapat random text dan foto berisi pesan, lalu ketika dicoba submit pada netcat, didapatkan pesan rahasia attacker adalah `MIO MIRZA`
   
### Fuzz

### Malwaew
