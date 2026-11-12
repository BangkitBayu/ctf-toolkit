# 🛡️ Linux System Hardening Cheat Sheet

Panduan praktis dan terstruktur untuk mengamankan (_hardening_) server berbasis Linux (Debian/Ubuntu) guna meminimalkan _attack surface_ saat kompetisi _cybersecurity_ (Defense / Blue Team / CTF).

---

## 📋 1. Persiapan & Inventarisasi Sistem (Recon Lokal)

Sebelum melakukan perubahan, lakukan pemetaan kondisi awal sistem dan pastikan paket keamanan standar sudah terpasang.

### A. Paket Pendukung Wajib

| Package                | Perintah Instalasi                     | Fungsi Utama                                              |
| :--------------------- | :------------------------------------- | :-------------------------------------------------------- |
| **`libpam-pwquality`** | `sudo apt install -y libpam-pwquality` | Modul PAM untuk validasi kompleksitas _password_.         |
| **`libpam-modules`**   | `sudo apt install -y libpam-modules`   | Kumpulan modul autentikasi dasar Linux PAM.               |
| **`openssh-server`**   | `sudo apt install -y openssh-server`   | Layanan akses _remote_ SSH yang aman.                     |
| **`ufw` / `iptables`** | `sudo apt install -y ufw`              | Manajemen _firewall_ host untuk membatasi akses jaringan. |

```bash
# Cek daftar paket yang sudah terinstal
dpkg -l | grep -E "pam|openssh|ufw"
```

### B. Audit User, Grup, dan Hak Akses (Privilege Audit)

Pastikan tidak ada akun mencurigakan atau memiliki hak akses _root_ ilegal:

```bash
# Cek semua user yang memiliki akses shell login (/bin/bash atau /bin/sh)
grep -E "sh$" /etc/passwd

# Cek apakah ada user selain 'root' yang memiliki UID 0
awk -F: '($3 == "0") {print}' /etc/passwd

# Cek anggota grup 'sudo' atau 'wheel'
getent group sudo

# Cek file dengan izin SUID/SGID (potensi privilege escalation)
find / -perm /6000 -type f 2>/dev/null
```

---

## 🔐 2. Privileged Access Management (PAM) & Autentikasi

PAM (_Pluggable Authentication Modules_) mengatur mekanisme autentikasi dan eskalasi hak akses di direktori `/etc/pam.d/` serta `/etc/security/`.

### A. Prinsip Utama Keamanan Akses

- **Least Privilege:** Berikan hak akses seminimal mungkin. Batasi aturan di `/etc/sudoers` (gunakan `visudo`), hindari pemberian akses `(ALL:ALL) ALL` tanpa alasan kuat.
- **Separation of Duties:** Gunakan perintah `sudo` (yang tercatat di `/var/log/auth.log`) daripada login langsung sebagai `root`.
- **Defense-in-Depth:** Kombinasikan kebijakan _password_ kuat, mekanisme _account lockout_, dan pembatasan akses SSH.

### B. Kebijakan Kompleksitas Password (`/etc/security/pwquality.conf`)

| Parameter          | Nilai Rekomendasi (CIS L1) | Keterangan                                                           |
| :----------------- | :------------------------- | :------------------------------------------------------------------- |
| `minlen`           | `14`                       | Panjang minimum karakter _password_.                                 |
| `minclass`         | `4`                        | Wajib mencakup 4 jenis karakter (huruf besar, kecil, angka, simbol). |
| `maxrepeat`        | `3`                        | Batas maksimal pengulangan karakter yang sama secara berurutan.      |
| `difok`            | `2`                        | Jumlah minimal karakter yang harus berbeda dari _password_ lama.     |
| `enforce_for_root` | _(Aktifkan)_               | Menerapkan aturan kompleksitas ini juga untuk akun `root`.           |

```bash
# Konfigurasi cepat kompleksitas password
sudo sed -i 's/^# *minlen.*/minlen = 14/' /etc/security/pwquality.conf
sudo sed -i 's/^# *minclass.*/minclass = 4/' /etc/security/pwquality.conf
sudo sed -i 's/^# *maxrepeat.*/maxrepeat = 3/' /etc/security/pwquality.conf
sudo sed -i 's/^# *difok.*/difok = 2/' /etc/security/pwquality.conf
```

### C. Kebijakan Account Lockout (`/etc/security/faillock.conf`)

Mencegah serangan _brute-force_ pada login lokal maupun layanan yang terintegrasi PAM:

| Parameter       | Nilai Rekomendasi (CIS L1) | Keterangan                                                     |
| :-------------- | :------------------------- | :------------------------------------------------------------- |
| `deny`          | `5`                        | Mengunci akun setelah 5 kali percobaan login gagal.            |
| `unlock_time`   | `900`                      | Durasi penguncian otomatis dalam detik (900 detik = 15 menit). |
| `fail_interval` | `900`                      | Jendela waktu penghitungan percobaan gagal.                    |

```bash
# Tambahkan aturan faillock
printf 'deny = 5\nunlock_time = 900\nfail_interval = 900\n' | sudo tee -a /etc/security/faillock.conf

# Cek status kegagalan login user tertentu & reset jika terkunci
faillock --user <USERNAME>
sudo faillock --user <USERNAME> --reset
```

---

## 🌐 3. Pengamanan Jaringan & SSH Hardening

### A. Pengamanan Layanan SSH (`/etc/ssh/sshd_config`)

SSH adalah target utama serangan _remote_. Ubah parameter berikut pada `/etc/ssh/sshd_config`:

```text
PermitRootLogin no              # Larang login langsung sebagai root
PasswordAuthentication no       # Wajibkan SSH Key (aktifkan hanya jika key sudah terpasang!)
PermitEmptyPasswords no         # Tolak password kosong
MaxAuthTries 3                  # Batasi maksimal percobaan autentikasi per koneksi
X11Forwarding no                # Matikan X11 forwarding jika tidak digunakan
AllowUsers <USER_AMAN>          # Batasi hanya user tertentu yang boleh SSH
```

> ⚠️ **Peringatan Lomba:** Sebelum me-restart layanan SSH (`sudo systemctl restart ssh`), selalu uji validitas sintaks dengan `sudo sshd -t` dan biarkan satu sesi terminal aktif tetap terbuka agar tidak terkunci (_lockout_).

### B. Mitigasi Dangerous Exposed Services

Tutup semua layanan dan port yang tidak dibutuhkan oleh skenario lomba:

```bash
# 1. Cek semua port TCP/UDP yang sedang listening
ss -tulpn

# 2. Cek layanan systemd yang sedang berjalan atau aktif saat boot
systemctl list-units --type=service --state=running
systemctl list-unit-files --type=service --state=enabled

# 3. Matikan dan nonaktifkan layanan yang tidak diperlukan (contoh: telnet, ftp, apache2)
sudo systemctl disable --now <NAMA_SERVICE>

# 4. Simulasi pemindaian dari luar untuk memverifikasi port tertutup
nmap -sV -sC -p- <IP_TARGET>
```

### C. Konfigurasi Firewall Dasar (UFW)

Terapkan prinsip _Default Deny_ untuk koneksi masuk (_incoming_):

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp            # Sesuaikan port layanan yang wajib dibuka (misal: 22, 80, 443)
sudo ufw enable
sudo ufw status numbered
```

---

## ⚡ 4. CLI Quick Reference & Lokasi File Penting

### Lokasi File Konfigurasi Umum

| Komponen                        | Lokasi File / Direktori                                                      |
| :------------------------------ | :--------------------------------------------------------------------------- |
| **Kebijakan Password & PAM**    | `/etc/pam.d/`, `/etc/security/pwquality.conf`, `/etc/security/faillock.conf` |
| **Konfigurasi SSH**             | `/etc/ssh/sshd_config`                                                       |
| **Hak Akses Sudo**              | `/etc/sudoers` dan `/etc/sudoers.d/`                                         |
| **Daftar User & Password Hash** | `/etc/passwd` dan `/etc/shadow`                                              |
| **Log Autentikasi & Sistem**    | `/var/log/auth.log` (Debian/Ubuntu) atau `/var/log/secure` (RHEL/CentOS)     |
| **Jadwal Tugas (Cron Jobs)**    | `/etc/crontab`, `/etc/cron.d/`, dan `crontab -l`                             |

### Perintah Manajemen Harian

```bash
# Manajemen Layanan (Systemd)
sudo systemctl status <SERVICE>       # Cek status layanan
sudo systemctl restart <SERVICE>      # Muat ulang konfigurasi layanan
sudo systemctl disable --now <SERVICE># Matikan layanan sekarang & saat boot

# Pemantauan Log Real-time
sudo tail -f /var/log/auth.log        # Pantau percobaan login & aktivitas sudo
```

---

## 📚 Referensi Standar

- **CIS Benchmarks (Center for Internet Security):** Panduan standar konfigurasi keamanan sistem operasi Linux.
- **MITRE ATT&CK Framework:** [https://attack.mitre.org/](https://attack.mitre.org/) (Referensi pemetaan teknik serangan seperti _Network Service Discovery_ `T1046` & _Brute Force_ `T1110`).
