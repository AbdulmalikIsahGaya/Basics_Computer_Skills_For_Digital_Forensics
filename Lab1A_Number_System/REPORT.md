# Digital Forensics Fundamentals: Number Systems, Encodings, Timestamps, and Endianness
**Course:** CIP-B101 Basic Computer Skills For Digital Forensics  
**Module:** Advanced Web Application Security Labs  
**Author:** Abdulmalik Isah Gaya  
**Student ID:** C11/26/FCDF/17171  
**Environment:** Kali Linux (`kali` VM)  
**Date:** November 4, 2026  

---

## 1. Executive Summary
This laboratory report details the core concepts of digital representation essential for digital forensics. The exercise covered manual and programmatic conversions across binary, decimal, and hexadecimal number systems, file character encodings (ASCII/UTF-8), cryptographic hash integrity verification under minor byte changes, temporal analysis (Unix vs. Mac Absolute Time epochs), and system byte ordering (endianness). All theoretical calculations were validated using native Linux utilities (`bash`, `bc`, `xxd`, `md5sum`, `sha256sum`) and Python 3.

---

## 2. Environment & Workspace Setup
A dedicated student working directory was created on the Kali Linux environment to maintain forensic evidence isolation.

```bash
mkdir -p ~/cip-b101-lab1a
cd ~/cip-b101-lab1a
```

---

## 3. Task A: Number System Conversions & Validation

### 3.1 Manual Conversions

#### 1. Binary to Decimal: $101010_2$
$$\begin{aligned}
101010_2 &= (1 \times 2^5) + (0 \times 2^4) + (1 \times 2^3) + (0 \times 2^2) + (1 \times 2^1) + (0 \times 2^0) \\
&= 32 + 0 + 8 + 0 + 2 + 0 \\
&= 42_{10}
\end{aligned}$$

#### 2. Hexadecimal to Decimal: $\text{AB2}_{16}$
$$\begin{aligned}
\text{AB2}_{16} &= (10 \times 16^2) + (11 \times 16^1) + (2 \times 16^0) \\
&= (10 \times 256) + (11 \times 16) + (2 \times 1) \\
&= 2560 + 176 + 2 \\
&= 2738_{10}
\end{aligned}$$

#### 3. Decimal to Binary by Successive Division: $126_{10}$
$$\begin{aligned}
126 \div 2 &= 63 \quad \text{Remainder } 0 \\
63 \div 2 &= 31 \quad \text{Remainder } 1 \\
31 \div 2 &= 15 \quad \text{Remainder } 1 \\
15 \div 2 &= 7 \quad \text{Remainder } 1 \\
7 \div 2 &= 3 \quad \text{Remainder } 1 \\
3 \div 2 &= 1 \quad \text{Remainder } 1 \\
1 \div 2 &= 0 \quad \text{Remainder } 1 \\
126_{10} &= 1111110_2
\end{aligned}$$

#### 4. Decimal to Hexadecimal by Successive Division: $2738_{10}$
$$\begin{aligned}
2738 \div 16 &= 171 \quad \text{Remainder } 2 \\
171 \div 16 &= 10 \quad \text{Remainder } 11 \ (\text{B}) \\
10 \div 16 &= 0 \quad \text{Remainder } 10 \ (\text{A}) \\
2738_{10} &= \text{AB2}_{16}
\end{aligned}$$

#### 5. Binary to Hexadecimal via Bit Grouping: $101010110010_2$
$$\begin{aligned}
1010_2 &= 10_{10} = \text{A}_{16} \\
1011_2 &= 11_{10} = \text{B}_{16} \\
0010_2 &= 2_{10} = 2_{16} \\
101010110010_2 &= \text{AB2}_{16}
\end{aligned}$$

---

### 3.2 Verification via Shell and Python

#### Linux Shell Commands
```bash
# Binary/Hex to Decimal
echo $((2#101010))       # Output: 42
echo $((16#AB2))          # Output: 2738

# Decimal to Binary (bc)
echo "obase=2;126" | bc   # Output: 1111110

# Decimal to Hexadecimal (printf)
printf "%X\n" 2738       # Output: AB2
```

#### Python 3 Verification Script
```python
python3 - << 'PY'
print(int("101010", 2))   # 42
print(int("AB2", 16))     # 2738
print(bin(126))           # 0b1111110
print(hex(2738))          # 0xab2
PY
```

---

## 4. Task B: ASCII, UTF-8, and Hexadecimal Strings

### 4.1 Byte Inspection and Hex Dump Manipulation
Created test string artifacts and analyzed them with `xxd`:

```bash
# Create artifact without trailing newline
echo -n "Hello" > evidence_text.txt
cat evidence_text.txt
# Output: Hello

# View Hex Dump
xxd evidence_text.txt
# Output: 00000000: 4865 6c6c 6f  Hello

# Extract Plain Hexadecimal Representation
xxd -p evidence_text.txt
# Output: 48656c6c6f
```

Reverse transformation from raw hex back to ASCII string:

```bash
printf "48656c6c6f" | xxd -r -p
# Output: Hello
```

Python programmatic verification:
```python
python3 - << 'PY'
import binascii
text = b'Hello'
print('ASCII bytes:', text)
print('ASCII to hex:', binascii.hexlify(text).decode())
print('Hex to ASCII:', binascii.unhexlify('48656c6c6f').decode())
print('ord(H):', ord('H'))
print('chr(72):', chr(72))
PY
```

### 4.2 Forensic Artifact String Creation
Created personal evidence artifact `Abdulmalik_lab1a.txt`:

```bash
echo -n "Digital Forensics Begins With Bytes." > Abdulmalik_lab1a.txt
xxd Abdulmalik_lab1a.txt
```

**Hex Dump Output:**
```text
00000000: 4469 6769 7461 6c20 466f 7265 6e73 6963  Digital Forensic
00000010: 7320 4265 6769 6e73 2057 6974 6820 4279  s Begins With By
00000020: 7465 732e                               tes.
```

Plain hex reverse string test:
```bash
xxd -p Abdulmalik_lab1a.txt | xxd -r -p
# Output: Digital Forensics Begins With Bytes.
```

*Note on Newline (`0x0A`):* Omitting the `-n` flag in `echo` appends a line feed character (`\n`, hex `0x0A`) to the end of the file, which increases file size by 1 byte and alters hashing output.

---

## 5. Task C: Timestamp Analysis and Epoch Conversion

Digital investigation requires normalizing timestamps from different operating system epochs to Universal Coordinated Time (UTC).

### 5.1 Unix Timestamp Conversion (Epoch: 1970-01-01 00:00:00 UTC)

```bash
# Current System Epoch Timestamp
date +%s

# Convert specific Unix timestamp (1643643491) to UTC
date -u -d @1643643491
# Output: Mon Jan 31 03:38:11 PM UTC 2022
```

### 5.2 Local Time vs. UTC Comparison

```python
python3 - << 'PY'
from datetime import datetime, timezone
print('Local:', datetime.now())
print('UTC  :', datetime.now(timezone.utc))
PY
```
*Observation:* A difference of 1 hour was observed between local virtual machine time and UTC, highlighting the necessity of standardizing all digital evidence timelines to UTC.

### 5.3 Mac Absolute Time Conversion (Epoch: 2001-01-01 00:00:00 UTC)

Apple Operating Systems use an epoch starting on `2001-01-01 00:00:00 UTC`. The static offset between the Unix epoch ($1970$) and Mac epoch ($2001$) is **$978,307,200$ seconds**.

```python
python3 - << 'PY'
from datetime import datetime, timezone

unix_epoch = datetime(1970, 1, 1, tzinfo=timezone.utc)
mac_epoch  = datetime(2001, 1, 1, tzinfo=timezone.utc)
offset     = int((mac_epoch - unix_epoch).total_seconds())

print("Epoch Offset (seconds):", offset) # 978307200

# Convert sample Mac Absolute Time (725846400) to UTC
mac_value  = 725846400
unix_value = mac_value + offset
print("Converted UTC Date    :", datetime.fromtimestamp(unix_value, tz=timezone.utc))
PY
```
**Result:** `2024-01-02 00:00:00+00:00`

---

## 6. Task D: Cryptographic Hashing and the Newline Problem

Cryptographic hash functions are extremely sensitive to input alterations. Even a single non-printable byte change completely changes the resulting digest (avalanche effect).

### 6.1 Demonstration Test

```bash
echo "Hello" > hello_with_newline.txt
echo -n "Hello" > hello_without_newline.txt

xxd hello_with_newline.txt
# Hex: 4865 6c6c 6f0a  (Includes 0a)

xxd hello_without_newline.txt
# Hex: 4865 6c6c 6f    (No 0a)
```

**Hash Verification Results:**

| File Name | MD5 Hash Digest | SHA-256 Hash Digest |
| :--- | :--- | :--- |
| `hello_with_newline.txt` | `09f7e02f1290be211da707a266f153b3` | `66a045b452102c59d840ec097d59d9467e13a3f34f6494e539ffd32c1bb35f18` |
| `hello_without_newline.txt` | `8b1a9953c4611296a827abf8c47804d7` | `185f8db32271fe25f561a6fc938b2e264306ec304eda518007d1764826381969` |

### 6.2 Forensic Student Exercise (`Abdulmalik` Test)

```bash
echo "Hello " > abdulmalik_with_newline.txt
echo -n "Hello " > abdulmalik_without_newline.txt

md5sum abdulmalik_with_newline.txt abdulmalik_without_newline.txt
sha256sum abdulmalik_with_newline.txt abdulmalik_without_newline.txt
```

**Results Table:**

| File Artifact | MD5 Digest | SHA-256 Digest |
| :--- | :--- | :--- |
| `abdulmalik_with_newline.txt` | `adb3f07f896745a101145fc3c1c7b2ea` | `a4cfc596d52c95b51ab28b4def04f83da35cd3a4c6db1b6001d4d0fa41809221` |
| `abdulmalik_without_newline.txt` | `d1a7fb5eab1c16cb4f7cf341cf188c3d` | `2ec5a3f0c2fc3e6dcee0f6f3a5735a6c69d2056579a5452095b75802094043a8` |

---

## 7. Task E: System Byte Order (Endianness)

Data storage order determines how multi-byte numbers are reconstructed in memory.
* **Little-Endian:** Least Significant Byte (LSB) stored at the lowest memory address (common in x86/x64 architecture).
* **Big-Endian:** Most Significant Byte (MSB) stored at the lowest memory address (common in network protocols).

```python
python3 - << 'PY'
import sys
print('System byte order:', sys.byteorder) # Outputs: little

raw = bytes.fromhex('78563412')
print('Raw bytes                 :', raw.hex())
print('Interpreted Little-Endian :', int.from_bytes(raw, 'little'))
print('Interpreted Big-Endian    :', int.from_bytes(raw, 'big'))
PY
```

### Endian Conversion Exercise Matrix

| Raw Hex Bytes | Little-Endian Decimal | Big-Endian Decimal | Python Method Used | Explanation |
| :--- | :--- | :--- | :--- | :--- |
| `01000000` | $1$ | $16,777,216$ | `int.from_bytes(raw, 'little'/'big')` | Little-endian evaluates `0x01` as LSB ($1$). Big-endian evaluates `0x01` as MSB ($1 \times 256^3$). |
| `FF000000` | $255$ | $4,278,190,080$ | `int.from_bytes(raw, 'little'/'big')` | Little-endian evaluates `0xFF` as $255$. Big-endian treats `0xFF` as MSB ($255 \times 256^3$). |
| `00000100` | $65,536$ | $256$ | `int.from_bytes(raw, 'little'/'big')` | Shifting byte position changes value based on endianness weighting. |
| `78563412` | $305,419,896$ | $2,018,915,346$ | `int.from_bytes(raw, 'little'/'big')` | Little-endian interprets byte order as `0x12345678`; Big-endian interprets byte order as `0x78563412`. |

---

## 8. Task F: Mini Forensic Interpretation Case

### Evidence Item Summary Table

| Evidence ID | Raw Hex / Binary Value | Decoded / Interpreted Value | Category | Verification Method |
| :--- | :--- | :--- | :--- | :--- |
| **EVID-001** | `48656c6c6f20464f52454e53494353` | `"Hello FORENSICS"` | Content | `bytes.fromhex().decode()` / `xxd -r -p` |
| **EVID-002** | `01001000 01101001` | `"Hi"` | Content | `chr(int('01001000', 2)) + chr(int('01101001', 2))` |
| **EVID-003** | `1675008832` | `2023-01-29 16:13:52 UTC` | Timeline | `datetime.fromtimestamp(1675008832, tz=timezone.utc)` |
| **EVID-004** | `725846400` *(Mac Absolute Time)* | `2024-01-02 00:00:00 UTC` | Timeline | `datetime.fromtimestamp(725846400 + 978307200, tz=timezone.utc)` |
| **EVID-005** | `78563412` *(Raw Hex Bytes)* | **LE:** `305419896`<br>**BE:** `2018915346` | Data Structure | `int.from_bytes(bytes.fromhex('78563412'), 'little'/'big')` |

### Required Case Narrative
1. **EVID-001 (ASCII Text Content):** The raw hexadecimal string `48656c6c6f20464f52454e53494353` represents printable ASCII/UTF-8 character bytes. Decoding yields the plain text `"Hello FORENSICS"`. This artifact constitutes text communication content.
2. **EVID-002 (Binary Text Content):** The binary sequences `01001000` ($72_{10} = \text{'H'}$) and `01101001` ($105_{10} = \text{'i'}$) evaluate directly to the ASCII string `"Hi"`.
3. **EVID-003 (Unix Timeline Timestamp):** The numeric value `1675008832` represents a standard 32-bit Unix epoch timestamp. Calculating seconds elapsed since `1970-01-01 00:00:00 UTC` resolves to `2023-01-29 16:13:52 UTC`.
4. **EVID-004 (Mac OS Epoch Timestamp):** The integer `725846400` represents a Mac Absolute Time timestamp. Accounting for the epoch difference ($2001$ vs $1970$) by adding the offset of $978,307,200$ seconds yields a total Unix timestamp of $1,704,153,600$, corresponding to `2024-01-02 00:00:00 UTC`. Failing to apply the offset would misinterpret the event timestamp by 31 years.
5. **EVID-005 (Byte-Order Integrity):** The sequence `78563412` demonstrates the critical nature of hardware architecture context. If parsed on a little-endian host system (e.g., x86_64), it evaluates to $305,419,896$ (`0x12345678`). On a big-endian network architecture, it evaluates to $2,018,915,346$ (`0x78563412`).

---

## 9. Conclusion
This lab demonstrates that raw data cannot be properly analyzed without knowing its context (data type, encoding scheme, timestamp epoch, and byte architecture). Misinterpreting an epoch or byte order leads to erroneous evidence analysis in digital forensic investigations.
