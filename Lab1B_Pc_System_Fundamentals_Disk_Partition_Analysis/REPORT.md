# Digital Forensics Lab Report: System Fundamentals & Disk Partition Analysis
**Course:** CIP-B101 Basic Computer Skills For Digital Forensics  
**Module:** System & File System Fundamentals (Lab 1B)  
**Author:** Abdulmalik Isah Gaya  
**Student ID:** C11/26/FCDF/17171  
**Environment:** Dual-Host Analysis (Windows 10 Home `DESKTOP-BSMART` & Kali Linux `kali` VM)  
**Date:** November 4, 2026  

---

## 1. Executive Summary
This report details the practical execution of **Lab 1B: System Fundamentals and Disk Partition Analysis**. The primary objective of this laboratory exercise is to systematically map host system architecture, inspect running memory processes, perform mathematical disk sector calculations, evaluate partition schemes (MBR vs. GPT), analyze system boot sequences, and interpret underlying file system structures across Windows and Linux environments. 

All findings are documented using read-only administrative utilities (`msinfo32`, `systeminfo`, `wmic`, `tasklist`, `diskpart`, `fdisk`, `lsblk`, `df`, `parted`) to preserve forensic integrity.

---

## 2. Part A: Host System Specification & Evidence Mapping

### 2.1 Hardware and Operating System Specification

System data was acquired using `msinfo32`, `systeminfo`, and `wmic`:

| Field / Attribute | Windows Host (`DESKTOP-BSMART`) | Linux Host (`kali` VM) |
| :--- | :--- | :--- |
| **Operating System** | Microsoft Windows 10 Home (Build 19045) | Kali Linux (x86_64) |
| **System Manufacturer** | LENOVO (Model: 81TK - IdeaPad C340-14IML) | VMware Virtual Platform |
| **Processor** | Intel(R) Core(TM) i3-10110U CPU @ 2.10GHz | Virtualized Core |
| **BIOS Mode / Firmware** | UEFI (Version: LENOVO CKCN19WW V1.09) | Legacy BIOS / MBR Emulated |
| **Total Physical Memory** | 3,893 MB (~4 GB) | 1,750 MB (~2 GB) |
| **Storage Controller** | NVMe SKHynix_HFS256GD9TNG-L3A0B | VMware Virtual SCSI (`/dev/sda`) |
| **Total Disk Capacity** | 256,052,966,400 bytes (~238.47 GiB) | 86,000,000,000 bytes (~80.09 GiB) |

---

### 2.2 Forensic Layer Evidence Mapping

Digital evidence spans multiple operating layers. A single user action propagates across these layers, creating redundant investigative artifacts:

| Forensic Layer | Example Evidence Types | System Artifact Location | Investigative Significance |
| :--- | :--- | :--- | :--- |
| **User Layer** | User activity, typed commands, search history | Recent files, Shell history (`.bash_history`), Browser profiles | Directly links specific user actions to malicious or unauthorized activity. |
| **Application Layer** | Application logs, cache, document metadata | AppData, Office metadata, sqlite databases | Proves which application executed a specific action or file creation. |
| **OS Layer** | Event logs, registry keys, installed software | `C:\Windows\System32\config` (Registry), `/var/log/` | Establishes user session logins, service persistence, and system execution timelines. |
| **Disk Layer** | Unallocated clusters, deleted files, partitions | Master File Table (MFT), Superblocks, Inodes | Facilitates raw data recovery, file carving, and structural timeline analysis. |
| **Memory / Process Layer** | Volatile RAM, active network sockets, injected DLLs | Memory Dumps, Process Lists, Task Manager | Captures running malware binaries and active C2 connections before power-down. |

---

## 3. Part B: Program, Process, and Thread Investigation

### 3.1 Observed System Processes

Process identification was executed via the Windows Command Prompt using `tasklist`:

```text
Image Name                     PID Session Name        Session#    Mem Usage
========================= ======== ================ =========== ============
System Idle Process              0 Services                   0          8 K
System                           4 Services                   0         80 K
Registry                       100 Services                   0     33,788 K
smss.exe                       544 Services                   0        328 K
services.exe                   888 Services                   0      6,160 K
lsass.exe                      896 Services                   0     19,316 K
svchost.exe                   1020 Services                   0     32,136 K
```

#### Identified Core Operating Processes:
1. **`services.exe` (PID 888):** Windows Service Control Manager, responsible for starting and stopping background services.
2. **`Registry` (PID 100):** Kernel process hive holding active memory-mapped registry data structures.
3. **`smss.exe` (PID 544):** Session Manager Subsystem, responsible for initializing user environments.

### 3.2 Conceptual Distinction: Program, Process, and Thread

* **Program:** A passive set of instructions stored statically on a disk volume (e.g., an executable file `.exe` or ELF binary). It consumes no RAM or CPU cycles while idle.
* **Process:** An active execution instance of a program loaded into volatile RAM. A process possesses an isolated virtual address space, file handles, Process Identifier (PID), and security context.
* **Thread:** The smallest executable unit inside a process. A single process can spawn multiple concurrent threads that share the process's memory space to perform parallel execution tasks.

#### Forensic Relevance of Memory Processes
Investigating volatile processes is crucial because modern malware often executes exclusively in RAM (fileless malware) or injects malicious code into legitimate processes (e.g., `svchost.exe`). Process analysis reveals unencrypted memory buffers, active network sockets, and command-line execution parameters that are lost upon reboot.

---

## 4. Part C: Disk Sector and Capacity Calculations

### 4.1 Theoretical Slide Calculation

Using standard LBA sector sizing ($1 \text{ sector} = 512 \text{ bytes}$):
* **Parameters:** 400 cylinders, 12 heads, 17,000 sectors/track

$$\text{Total Sectors} = 400 \times 12 \times 17,000 = 81,600,000 \text{ sectors}$$

$$\text{Total Bytes} = 81,600,000 \times 512 = 41,779,200,000 \text{ bytes}$$

$$\text{Kilobytes (KB)} = \frac{41,779,200,000}{1024} = 40,800,000 \text{ KB}$$

$$\text{Megabytes (MB)} = \frac{40,800,000}{1024} = 39,843.75 \text{ MB}$$

$$\text{Gigabytes (GiB)} = \frac{39,843.75}{1024} = 38.91 \text{ GiB}$$

---

### 4.2 Practical Partition Size Calculation (`/dev/sda1`)

From Kali Linux read-only disk inspection (`sudo fdisk -l`):
* **Target Partition:** `/dev/sda1`
* **Start Sector:** `2048`
* **End Sector:** `167968749`
* **Sector Count:** $167,968,749 - 2048 + 1 = 167,966,702 \text{ sectors}$
* **Sector Size:** $512 \text{ bytes}$

#### Calculation Steps:
1. **Total Bytes:**
   $$167,966,702 \times 512 = 85,998,951,424 \text{ bytes}$$

2. **Kibibytes (KiB):**
   $$\frac{85,998,951,424}{1024} = 83,983,351 \text{ KiB}$$

3. **Mebibytes (MiB):**
   $$\frac{83,983,351}{1024} = 82,014.99 \text{ MiB}$$

4. **Gibibytes (GiB):**
   $$\frac{82,014.99}{1024} = 80.09 \text{ GiB}$$

5. **Decimal Gigabytes (GB):**
   $$\frac{85,998,951,424}{1,000,000,000} = 85.99 \text{ GB}$$

---

## 5. Part D: Non-Destructive Partition Table Inspection

### 5.1 Windows Inspection (`diskmgmt.msc`, PowerShell, `diskpart`)

#### Disk Structure:
* **Disk 0 Style:** GUID Partition Table (GPT)
* **Total Capacity:** 238.47 GiB / 256.05 GB

```text
Partition ###  Type              Size     Offset
-------------  ----------------  -------  -------
Partition 1    System (EFI)       260 MB  1024 KB
Partition 2    Reserved (MSR)      16 MB   261 MB
Partition 3    Primary (C:)       237 GB   277 MB
Partition 4    Recovery          1000 MB   237 GB
```

---

### 5.2 Linux Inspection (`lsblk`, `fdisk`, `parted`, `df`)

#### Execution Commands:
```bash
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINT
sudo fdisk -l
sudo parted -l
df -Th
```

#### Output Metrics:
* **Disk Identifier:** `0x0d0553f7`
* **Partition Table Type:** MBR (`dos`)
* **Primary Partition:** `/dev/sda1` (Boot Flag: `*`, Start: `2048`, End: `167968749`, File System: `ext4`)

---

## 6. Part E: Boot Process Analysis & Partition Structure Comparison

### 6.1 Computer Boot Process Sequence

```text
[ Power-On ]
     │
     ▼
[ Power-On Self Test (POST) ]
     │
     ▼
[ BIOS / UEFI Firmware Initialization ] ◄── (Reads CMOS Settings)
     │
     ▼
[ Boot Record Lookup (MBR Sector 0 / EFI System Partition) ]
     │
     ▼
[ Boot Loader Execution (GRUB / Windows Boot Manager) ]
     │
     ▼
[ Operating System Kernel Loading (vmlinuz / ntoskrnl.exe) ]
     │
     ▼
[ System Initialization & User Space Control (systemd / Winlogon) ]
```

#### Detailed Stage Breakdown:
1. **Power-On & POST:** Motherboard hardware initializes firmware. The CPU executes POST routines to test hardware (RAM, CPU, storage).
2. **Firmware Stage:** BIOS or UEFI reads CMOS settings to establish hardware parameters and evaluate the boot priority list. *Evidence Note:* The host Windows environment operates under **UEFI**, while the Kali VM operates under **Legacy BIOS**.
3. **Boot Record Lookup:** BIOS locates Sector 0 ($512 \text{ bytes}$) on the target disk for the Master Boot Record (MBR). UEFI locates the GPT header and loads boot configurations from the dedicated **EFI System Partition (ESP)**.
4. **Boot Loader Execution:** The MBR or ESP passes execution to the boot loader (e.g., GRUB for Linux, `bootmgr` for Windows).
5. **Kernel Loading:** The boot loader loads the OS kernel into memory. The kernel configures low-level hardware drivers and hardware abstractions.
6. **OS Initialization:** The kernel invokes the system initializer (`systemd` in Linux or `smss.exe` in Windows) to mount disk partitions and present user interfaces.

---

### 6.2 Structural Comparison: MBR vs. GPT

| Feature Criteria | Master Boot Record (MBR) | GUID Partition Table (GPT) |
| :--- | :--- | :--- |
| **Introduced Era** | 1983 (PC DOS 2.0) | Modern Standard (UEFI Spec) |
| **Max Addressing Capacity** | 2.19 TB ($2^{32} \text{ sectors}$) | 9.4 ZB ($9.4 \times 10^{21} \text{ bytes}$) |
| **Partition Allocation** | Max 4 Primary partitions (or 3 Primary + 1 Extended) | Up to 128 Primary partitions in Windows |
| **Redundancy & Structural Integrity** | Single point of failure (Sector 0 contains table & boot code) | Dual structure: Primary GPT Header at LBA 1 and Backup Header at end of disk |
| **Checksum / Error Detection** | None (Data corruption goes undetected) | CRC32 checksums for partition tables and headers |

---

## 7. Part F: File System Identification and Forensic Implications

### 7.1 File System Comparison Matrix

| File System | Supported OS | Key Forensic Characteristics & Metadata Structures |
| :--- | :--- | :--- |
| **NTFS** | Windows | Master File Table (`$MFT`), Alternate Data Streams (ADS), Transaction Logging (`$LogFile`, `$UsnJrnl`), Granular ACLs. High potential for file recovery. |
| **ext4** | Linux | Inode-based allocation, Extent mapping, Journaling (`jbd2`). Directory entries map filenames to inode numbers. Deleted files clear inode pointers. |
| **FAT32** | Universal / Embedded | File Allocation Table structure, legacy compatibility, 4 GB single-file limitation. Lacks journaling and access logs, making timestamps less reliable. |
| **exFAT** | Flash drives | Optimized for large flash storage. Lacks journaling, but supports files > 4 GB. |

### 7.2 Forensic Value of File System Analysis
Identifying the file system prior to acquiring or analyzing evidence is critical:
* **NTFS Artifacts:** Forensic tools parse the `$MFT` to retrieve creation, access, MFT modification, and read timestamps (MACB). The `$UsnJrnl` tracks volume changes.
* **ext4 Artifacts:** Investigators analyze inodes to recover block pointers, file ownership (UID/GID), access rights, and journal transactions.
* **Tool Compatibility:** Applying an incorrect file system parser will misinterpret timestamps, miscalculate cluster allocation, and fail to recover deleted data.

---

## 8. Part G: Comprehensive Practical Partition Verification Matrix

### 8.1 Partition Measurement Summary

$$\text{Calculated Size (GiB)} = \frac{\text{Sectors} \times 512}{1024^3}$$

```text
+-----------------------------------------------------------------------------------+
|                            PARTITION COMPARISON MATRIX                            |
+-------------------+-----------------------------+---------------------------------+
| Metric / Feature  | Windows Partition (C:)      | Linux Partition (/dev/sda1)     |
+-------------------+-----------------------------+---------------------------------+
| Partition Type    | GPT Primary                 | MBR Primary (ID 83 Linux)       |
| File System       | NTFS                        | ext4                            |
| Sector Count      | 497,500,246 sectors         | 167,966,702 sectors             |
| Calculated Bytes  | 254,721,126,400 bytes       | 85,998,951,424 bytes            |
| Calculated (GiB)  | 237.23 GiB                  | 80.09 GiB                       |
| Calculated (GB)   | 254.72 GB (Decimal)         | 85.99 GB (Decimal)              |
| OS-Reported Size  | 237.23 GB (Disk Management) | 80.1G (lsblk) / 79G (df usable) |
| Size Status       | Exact Match                 | Exact Match                     |
+-------------------+-----------------------------+---------------------------------+
```

---

### 8.2 Explanation of Minor Discrepancies Across Utilities

1. **Base-2 (GiB) vs. Base-10 (GB) Sizing Units:** Storage manufacturers report disk capacity in decimal gigabytes ($1 \text{ GB} = 10^9 \text{ bytes}$). Operating systems often display binary gibibytes ($1 \text{ GiB} = 2^{30} = 1,073,741,824 \text{ bytes}$). This explains why a $256 \text{ GB}$ drive displays as $237.23 \text{ GiB}$ in Windows.
2. **Usable Space vs. Raw Partition Capacity:** The Linux `df -Th` command displays usable space ($79 \text{ GiB}$) rather than raw disk capacity ($80.09 \text{ GiB}$). By default, `ext4` reserves $5\%$ of disk blocks for root system processes and allocates space for the filesystem journal (`jbd2`).
3. **Partition Alignment Offsets:** Modern systems align partition start sectors to $2048$ ($1 \text{ MiB}$ offset) to match 4K disk block structures.

---

## 9. Conclusion
Lab 1B demonstrates the essential relationship between operating system architecture, partition schemes, and digital evidence. Mastering non-destructive analysis tools enables investigators to reconstruct system configurations, verify partition integrity, and select correct parsing techniques during forensic investigations.
