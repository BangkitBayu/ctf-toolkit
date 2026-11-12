# Hardening Cheat Sheet

## Linux Hardening

### Required Packages & Tools

Sebelum melakukan hardening, pastikan paket-paket pendukung berikut terinstal di VM Ubuntu:

| Package              | Command Installation                | Description                              |
| :------------------- | :---------------------------------- | :--------------------------------------- |
| **libpam-pwquality** | `sudo apt install libpam-pwquality` | PAM module to check password strength    |
| **libpam-modules**   | `sudo apt install libpam-modules`   | Pluggable Authentication Modules for PAM |
| **openssh-server**   | `sudo apt install openssh-server`   | Package to connect into ssh server       |

```bash
# Cek package apa saja yang terinstall
apt list --installed

# Cek spesifik package yang sudah terinstall
apt list --installed | grep "<PACKAGE_NAME>"
```

---

### Privileged Access Management (PAM)

> **Privileged Access Management (PAM)** adalah teknik mengelola siapa yang berhak memperoleh akses root, dengan cara apa dan terekam dimana. Namun selain itu istilah PAM dilinux juga berarti Pluggable Authentication Module

File konifigurasi utama berada di `/etc/pam.d`

#### PAM Principles

| Prinsip                  | Definisi singkat                                                                   | Implikasi praktis di Linux                                              |
| ------------------------ | ---------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| **Least Privilege**      | Beri hak seminimal mungkin untuk satu tugas.                                       | `sudo` per-perintah, bukan `(ALL) ALL` ; tidak ada UID 0 ganda.         |
| **Separation of Duties** | Eskalasi privilege harus eksplisit, terkontrol, dan terlog.                        | Pakai `sudo` (terlog, granular) menggantikan `su` /login root langsung. |
| **Defense-in-Depth**     | Banyak lapisan: kebijakan password, lockout, pembatasan `su` , polkit, root login. | Satu kontrol jebol tidak langsung memberi root.                         |
| **Auditability**         | Setiap penggunaan privilege harus terekam dan terverifikasi.                       | Log `sudo` , integritas `pam.d` , jejak `auth.log` / `secure` .         |

- **Mengatur kompleksitas password `/etc/security/pwquality.conf`**

| Opsi                                    | Nilai (CIS L1)                   | Arti                                            |
| --------------------------------------- | -------------------------------- | ----------------------------------------------- |
| `minlen`                                | `14`                             | Panjang minimum.                                |
| `minclass`                              | `4`                              | Wajib 4 kelas (huruf besar/kecil/angka/simbol). |
| `dcredit` `ucredit` `lcredit` `ocredit` | `-1` (bila tak pakai `minclass`) | Minimal 1 tiap jenis (kredit negatif = wajib).  |
| `maxrepeat`                             | `3`                              | Maksimal karakter berturut sama.                |
| `difok`                                 | `2`                              | Minimal karakter beda dari password lama.       |
| `enforce_for_root`                      | (set)                            | Berlakukan juga untuk root.                     |

```bash
# Set kebijakan kompleksitas (sumber tunggal, dibaca pam_pwquality)
sudo sed -i 's/^# *minlen.*/minlen = 14/'     /etc/security/pwquality.conf
sudo sed -i 's/^# *minclass.*/minclass = 4/'  /etc/security/pwquality.conf
grep -E '^(minlen|minclass|maxrepeat|difok)' /etc/security/pwquality.conf
```

- **Mengatur lockout `/etc/security/faillock.conf`**

| Opsi                                  | Nilai (CIS L1) | Arti                                                                         |
| ------------------------------------- | -------------- | ---------------------------------------------------------------------------- |
| `deny`                                | `5`            | Kunci setelah 5 gagal.                                                       |
| `unlock_time`                         | `900`          | Buka otomatis setelah 900 detik.                                             |
| `fail_interval`                       | `900`          | Jendela penghitungan gagal.                                                  |
| `even_deny_root` / `root_unlock_time` | hati-hati      | Kunci root juga (risiko lockout total — pikirkan akses fisik/recovery dulu). |

```bash
printf 'deny = 5\nunlock_time = 900\nfail_interval = 900\n' | sudo tee -a /etc/security/faillock.conf

# Lihat & reset hitungan gagal sebuah user (perintah resmi util faillock)
faillock --user alice
sudo faillock --user alice --reset
```

### Dangerous Exposed Services

## CLI Quick Reference

```bash
# Untuk mengecek apakah layanan tertentu aktif, eg ssh
sudo systemctl status <service>

# Untuk menjalankan layanan sementara, eg ssh
sudo systemctl start <service>

# Menjalankan ulang service, eg ssh
sudo systemctl restart <service>

# Menjalankan layanan secara permanen, eg ssh
sudo systemctl enable <service>
```

## References

- Mitre attack - [https://attack.mitre.org/](https://attack.mitre.org/)
