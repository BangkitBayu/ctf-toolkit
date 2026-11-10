# Networking Cheat Sheet

## OSI Model

> **OSI (Open System Interconnection)** adalah sebuah konsep yang dikembangkan oleh International Organization for Standardization (ISO) tentang bagaimana cara data dikirim dan diterima oleh jaringan komputer, tujuannya menciptakan standar global agar perangkat lunak (Software) dan perangkat keras(Hardware) dapat saling berkomunikasi

### 7 OSI Layers

| Layer        | Definition                                                                                                                            | Example                                       |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------- |
| Application  | Antarmuka utama antara aplikasi dan jaringan komputer                                                                                 | HTTP, HTTPS, FTP, SMTP, DNS                   |
| Presentation | Menerjemahkan, mengenkripsi dan mengompres data agar sesuai format                                                                    | SSL, TLS, ASCII, JPEG, MIDI, MPEG             |
| Session      | Membuka, menjaga dan menutup sesi ketika pertukaran data berlangsung                                                                  | NetBIOS, RPC, NFS, SMB                        |
| Transport    | Mengatur cara pengiriman data seperti memecah menjadi paket-paket kecil (segments), memastikan data tidak hilang dan menyusun kembali | TCP, UDP                                      |
| Network      | Menentukan jalur (routing) agar paket data sampai ke tujuan                                                                           | IP (IPv4/IPv6), ICMP, OSPF, Router            |
| Data Link    | Mengatur pengiriman data antar perangkat yang terhubung dalam 1 jaringan menggunakan MAC Address                                      | Ethernet, Wi-Fi (IEEE 802.11), Switch, Bridge |
| Physical     | Mengubah data digital menjadi sinyal fisik seperti listrik, cahaya, gelombang radio                                                   | UTP Cable, Fiber                              |

## Linux Networking & CLI Quick Reference

```bash
# Cek IP Address & Interface
ip a
ifconfig
ip addr

# Diagnosis koneksi dan DNS
ping <IP/DOMAIN> # Tes konektivitas
traceroute <IP/DOMAIN> # Melacak jalur hop data
whois <IP/DOMAIN> # Menampilkan informasi tentang kepemilikan IP/Domain secara detail
dig <domain> @<dns-server-ip> # Menampilkan informasi tentang DNS lebih detail
```

## Nmap CLI Quick Reference

```bash
# SCAN TECHNIQUES:
nmap -sS <IP/DNS> # SYN scan / stealth scan
nmap -sU <IP/DNS> # UDP scan
nmap -sT <IP/DNS> # TCP connect scan

# OS Detection
nmap -O <IP/DNS> # Deteksi OS

# SERVICE/VERSION DETECTION:
nmap -sV <IP/DNS> # Deteksi service version

# OUTPUT
nmap -v <IP/DNS> # verbose
nmap -vv <IP/DNS> # High verbose
nmap -oA <IP/DNS> # Save with 3 major format
nmap -oN <IP/DNS> # normal format
nmap -oG <IP/DNS> # Grep format

# MISC
nmap -A <IP/DNS> # Aggresive mode

# Timing & Performance
nmap -T<1-5> <IP/DNS> # Speed scan
nmap -p- <IP/DNS> # Scan all port
nmap -p <port-range> <IP/DNS> # Scan port with range
```

## References

- Intro to networking - [https://tryhackme.com/module/intro-to-networking](https://tryhackme.com/module/intro-to-networking)
- Nmap book - [https://nmap.org/book/](https://nmap.org/book/)
