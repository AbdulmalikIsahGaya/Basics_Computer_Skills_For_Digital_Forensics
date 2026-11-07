# Lab 1A: Number Systems in Digital Forensics

## Executive Summary

This report documents the foundational principles of number systems and their relevance in digital forensics. Number systems form the basis for how data is represented, processed, and interpreted by computers. In forensic investigations, an understanding of binary, decimal, octal, and hexadecimal systems is essential for analyzing file structures, memory addresses, network traffic, timestamps, and forensic artifacts.

The purpose of this laboratory exercise was to reinforce the relationship among different numeral systems and demonstrate how conversion techniques support technical analysis in digital forensics. This lab provides a structured introduction to data representation and computational logic, which are critical for interpreting evidence in a forensic context.

---

## 1. Introduction

Digital systems operate using a finite set of symbols and positional values. The most common numeral systems used in computing are:

- Decimal (base 10): used by humans
- Binary (base 2): used internally by computers
- Octal (base 8): used in some low-level computing contexts
- Hexadecimal (base 16): widely used in memory addresses, file signatures, and debugging

In digital forensics, the use of hexadecimal notation is especially important because it allows investigators to view raw data in a compact and structured way.

---

## 2. Objectives

The objectives of this lab were to:

1. Explain the concept of number systems and positional notation.
2. Convert values between decimal, binary, octal, and hexadecimal formats.
3. Demonstrate how these systems are used in computing and digital forensics.
4. Interpret the practical importance of number systems in evidence analysis.

---

## 3. Theoretical Background

### 3.1 Decimal System

The decimal system is base 10 and uses digits 0 to 9. Each digit's value depends on its position.

Example:

- 245 = (2 x 10^2) + (4 x 10^1) + (5 x 10^0)
- 245 = 200 + 40 + 5

### 3.2 Binary System

The binary system is base 2 and uses only 0 and 1. It is the native language of computers.

Example:

- 101101 = (1 x 2^5) + (0 x 2^4) + (1 x 2^3) + (1 x 2^2) + (0 x 2^1) + (1 x 2^0)
- 101101 = 32 + 8 + 4 + 1 = 45

### 3.3 Octal System

The octal system is base 8 and uses digits 0 to 7.

Example:

- 37_8 = (3 x 8^1) + (7 x 8^0) = 24 + 7 = 31

### 3.4 Hexadecimal System

The hexadecimal system is base 16 and uses digits 0 to 9 and letters A to F.

Example:

- 3A_16 = (3 x 16^1) + (10 x 16^0) = 48 + 10 = 58

---

## 4. Conversion Examples

### 4.1 Decimal to Binary

Convert 156 to binary:

- 156 / 2 = 78 remainder 0
- 78 / 2 = 39 remainder 0
- 39 / 2 = 19 remainder 1
- 19 / 2 = 9 remainder 1
- 9 / 2 = 4 remainder 1
- 4 / 2 = 2 remainder 0
- 2 / 2 = 1 remainder 0
- 1 / 2 = 0 remainder 1

Reading remainders from last to first:

- 156 = 10011100_2

### 4.2 Binary to Decimal

Convert 10110101_2 to decimal:

- 1 x 128 = 128
- 0 x 64 = 0
- 1 x 32 = 32
- 1 x 16 = 16
- 0 x 8 = 0
- 1 x 4 = 4
- 0 x 2 = 0
- 1 x 1 = 1

Total = 181

Therefore:

- 10110101_2 = 181

### 4.3 Hexadecimal to Decimal

Convert 2F_16 to decimal:

- 2 x 16 = 32
- F = 15
- 32 + 15 = 47

Therefore:

- 2F_16 = 47

### 4.4 Decimal to Hexadecimal

Convert 255 to hexadecimal:

- 255 / 16 = 15 remainder 15 (F)
- 15 / 16 = 0 remainder 15 (F)

Therefore:

- 255 = FF_16

---

## 5. Importance in Digital Forensics

Number systems are not limited to mathematics; they are essential in digital forensic practice. Their relevance can be seen in several areas:

### 5.1 File Signatures

File signatures are often represented in hexadecimal values. Investigators use HEX values to identify file types, such as DOCX, PNG, PDF, or EXE files, without relying only on file extensions.

### 5.2 Memory Forensics

Memory addresses, stack values, and process metadata are frequently represented in hexadecimal form. Analysts often inspect memory dumps using hexadecimal values to locate suspicious code, malicious activities, or hidden data.

### 5.3 Disk and Sector Analysis

Computer storage is organized into sectors and blocks. These areas are often addressed using binary or hexadecimal values, making number systems crucial during disk analysis and evidence reconstruction.

### 5.4 Networking and Logs

Network packet headers and log records use binary and hexadecimal representations. Forensic investigators may evaluate raw packets to trace communication, detect malicious behavior, or verify evidence integrity.

### 5.5 Data Integrity and Hashing

Checksums and hash values are often displayed in hexadecimal. These values help validate whether files or evidence have been modified, altered, or tampered with during analysis.

---

## 6. Practical Laboratory Observations

During this exercise, the following principles were reinforced:

- Binary is the underlying computational representation used by digital devices.
- Hexadecimal is an efficient notation for representing large binary values.
- Conversion skills are necessary for analyzing digital evidence and understanding system-level data.
- A strong understanding of number systems improves interpretation of memory dumps, file signatures, and logs.

---

## 7. Findings and Discussion

This laboratory exercise demonstrated that number systems are a fundamental layer of digital technology. Although they may appear theoretical, they directly support forensic examination by providing a method to interpret digital artifacts. The ability to convert and understand hexadecimal and binary data allows digital forensic analysts to identify evidence patterns, detect anomalies, and communicate findings accurately.

In particular, hexadecimal notation emerged as the most practical system in forensic settings because it is compact, readable, and closely aligned with machine-level representation. It provides an efficient bridge between human interpretation and binary data structures.

---

## 8. Conclusion

Lab 1A on number systems established the importance of numeral conversion and data representation in digital forensics. By understanding decimal, binary, octal, and hexadecimal systems, students and investigators can better engage with the technical structure of computer systems and forensic artifacts.

This knowledge is essential for evidence acquisition, analysis, and reporting. It forms the basis for more advanced forensic tasks, including crash analysis, malware investigation, memory forensics, and digital evidence interpretation.

---

## 9. References

1. Stallings, W. (2018). Computer Organization and Architecture. Pearson.
2. Forouzan, B. A. (2017). Data Communications and Networking. McGraw-Hill.
3. Digital Forensics instructional materials and lecture notes.

---

## 10. Final Remarks

The study of number systems is a foundational component of computer literacy and digital forensic investigation. A strong grasp of these concepts allows professionals to move confidently between human-readable data and machine-readable information, which is essential for evidence analysis and interpretation.

This report serves as a formal record of the laboratory exercise and demonstrates the essential role of number systems in understanding digital evidence.
