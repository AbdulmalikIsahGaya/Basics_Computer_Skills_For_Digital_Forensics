# Linux Command Line Forensics Basics

## Professional Laboratory Report

**Student:** Abdulmalik Isah Gaya  
**Registration Number:** C11/26/FCDF/17171  
**Course:** CIP-B101 Basic Computer Skills for Digital Forensics  
**Lab Title:** Linux Command Line for Digital Forensics Basics (Lab 2A)  
**Institution:** International Cybersecurity and Digital Forensics Academy (ICDFA)  
**Hostname:** Kali  
**Date:** November 11, 2026

---

# Executive Summary

This lab focused on foundational Linux command-line skills required for digital forensics investigations. Activities included evidence workspace preparation, filesystem analysis, inode examination, path navigation, file management, evidence searching with grep, network inspection, shell scripting, PATH configuration, and evidence archiving.

# Introduction

Linux is widely used in digital forensics because it provides powerful command-line tools for evidence acquisition, analysis, automation, incident response, and system administration. Understanding Linux commands enables investigators to efficiently collect and examine digital evidence.

# Lab Environment

- Operating System: Kali Linux
- Username: kali
- Terminal: Linux Shell
- Tools Used: tree, grep, stat, df, find, ip, ping, ss, nmap, zip, tar
- Virtual Machine Hostname: Kali

---

# Task 1 – Evidence Workspace Preparation

A structured workspace was created containing:

```text
notes/
evidence/
scripts/
outputs/
screenshots/
archive/
```

Screenshot:
`screenshots/task1_workspace_creation.png`

---

# Task 2 – Linux Filesystem and OS Information

Commands Used:

```bash
pwd
ls /
ls -la /
cat /etc/os-release
uname -a
whoami
id
```

## Filesystem Explanation

- `/home` – User directories and personal files.
- `/etc` – System configuration files.
- `/proc` – Virtual filesystem containing process information.
- `/sys` – Hardware and kernel information.
- `/dev` – Device files.
- `/bin` – Essential command binaries.
- `/tmp` – Temporary storage.

Screenshots:
- `screenshots/task2_linux_filesystem_01.png`
- `screenshots/task2_linux_filesystem_02.png`

---

# Task 3 – Inodes and Disk Information

Commands Used:

```bash
ls -li evidence/lab2a_evidence.txt
stat evidence/lab2a_evidence.txt
df -h
df -i
```

## What is an Inode?

An inode is a filesystem data structure that stores metadata about a file including permissions, ownership, timestamps, and disk locations. It does not store the filename itself.

## Forensic Importance

Inodes help investigators trace deleted, renamed, hidden, or recovered files and establish evidence continuity.

Screenshots:
- `screenshots/task3_inode_stat.png`
- `screenshots/task3_df_output.png`

---

# Task 4 – Absolute and Relative Paths

Absolute Path:

```text
/home/kali/CIP-B101_Lab2A_ABDULMALIK_ISAH_GAYA/evidence/lab2a_evidence.txt
```

Relative Path:

```text
../evidence/lab2a_evidence.txt
```

Screenshot:
`screenshots/task4_paths.png`

---

# Task 5 – File Management Operations

Activities Performed:

- Created files.
- Viewed file contents.
- Copied files.
- Renamed files.
- Removed temporary files.

Forensic Relevance:

Evidence handling requires maintaining original files while performing analysis on working copies.

Screenshots:
- `screenshots/task5_file_management_01.png`
- `screenshots/task5_file_management_02.png`

---

# Task 6 – Evidence Searching with grep

Commands Used:

```bash
grep "login" evidence/activity_log.txt
grep -n "failed" evidence/activity_log.txt
grep -Ei "delete|password" evidence/activity_log.txt
grep -Ei "192\.168\.56\.[0-9]+" evidence/activity_log.txt
```

## Findings

Failed login evidence:

```bash
grep -n "failed" evidence/activity_log.txt
```

Suspicious activity search:

```bash
grep -Ei "delete|password" evidence/activity_log.txt
```

Screenshots:
- `screenshots/task6_grep_01.png`
- `screenshots/task6_grep_02.png`

---

# Task 7 – File Discovery and Directory Tree

Commands:

```bash
tree
find . -type f -name "*.txt"
find . -type f -iname "*log*"
find . -type d -name "evidence"
```

Screenshots:
- `screenshots/task7_tree.png`
- `screenshots/task7_find.png`

---

# Task 8 – Network Information Analysis

Information Collected:

- IPv4 Loopback: 127.0.0.1
- IPv6 Loopback: ::1
- Hostname: localhost

Commands Used:

```bash
ip addr
ip route
ping -c 4 127.0.0.1
ping -c 4 localhost
ss -lnt
nmap localhost
```

## Why Localhost Testing is Safer

Localhost testing remains within the investigator's own system, avoids legal issues, requires no external authorization, and reduces operational risk.

Screenshots:
- `screenshots/task8_network_01.png`
- `screenshots/task8_network_02.png`
- `screenshots/task8_network_03.png`

---

# Task 9 – Shell Script Creation

Script Created:

```bash
lab2a_sys_summary.sh
```

Purpose:

Generate automated forensic system summaries containing system, user, and filesystem information.

Screenshots:
- `screenshots/task9_script_creation.png`
- `screenshots/task9_script_output.png`

---

# Task 10 – PATH Environment Variable

The script directory was temporarily added to PATH allowing execution from another folder.

## What PATH Does

PATH tells Linux where to locate executable commands. This allows commands such as `ls`, `grep`, and custom scripts to run without specifying a full path.

Screenshot:
`screenshots/task10_path_execution.png`

---

# Task 11 – Evidence Compression

Archives Created:

```text
CIP-B101_Lab2A_ABDULMALIK_ISAH_GAYA.zip
CIP-B101_Lab2A_ABDULMALIK_ISAH_GAYA.tar.gz
```

## zip vs tar.gz

- zip combines compression and archiving.
- tar.gz creates an archive first and then compresses it using gzip.

Screenshot:
`screenshots/task11_archive.png`

---

# Challenges Encountered

No significant issues were encountered. Linux commands were executed successfully and outputs were verified through screenshots and command validation.

# Forensic Interpretation

- Inodes provide critical metadata for file investigations.
- PATH allows trusted command execution from any location.
- grep assists with evidence discovery and keyword searching.
- Localhost testing supports safe and authorized network investigations.

# Conclusion

The Linux Command Line Forensics Basics lab successfully demonstrated essential skills required for future digital forensics and incident response work. The exercises strengthened knowledge of Linux filesystems, metadata analysis, evidence management, content searching, scripting, network analysis, and evidence preservation.

