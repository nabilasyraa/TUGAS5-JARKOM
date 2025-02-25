Nama : Nabila Asyura Chandra Putri <br>
NIM : 09030582327098 <br>
Kelas : TK4C <br>
Mata Kuliah : Praktikum Jaringan Komputer <br>

# Packet Tracer - Konfigurasi Pengaturan Switch
## Bagian 1: Verifikasi Konfigurasi Sakelar Default
### Langkah 1: Masuk ke mode EXEC khusus
Anda dapat mengakses semua perintah sakelar dari mode EXEC khusus. Namun, karena banyak perintah khusus yang mengonfigurasi parameter operasi, akses mode khusus harus dilindungi kata sandi untuk mencegah penggunaan yang tidak sah. Kumpulan perintah EXEC khusus mencakup perintah yang tersedia dalam mode EXEC pengguna, banyak perintah tambahan, dan perintah konfigurasi yang digunakan untuk mengakses mode konfigurasi.
- Klik S1, lalu tab CLI. Tekan Enter
- Masuk ke mode EXEC istimewa dengan memasukkan perintah enable:
  ```
  Switch> enable
  Switch#
  ```
- Perhatikan bahwa prompt berubah untuk mencerminkan mode EXEC khusus.
![Image](https://github.com/user-attachments/assets/9fbc8581-a92e-4708-bd2b-f129e84e222b)

### Langkah 2: Periksa konfigurasi sakelar saat ini
Masukkan perintah show running-config.
```
Switch# show running-config
```
![Image](https://github.com/user-attachments/assets/586441db-4b16-4b53-9fa7-8d3323f18431)

## Bagian 2: Membuat Konfigurasi Sakelar Dasar
### Langkah 1: Menetapkan nama untuk sakelar
Untuk mengonfigurasi parameter pada sakelar, Anda mungkin diminta untuk berpindah di antara berbagai mode konfigurasi. Perhatikan bagaimana prompt berubah saat Anda menavigasi sakelar.
```
Switch# configure terminal
Switch(config)# hostname S1
S1(config)# exit
S1#
```
![Image](https://github.com/user-attachments/assets/e42bf53e-fe60-4350-8ba5-9a51ecfcbb79)

### Langkah 2: Mengamankan akses ke jalur konsol.
Untuk mengamankan akses ke baris konsol, akses mode config-line dan atur kata sandi konsol ke letmein.
```
S1# configure terminal
Enter configuration commands, one per line. End with CNTL/Z.
S1(config)# line console 0
S1(config-line)# password letmein
S1(config-line)# login
S1(config-line)# exit
S1(config)# exit
S1#
%SYS-5-CONFIG_I: Configured from console by console
```
![Image](https://github.com/user-attachments/assets/5b76b97a-0174-4cd9-9bc4-aa64347e17fd)

### Langkah 3: Verifikasi bahwa akses konsol telah diamankan.
Keluar dari mode istimewa untuk memverifikasi bahwa kata sandi port konsol sudah berlaku.
```
S1# exit

Switch con0 is now available

Press RETURN to get started.

 

User Access Verification

Password:

S1>
```
![Image](https://github.com/user-attachments/assets/509608b6-786f-4d36-a3d8-565fa1fe8713)

### Langkah 4: Mengamankan akses mode istimewa.
Tetapkan kata sandi pengaktifan ke c1$c0. Kata sandi ini melindungi akses ke mode istimewa.
```
S1> enable
S1# configure terminal
Enter configuration commands, one per line. End with CNTL/Z.
S1(config)# enable password c1$c0
S1(config)# exit
S1#
%SYS-5-CONFIG_I: Configured from console by console
```
![Image](https://github.com/user-attachments/assets/957cd338-80f8-4cba-aadb-a334b259de26)

### Langkah 5: Verifikasi bahwa akses mode istimewa aman.
- Masukkan perintah exit sekali lagi untuk keluar dari switch.
- Tekan <Enter> dan Anda sekarang akan diminta memasukkan kata sandi:
  ```
  User Acces Verification

  Password:
  ```
- Kata sandi pertama adalah kata sandi konsol yang Anda konfigurasikan untuk baris con 0. Masukkan kata sandi ini untuk kembali ke modus eksekusi pengguna.
- Masukkan perintah untuk mengakses mode hak khusus.
- Masukkan kata sandi kedua yang Anda konfigurasikan untuk melindungi mode EXEC khusus.
- Verifikasi konfigurasi Anda dengan memeriksa isi file running-configuration:
  ```
  S1# show running-config
  ```
![Image](https://github.com/user-attachments/assets/d5ff12f7-4402-4a97-8c7f-2a736f860abd)

### Langkah 6: Konfigurasikan kata sandi terenkripsi untuk mengamankan akses ke mode istimewa.
Kata sandi aktifkan harus diganti dengan kata sandi rahasia terenkripsi yang lebih baru dengan menggunakan perintah aktifkan rahasia. Atur kata sandi aktifkan rahasia ke itsasecret.
```
S1# config t
S1(config)# enable secret itsasecret
S1(config)# exit
S1#
```
![Image](https://github.com/user-attachments/assets/85c3bd8e-eecc-4913-8b89-0d72a3682fab)

### Langkah 7: Verifikasi bahwa enable secret password (aktifkan kata sandi rahasia) telah ditambahkan ke file konfigurasi.
Masukkan perintah show running-config lagi untuk memverifikasi kata sandi rahasia aktif yang baru telah dikonfigurasi.
```
S1# show run
```
![Image](https://github.com/user-attachments/assets/7fb71ac9-463e-49ec-a246-efd92a8af3b3)

### Langkah 8: Enkripsi kata sandi aktifkan dan konsol.
Seperti yang Anda perhatikan pada Langkah 7, kata sandi rahasia aktifkan telah dienkripsi, tetapi kata sandi aktifkan dan konsol masih berupa teks biasa. Sekarang kita akan mengenkripsi kata sandi teks biasa ini menggunakan perintah enkripsi kata sandi layanan.
```
S1# config t
S1(config)# service password-encryption
S1(config)# exit
```
![Image](https://github.com/user-attachments/assets/9e2c542a-4202-42ba-b657-b48212e6c535)

## Bagian 3: Mengonfigurasi Spanduk MOTD
### Langkah 1: Mengonfigurasi spanduk pesan hari ini (MOTD).
Kumpulan perintah Cisco IOS menyertakan fitur yang memungkinkan Anda mengonfigurasi pesan yang dilihat oleh siapa pun yang masuk ke switch. Pesan-pesan ini disebut pesan hari ini, atau spanduk MOTD. Lampirkan teks spanduk dalam tanda kutip atau gunakan pembatas yang berbeda dari karakter apa pun yang muncul dalam string MOTD.
```
S1# config t
S1(config)# banner motd "This is a secure system. Authorized Access Only!"
S1(config)# exit
S1#
%SYS-5-CONFIG_I: Configured from console by console
```
![Image](https://github.com/user-attachments/assets/ded60655-a3e8-4391-8d77-ec5daa2ea7cb)

## Bagian 4: Menyimpan dan Memverifikasi File Konfigurasi ke NVRAM
### Langkah 1: Verifikasi bahwa konfigurasi sudah akurat dengan menggunakan perintah show run.
Simpan file konfigurasi. Anda telah menyelesaikan konfigurasi dasar sakelar. Sekarang, cadangkan file konfigurasi yang sedang berjalan ke NVRAM untuk memastikan bahwa perubahan yang dibuat tidak hilang jika sistem di-boot ulang atau kehilangan daya.
```
S1# copy running-config startup-config
Destination filename [startup-config]?[Enter]
Building configuration...
[OK]
Close Configuration Window for S1
```
![Image](https://github.com/user-attachments/assets/0f601a65-ca14-41dc-8eea-d23dc3027e3b)

## Bagian 5: Mengkonfigurasi S2
- Setelah menyelesaikan konfigurasi pada S1, lanjutkan mengkonfigurasi S2. Jika sudah Klik Check Result.
![Image](https://github.com/user-attachments/assets/606f30ff-13b0-4c6b-9533-5e3b899a18d6)
- Lalu cek pada bagian Assessment Items, jika sudah benar akan ditandai dengan checklist warna hijau.
![Image](https://github.com/user-attachments/assets/c29e2782-fdaf-4df5-bfdc-4514e78d2092)
