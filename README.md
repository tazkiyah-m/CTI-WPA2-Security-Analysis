# 🛡️ Threat Intelligence & WPA2-Personal Security Analysis

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Category: Cybersecurity](https://img.shields.io/badge/Category-Cybersecurity-blue.svg)]()
[![Framework: MITRE ATT&CK](https://img.shields.io/badge/Framework-MITRE%20ATT%26CK-red.svg)](https://attack.mitre.org/)

> **Mata Kuliah:** Cyber Threat Intelligence (CTI)  
> **Dosen Pengampu:** [Isi Nama Dosen]  
> **Disusun Oleh:** EKO PRASETYO ADI NUGROHO | **NIM:** 105841114223  
> **Video Demonstrasi YouTube:** [Tempel Tautan Link YouTube Di Sini](https://youtu.be/[link-video-anda])

---

## 📋 Deskripsi Proyek

Proyek ini menganalisis tingkat keamanan protokol **WPA2 Personal (PSK)** melalui teknik penangkapan paket data (*packet capture*) dan analisis *4-Way Handshake*. Proyek ini bertujuan untuk memetakan potensi ancaman (*Threat Intelligence*) yang memanfaatkan kerentanan jaringan nirkabel serta memberikan rekomendasi mitigasinya berbasis kerangka kerja **MITRE ATT&CK**.

---

## 🛠️ Alat dan Lingkungan Pengujian

- **OS Penyerang/Analisis:** Kali Linux
- **Tools Utama:** Wireshark, Aircrack-ng suite (`airmon-ng`, `airodump-ng`), Compact Wireless Driver
- **Target Network:** SSID Uji Coba (`wcut`) — Channel 8
- **Protokol Terlibat:** IEEE 802.11, EAPOL, DHCP, ARP

---

## 🔍 Hasil Analisis Traffic & Handshake

### 1. Probe Request & Discovery
Penangkapan paket broadcast *Probe Request* dari perangkat klien untuk mendeteksi keberadaan SSID target. Metadata yang berhasil diisolasi meliputi Radio Header, SSID Name, dan Format Frame Heksadesimal/ASCII.

### 2. WPA2 4-Way Handshake (EAPOL)
Proses pertukaran kunci berhasil direkam secara rinci:
- **Message 1/4:** AP ➡️ Client (Pengiriman AP Nonce / ANonce)
- **Message 2/4:** Client ➡️ AP (Pengiriman Client Nonce / SNonce + MIC)
- **Message 3/4:** AP ➡️ Client (Konfirmasi PTK & pengiriman GTK terenkripsi)
- **Message 4/4:** Client ➡️ AP (Konfirmasi akhir untuk memulai sesi terenkripsi)

### 📸 Bukti Dokumentasi Visual (Screenshots)

| 1. Pengaktifan Monitor Mode | 2. Notifikasi Handshake Terminal |
| :---: | :---: |
| ![Monitor Mode](screenshots/monitor_mode.png) | ![Handshake Terminal Proof](screenshots/handshake_terminal_proof.png) |

| 3. Alur 4-Way Handshake (Wireshark) | 4. Detail Struktur EAPOL / Key Data |
| :---: | :---: |
| ![Wireshark EAPOL](screenshots/wireshark_eapol.png) | ![EAPOL Details](screenshots/eapol_details.png) |

---

## ⚠️ Threat Intelligence & Mapping Kerentanan

| Threat Vector | MITRE ATT&CK ID | Tingkat Risiko | Deskripsi Singkat |
| :--- | :--- | :--- | :--- |
| **WPA2 Handshake Capture & Offline Cracking** | T1110.001 | 🔴 **High** | Ekstraksi PTK/PMK via *dictionary attack* terhadap file PCAP handshake. |
| **ARP Spoofing / MitM (Man-in-the-Middle)** | T1557.002 | 🟠 **Medium-High** | Pengalihan lalu lintas data lokal akibat tidak adanya otentikasi pada protokol ARP. |
| **KRACK (Key Reinstallation Attack)** | CVE-2017-13077 | 🔴 **High** | Memaksa instalasi ulang kunci nonce saat Message 3/4 untuk dekripsi data. |

---

## 🛡️ Rekomendasi Mitigasi (Intelligence Action)

1. **Password Policy:** Gunakan Pre-Shared Key (PSK) minimal 16-20 karakter kombinasi Alfanumerik + Simbol untuk menggagalkan *offline dictionary attack*.
2. **Patch & Firmware Update:** Rutin memperbarui firmware AP dan sistem operasi klien untuk menutup celah Kerentanan KRACK.
3. **Migrasi Protokol:** Mempertimbangkan migrasi ke **WPA3-Personal** yang menggunakan *Simultaneous Authentication of Equals (SAE)* untuk mencegah serangan *offline dictionary attack*.
4. **Implementasi Wireless IPS (WIPS):** Menggunakan sistem pemantauan untuk mendeteksi *Rogue AP* atau pemindaian mode monitor yang mencurigakan.

---

## 📁 Struktur Repositori

```text
.
├── captures/
│   ├── wpa2_handshake-04.cap         # File Packet Capture Handshake WPA2
│   └── README.md                     # Panduan Penangkapan Paket
├── screenshots/
│   ├── monitor_mode.png              # Bukti pengaktifan mode monitor
│   ├── handshake_terminal_proof.png  # Bukti notifikasi WPA Handshake di terminal
│   ├── wireshark_eapol.png           # Tampilan alur EAPOL 1-4 di Wireshark
│   └── eapol_details.png             # Detail heksadesimal & struktur kunci EAPOL
├── Naskah_Video_Presentasi_YouTube.md # Naskah Presentasi & Panduan Rekaman Video
├── README.md                          # Dokumentasi Utama Repositori
└── references.md                      # Daftar Referensi & CVE
```
