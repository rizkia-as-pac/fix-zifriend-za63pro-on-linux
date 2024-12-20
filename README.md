# Mengatasi Keyboard Zifriend ZA63Pro Tidak Bisa Wired Mode

## Sistem
- **OS**: EndeavourOS (Arch Linux)

---

## Kondisi Awal
Saya coba gunakan keyboard ini dalam mode wireless 2.4G, dan hasilnya berjalan normal. Namun, ketika saya mencoba menggunakan mode wired, keyboard tidak berfungsi sama sekali. Tidak ada tombol yang merespons saat ditekan.

Untuk investigasi lebih lanjut, saya menggunakan perintah `xev`:
- **Mode Wireless**: Input terdeteksi dan terekam.
- **Mode Wired**: Tidak ada input yang terdeteksi sama sekali.

### Percobaan di Linux Lain
Karena saya berpikir ini mungkin masalah yang hanya terjadi di Arch Linux, saya mencoba booting ke Linux Mint. Namun, saat proses booting, saya mendapatkan error seperti berikut:

<img src="https://raw.githubusercontent.com/rizkia-as-pac/fix-zifriend-za63pro-on-linux/refs/heads/main/1734652914276.jpg" width="600">

Setelah login ke Linux Mint, saya coba lagi menggunakan keyboard, tetapi mode wired tetap tidak berfungsi.

---

### Pemeriksaan Log dengan `dmesg`
Saya memeriksa log menggunakan perintah `dmesg` dan menemukan log seperti berikut:

```log
error log
```

### Solusi
Tentu saja ini bukan solusi yang saya ciptakan sendiri. Berdasarkan log tersebut, saya melakukan pencarian di internet dan menemukan solusi di sumber berikut:
[Zifriend-Keyboard-Linux](https://github.com/ken-kuro/Zifriend-Keyboard-Linux/)

#### 1. Temukan VendorID dan ProductID keyboard
Untuk menemukan VendorID dan ProductID dari keyboard ini saya menggunakan cara berikut:
[https://kb.synology.com/en-in/DSM/tutorial/How_do_I_check_the_PID_VID_of_my_USB_device](https://kb.synology.com/en-in/DSM/tutorial/How_do_I_check_the_PID_VID_of_my_USB_device)

disini saya akan mencoba mendapatkan VendorID dan ProductID menggunakan windows, alasannya karena kebetulan saya masih memiliki windows sebagai dual boot pada komputer saya dan keyboard ini juga berfungsi dengan baik pada OS windows.

#### 1.1 Device Manager
Buka device manager lalu, pilih keyboard. Disini akan ada banyak device keyboard yang dilist. Untuk menentukan mana device yang merupakan keyboard saya yang bermasalah saya hanya lepas dan pasang kembali keyboard saya dan melihat bagian mana yang hilang dan muncul kembali.

<img src="https://raw.githubusercontent.com/rizkia-as-pac/fix-zifriend-za63pro-on-linux/refs/heads/main/2342341241412.png" width="600">

#### 1.1 

