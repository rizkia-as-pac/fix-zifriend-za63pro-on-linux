# 🎹 Mengatasi Keyboard Zifriend ZA63Pro Tidak Bisa Wired Mode

## 💻 Sistem
- **OS**: EndeavourOS (Arch Linux)

---

## 🚩 Kondisi Awal
Saya coba gunakan keyboard ini dalam mode wireless 2.4G, dan hasilnya berjalan normal. Namun, ketika saya mencoba menggunakan mode wired, keyboard tidak berfungsi sama sekali. Tidak ada tombol yang merespons saat ditekan.

Untuk investigasi lebih lanjut, saya menggunakan perintah `xev`:
- **Mode Wireless**: ✅ Input terdeteksi dan terekam.
- **Mode Wired**: ❌ Tidak ada input yang terdeteksi sama sekali.

### 🔄 Percobaan di Linux Lain
Karena saya berpikir ini mungkin masalah yang hanya terjadi di Arch Linux, saya mencoba booting ke Linux Mint. Namun, saat proses booting, saya mendapatkan error seperti berikut:

<img src="https://raw.githubusercontent.com/rizkia-as-pac/fix-zifriend-za63pro-on-linux/refs/heads/main/1734652914276.jpg" width="600">

Setelah login ke Linux Mint, saya coba lagi menggunakan keyboard, tetapi mode wired tetap tidak berfungsi.

---

### 🔍 Pemeriksaan Log dengan `dmesg`
Saya memeriksa log menggunakan perintah `dmesg -w` dan menemukan log seperti berikut:

```log
[   17.656144] usb 1-2: unable to read config index 0 descriptor/start: -71
[   17.656158] usb 1-2: can't read configurations, error -71
```

---

### 💡 Solusi
Tentu saja ini bukan solusi yang saya ciptakan sendiri. Berdasarkan log tersebut, saya melakukan pencarian di internet dan menemukan solusi di sumber berikut:
[Zifriend-Keyboard-Linux](https://github.com/ken-kuro/Zifriend-Keyboard-Linux/)

#### 1️⃣ Temukan VendorID dan ProductID Keyboard
Untuk menemukan VendorID dan ProductID dari keyboard ini saya menggunakan cara berikut:
[https://kb.synology.com/en-in/DSM/tutorial/How_do_I_check_the_PID_VID_of_my_USB_device](https://kb.synology.com/en-in/DSM/tutorial/How_do_I_check_the_PID_VID_of_my_USB_device)

Di sini saya akan mencoba mendapatkan VendorID dan ProductID menggunakan Windows, karena kebetulan saya masih memiliki Windows sebagai dual boot pada komputer saya dan keyboard ini juga berfungsi dengan baik pada OS Windows.

#### 🔧 1.1 Device Manager
Buka device manager, lalu pilih keyboard. Akan ada banyak device keyboard yang dilist. Untuk menentukan mana device yang merupakan keyboard saya yang bermasalah, saya hanya lepas dan pasang kembali keyboard saya dan melihat bagian mana yang hilang dan muncul kembali. Klik kanan, lalu pilih Properties.

<img src="https://raw.githubusercontent.com/rizkia-as-pac/fix-zifriend-za63pro-on-linux/refs/heads/main/2342341241412.png" width="600">

#### 📋 1.2 VendorID dan ProductID
Setelah di dalam Properties, klik pada Details, lalu pada Property pilih **Hardware Ids**. Dari gambar berikut, bisa dilihat bahwa keyboard saya memiliki VendorID `5566` dan ProductID `0008`.

<img src="https://raw.githubusercontent.com/rizkia-as-pac/fix-zifriend-za63pro-on-linux/refs/heads/main/3489589435689.png" width="600">

---

#### 🛠️ 2. Mengubah Kernel Parameters
Buka file konfigurasi GRUB:
```shell
sudo nvim /etc/default/grub
```

Temukan line berikut:
```shell
GRUB_CMDLINE_LINUX_DEFAULT
```

Tambahkan `usbcore.quirks=<VID>:<PID>:gki` pada nilai `GRUB_CMDLINE_LINUX_DEFAULT`:
```shell
# Pada sistem saya defaultnya seperti berikut
# GRUB_CMDLINE_LINUX_DEFAULT='nowatchdog nvme_load=YES loglevel=3'
# Setelah saya ubah menjadi seperti berikut
GRUB_CMDLINE_LINUX_DEFAULT='nowatchdog nvme_load=YES loglevel=3 usbcore.quirks=5566:0008:gki'
```

Simpan perubahan tersebut.

---

#### 🔄 3. Update GRUB
Karena saya menggunakan Arch Linux dengan sistem UEFI, saya jalankan perintah berikut:
```shell
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

---

#### 🔁 4. Reboot Sistem

---

## ✅ Hasil Akhir
Setelah langkah-langkah di atas, keyboard saya sekarang bisa digunakan kembali dengan mode wired. 🎉
