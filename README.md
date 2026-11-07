# Digital Forensics Fundamentals Lab (CIP-B101 - Lab 1A)

[![Operating System](https://img.shields.io/badge/OS-Kali%20Linux-blue?logo=kalilinux)](https://www.kali.org/)
[![Language](https://img.shields.io/badge/Language-Python%203.13-yellow?logo=python)](https://www.python.org/)
[![Shell](https://img.shields.io/badge/Shell-Bash-green?logo=gnubash)](https://www.gnu.org/software/bash/)

## 📌 Overview
This repository contains the practical implementations, command logs, and forensic artifacts for **Lab 1A: Digital Forensics Fundamentals** under the course **CIP-B101 Basic Computer Skills For Digital Forensics**. 

The lab focuses on lower-level data analysis concepts necessary for forensic evidence processing:
- Manual & programmatic number conversions (Binary, Decimal, Hexadecimal).
- Binary-to-text encodings (ASCII, UTF-8, raw hex inspection).
- System timestamps and epoch conversions (Unix Epoch vs. Mac Absolute Time).
- Cryptographic hash verification & byte sensitivity (MD5, SHA-256).
- Memory byte ordering and architecture analysis (Little-Endian vs. Big-Endian).

---

## 👤 Student Information
- **Name:** Abdulmalik Isah Gaya
- **Student ID:** C11/26/FCDF/17171
- **Course:** CIP-B101 Basic Computer Skills For Digital Forensics
- **VM Hostname:** Kali Linux (`kali`)
- **Submission Date:** November 4, 2026

---

## 📁 Repository & Directory Structure

```text
cip-b101-lab1a/
├── Abdulmalik_lab1a.txt              # Custom text evidence artifact
├── abdulmalik_with_newline.txt       # Hashing test artifact (with trailing \n)
├── abdulmalik_without_newline.txt    # Hashing test artifact (without trailing \n)
├── evidence_text.txt                 # Initial string inspection file
├── hello_with_newline.txt            # Baseline comparison artifact (with \n)
├── hello_without_newline.txt         # Baseline comparison artifact (without \n)
├── Lab1A_Digital_Forensics_Report.md # Detailed formal lab report
└── README.md                         # Project documentation and quick execution guide
```

---

## 🛠️ Quick Command Reference

### 1. Number Conversions

```bash
# Binary to Decimal
echo $((2#101010))

# Hexadecimal to Decimal
echo $((16#AB2))

# Decimal to Binary
echo "obase=2;126" | bc

# Decimal to Hexadecimal
printf "%X\n" 2738
```

### 2. Hex Inspection & ASCII Extraction

```bash
# Create text without trailing newline
echo -n "Digital Forensics Begins With Bytes." > Abdulmalik_lab1a.txt

# Display formatted hex dump
xxd Abdulmalik_lab1a.txt

# Extract plain hex string
xxd -p Abdulmalik_lab1a.txt

# Reverse plain hex back to ASCII string
xxd -p Abdulmalik_lab1a.txt | xxd -r -p
```

### 3. Timestamp Conversions

```bash
# Standard Unix Timestamp to UTC
date -u -d @1675008832

# Mac Absolute Time to UTC (Python)
python3 -c "from datetime import datetime, timezone; print(datetime.fromtimestamp(725846400 + 978307200, tz=timezone.utc))"
```

### 4. Hash Generation & Comparison

```bash
# Generate MD5 Digests
md5sum abdulmalik_with_newline.txt abdulmalik_without_newline.txt

# Generate SHA-256 Digests
sha256sum abdulmalik_with_newline.txt abdulmalik_without_newline.txt
```

### 5. Endianness Parsing (Python)

```python
import sys
# Check host system architecture
print(sys.byteorder)

# Interpret raw bytes
raw_bytes = bytes.fromhex('78563412')
print("Little-Endian:", int.from_bytes(raw_bytes, 'little'))
print("Big-Endian   :", int.from_bytes(raw_bytes, 'big'))
```

---

## 🔬 Evidence Interpretation Summary

| Evidence ID | Raw Hex / Input | Result / Decoded Value | Description |
| :--- | :--- | :--- | :--- |
| **EVID-001** | `48656c6c6f20464f52454e53494353` | `"Hello FORENSICS"` | ASCII Hex String |
| **EVID-002** | `01001000 01101001` | `"Hi"` | Binary ASCII Representation |
| **EVID-003** | `1675008832` | `2023-01-29 16:13:52 UTC` | 32-bit Unix Timestamp |
| **EVID-004** | `725846400` | `2024-01-02 00:00:00 UTC` | Mac Epoch ($+978307200\text{s}$) |
| **EVID-005** | `78563412` | **LE:** `305419896` / **BE:** `2018915346` | Endianness Byte Evaluation |

---

## 📜 License
This repository is submitted strictly for academic purposes under the CIP-B101 Digital Forensics module.
