# Hardening Cheat Sheet

## Linux Hardening

### Privileged Access Management (PAM)

> **Privileged Access Management (PAM)** adalah teknik mengelola siapa yang berhak memperoleh akses root, dengan cara apa dan terekam dimana. Namun selain itu istilah PAM dilinux juga berarti Pluggable Authentication Module

File konifigurasi utama berada di `/etc/pam.d`

### PAM Techniques

### CLI Quick Reference

```bash
sudo systemctl status <service> # Untuk mengecek apakah layanan tertentu aktif, eg ssh
sudo systemctl start <service> # Untuk mnejalankan layanan, eg ssh
```

## References

- Mitre attack - [https://attack.mitre.org/](https://attack.mitre.org/)
