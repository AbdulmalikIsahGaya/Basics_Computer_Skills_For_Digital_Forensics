# System Fundamentals & Disk Partition Analysis (CIP-B101 - Lab 1B)

## 📌 Overview

This repository contains the practical evidence logs, command outputs, mathematical partition calculations, and forensic interpretations for **Lab 1B: System Fundamentals and Disk Partition Analysis**.

The lab explores low-level OS architecture and disk structures:
* Host system hardware and OS profiling (`msinfo32`, `systeminfo`, `wmic`).
* Forensic layer mapping (User, Application, OS, Disk, Memory).
* Memory process and thread inspection (`tasklist`).
* Manual LBA disk sector calculations vs. system values.
* Partition table analysis across Windows (GPT) and Linux (MBR).
* Computer boot process sequence analysis (BIOS/UEFI, MBR/GPT).
* File system metadata analysis (NTFS vs. ext4 vs. FAT32).

---

## 👤 Student Information

* **Name:** Abdulmalik Isah Gaya
* **Student ID:** C11/26/FCDF/17171
* **Course:** CIP-B101 Basic Computer Skills For Digital Forensics
* **Environment:** Windows 10 Home (`DESKTOP-BSMART`) & Kali Linux (`kali` VM)
* **Submission Date:** November 4, 2026

---

## 📁 Repository Structure

```text
cip-b101-lab1b/
├── Lab1B_Disk_Partition_Analysis_Report.md  # Detailed forensic report
├── README.md                                 # Project repository documentation
└── screenshots/                              # Command outputs & evidence logs
    ├── windows_systeminfo.png
    ├── windows_wmic_storage.png
    ├── windows_tasklist.png
    ├── windows_diskmgmt.png
    ├── windows_powershell_disk.png
    ├── linux_fdisk_lsblk.png
    ├── linux_parted_df.png
    └── linux_filesystem_blkid.png
```

---

## 🛠️ Execution & Inspection Commands

### 1. Host System Profiling (Windows)
```cmd
:: Launch System Information GUI
msinfo32

:: CLI System Overview
systeminfo

:: Storage Device Hardware Extraction
wmic diskdrive get model,size,serialnumber
wmic logicaldisk get caption,filesystem,size,freespace,volumename
```

### 2. Process Inspection (Windows)
```cmd
:: Display running processes with PIDs and Memory Usage
tasklist
```

### 3. Non-Destructive Partition Table Inspection (Windows PowerShell & CMD)
```powershell
# PowerShell Storage Cmdlets
Get-Disk
Get-Partition
Get-Volume

# DiskPart CLI (Read-Only List)
diskpart
> list disk
> select disk 0
> list partition
> exit
```

### 4. Non-Destructive Partition Table Inspection (Linux)
```bash
# Block Device & Filesystem Inspection
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINT
lsblk -f

# Read-only Disk Partition Table Display
sudo fdisk -l
sudo parted -l

# Filesystem Usage & Mount points
df -Th
```

---

## 🔬 Mathematical Formulas

### Sector to Capacity Conversion:
$$\text{Total Bytes} = \text{Sectors} \times \text{Sector Size (512 bytes)}$$

$$\text{Capacity (GiB)} = \frac{\text{Total Bytes}}{1024 \times 1024 \times 1024} = \frac{\text{Total Bytes}}{1,073,741,824}$$

$$\text{Capacity (GB)} = \frac{\text{Total Bytes}}{1,000,000,000}$$

---

## 📊 Partition Calculation Summary Table

| Device / Volume | File System | Sector Count | Total Calculated Bytes | Binary Size (GiB) | Decimal Size (GB) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Windows Volume `C:`** | NTFS | $497,500,246$ | $254,721,126,400 \text{ B}$ | $237.23 \text{ GiB}$ | $254.72 \text{ GB}$ |
| **Linux `/dev/sda1`** | ext4 | $167,966,702$ | $85,998,951,424 \text{ B}$ | $80.09 \text{ GiB}$ | $85.99 \text{ GB}$ |

---

## 📜 License

This repository is created solely for academic purposes under the CIP-B101 Digital Forensics module.
