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

## CLI Quick Reference

```bash
sudo systemctl status <service> # Untuk mengecek apakah layanan tertentu aktif, eg ssh
sudo systemctl start <service> # Untuk mnejalankan layanan, eg ssh
```

## References

- Mitre attack - [https://attack.mitre.org/](https://attack.mitre.org/)
