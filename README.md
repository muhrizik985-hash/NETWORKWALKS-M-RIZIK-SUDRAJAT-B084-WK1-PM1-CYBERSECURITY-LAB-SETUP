# 🛡️ Networkwalks Cybersecurity Lab Setup - Week 1

Dokumentasi lengkap dan laporan praktikum penyiapan lingkungan lab keamanan siber menggunakan Oracle VirtualBox dan Kali Linux.

---

## 📋 Table of Contents
1. [Environment Details](#-environment-details)
2. [Step-by-Step Configuration Guide](#-step-by-step-configuration-guide)
   - [Step 1: Setup NAT Network](#step-1-setup-nat-network)
   - [Step 2: Configure VM Network Adapter](#step-2-configure-vm-network-adapter)
   - [Step 3: Boot Kali Linux & Static IP Setup](#step-3-boot-kali-linux--static-ip-setup)
   - [Step 4: Troubleshooting IPv4 DAD Timeout Error](#step-4-troubleshooting-ipv4-dad-timeout-error)
   - [Step 5: Setup Shared Folders](#step-5-setup-shared-folders)
   - [Step 6: Take VM Snapshot](#step-6-take-vm-snapshot)
3. [Proof of Work (Screenshots)](#-proof-of-work-screenshots)

---

## 💻 Environment Details
* **Hypervisor:** Oracle VirtualBox 7.x
* **Operating System:** Kali Linux (2026.2)
* **Network Mode:** NAT Network (`NatNetwork`)
* **Subnet:** `10.0.0.0/24`
* **Static IP Address:** `10.0.0.2`
* **Gateway:** `10.0.0.1`
* **DNS:** `8.8.8.8`

## 🌐 Network Topology & Architecture

```text
  +-----------------------------------------------------------------+
  |                       HOST SYSTEM (WINDOWS)                     |
  |  Shared Folder Path: C:\Users\rizik\Downloads                   |
  +-----------------------------------------------------------------+
                                  |
                      [ VirtualBox Hypervisor ]
                                  |
  +-----------------------------------------------------------------+
  |                 VIRTUAL NAT NETWORK (NatNetwork)                |
  |                 Subnet CIDR : 10.0.0.0/24                       |
  |                 Gateway     : 10.0.0.1                          |
  |                 DNS Server  : 8.8.8.8                           |
  +-----------------------------------------------------------------+
                                  |
                                  | eth0 (Promiscuous: Allow All)
                                  v
  +-----------------------------------------------------------------+
  |                    TARGET VM: KALI LINUX                        |
  |                    Host Name : kali                             |
  |                    Static IP : 10.0.0.2/24                      |
  +-----------------------------------------------------------------+
---

## 🛠️ Step-by-Step Configuration Guide

### Step 1: Setup NAT Network
1. Buka VirtualBox, masuk ke menu **File** > **Tools** > **Network Manager** (atau *Preferences* > *Network*).
2. Buat *NAT Network* baru dengan detail berikut:
   * **Name:** `NatNetwork`
   * **IPv4 Prefix:** `10.0.0.0/24`
   * **DHCP Server:** *Enabled*

### Step 2: Configure VM Network Adapter
1. Buka **Settings** pada VM Kali Linux.
2. Masuk ke menu **Network** > **Adapter 1**.
3. Atur konfigurasi:
   * **Attached to:** *NAT Network*
   * **Name:** `NatNetwork`
   * **Promiscuous Mode:** *Allow All*

### Step 3: Boot Kali Linux & Static IP Setup
Nyalakan VM Kali Linux, buka terminal, lalu jalankan perintah berikut untuk mengatur IP Statis menggunakan `nmcli`:
```bash
sudo nmcli connection modify "Wired connection 1" ipv4.addresses 10.0.0.2/24 ipv4.gateway 10.0.0.1 ipv4.dns "8.8.8.8" ipv4.method manual

step 4 :Troubleshooting IPv4 DAD Timeout Error
Kendala umum: Saat mengaktifkan kembali koneksi (nmcli connection up), sering terjadi error IP configuration could not be reserved (no available address, timeout, etc.) akibat Duplicate Address Detection (DAD).Solusi: Matikan timeout DAD dengan perintah berikut sebelum mengaktifkan ulang koneksi:

Bash
# Matikan fitur DAD
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0

# Restart koneksi jaringan
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"

# Uji koneksi (Ping Test)
ping -c 4 10.0.0.1
ping -c 4 google.com
Step 5: Setup Shared Folders
Masuk ke Settings VM > Shared Folders.

Tambahkan folder dari Host Windows (contoh: folder Downloads).

Centang opsi Auto-mount dan Make Permanent.

Step 6: Take VM Snapshot
Di VirtualBox Manager, klik kanan pada VM Kali Linux.

Pilih Snapshots > Take...

Beri nama snapshot: Week 1 Setup Selesai.


Author: M Rizik Sudrajat
