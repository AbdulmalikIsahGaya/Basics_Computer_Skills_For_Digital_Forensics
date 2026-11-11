# Windows Command Line Forensics Basics

## Professional Laboratory Report

**Student:** Abdulmalik Isah Gaya  
**Registration Number:** C11/26/FCDF/17171  
**Course:** CIP-B101 Basic Computer Skills for Digital Forensics  
**Lab:** Lab 1C – Windows Command Line Forensics Basics  
**Hostname:** Kali  
**Date:** November 4, 2026  

---

# Executive Summary

This lab introduced core Windows command-line skills used in digital forensics and incident response. Activities included evidence-folder creation, file-system navigation, file management, system and network information collection, evidence searching, ownership review, process enumeration, and batch-script automation. The practical exercises demonstrated how forensic analysts collect, preserve, organize, and document digital evidence in a structured manner.

---

# Objectives

- Create and organize a forensic evidence workspace.
- Understand absolute and relative paths.
- Manage files and folders using CMD.
- Collect host and network information.
- Search files for evidence indicators.
- Examine file ownership information.
- Enumerate running processes.
- Automate forensic tasks using batch scripts.

---

# Task 0 – Evidence Folder Creation

## Activity
A dedicated workspace named `CIP-B101_Lab1C_ABDULMALIK_ISAH_GAYA` was created inside the Windows file structure. Additional subfolders were created to organize evidence and investigation artifacts.

## Result
A structured forensic workspace was successfully established and verified.

## Screenshot Reference
`screenshots/task0_workspace_creation.png`

---

# Task 1 – Windows Paths and Navigation

## Objective
Practice navigating Windows directories using absolute paths, relative paths, parent directories, and root directories.

## Findings
### Absolute Path
The complete address of a file or folder starting from the root of the drive.

Example:
```text
C:\Users\Public\CIP-B101_Lab1C_ABDULMALIK_ISAH_GAYA
```

### Relative Path
A location specified relative to the current working directory.

Example:
```text
cd Public
```

## Student Explanation
The absolute path is the full address of a file or folder starting from the root drive. The relative path depends on the current location and does not start from the root.

## Screenshot References
- screenshots/task1_navigation_01.png
- screenshots/task1_navigation_02.png
- screenshots/task1_navigation_03.png

---

# Task 2 – Basic File and Folder Creation

## Objective
Create evidence files and understand output redirection.

## Findings
### Single Redirection
```cmd
>
```
Creates a new file or overwrites existing content.

### Double Redirection
```cmd
>>
```
Appends data without deleting existing content.

## Screenshot Reference
`screenshots/task2_file_creation.png`

---

# Task 3 – Copy, Rename and Delete Files

## Objective
Practice safe file handling techniques used in forensic investigations.

## Activities
- Copied files.
- Renamed files.
- Deleted temporary files after verification.
- Confirmed successful operations using directory listings.

## Forensic Relevance
Evidence should always be preserved while analysis is performed on copies.

## Screenshot References
- screenshots/task3_copy.png
- screenshots/task3_rename.png
- screenshots/task3_delete.png

---

# Task 4 – Understanding PATH

## Objective
Understand how Windows locates executable applications.

## Findings
Windows uses the PATH environment variable to determine which executable to launch when only a command name is entered.

### Security Risk
Attackers may abuse PATH through:
- PATH hijacking
- Malicious executable planting

## Screenshot Reference
`screenshots/task4_path.png`

---

# Task 5 – System Information Collection

## Information Gathered
- Hostname
- Operating system details
- Processor information
- BIOS information
- Memory information
- Disk information

## Forensic Importance
System information provides critical context during forensic triage and incident investigations.

## Screenshot Reference
`screenshots/task5_system_information.png`

---

# Task 6 – Network Information Collection

## Information Gathered
- IPv4 address
- Default gateway
- MAC address
- Connectivity test results

## Forensic Importance
Network artifacts assist with device identification, attribution, and incident analysis.

## Screenshot References
- screenshots/task6_ipconfig.png
- screenshots/task6_mac_address.png
- screenshots/task6_ping_test.png

---

# Task 7 – Searching for Evidence Text

## Tool Used
```cmd
findstr
```

## Findings
### /i Switch
Performs case-insensitive searches.

### /n Switch
Displays line numbers alongside matching results.

## Screenshot Reference
`screenshots/task7_findstr.png`

---

# Task 8 – File Ownership Review

## Finding
The file `mini_case_log.txt` was owned by:

```text
DESKTOP-BSMART\BILKISU
```

## Forensic Importance
Ownership information links files to user accounts and helps establish attribution.

## Screenshot Reference
`screenshots/task8_file_owner.png`

---

# Task 9 – Process Enumeration

## Objective
Identify running processes and browser activity.

## Activities
- Listed active processes.
- Identified browser-related processes.
- Verified process information.

## Forensic Importance
Process listings often reveal suspicious applications, malware activity, and persistence mechanisms.

## Screenshot References
- screenshots/task9_process_list.png
- screenshots/task9_browser_process.png
- screenshots/task9_process_verification.png

---

# Task 10 – Batch File Creation and Execution

## Scripts Created

### hello_world.bat
```bat
@echo off
echo Hello World
```

### sys_info.bat
Automated host-information collection script.

## Forensic Importance
Automation improves consistency, repeatability, and efficiency during evidence collection.

## Screenshot References
- screenshots/task10_hello_world.png
- screenshots/task10_sysinfo_batch.png

---

# Task 11 – Making sys_info.bat Accessible from Any Folder

## Activity
The PATH environment variable was temporarily updated to allow execution of the batch script from any directory.

## Screenshot Reference
`screenshots/task11_path_execution.png`

---

# Mini Forensic Interpretation Questions

## Why create a dedicated evidence folder?
To avoid contaminating evidence and maintain organized investigative documentation.

## Commands used to obtain IP and MAC addresses
```cmd
ipconfig
ipconfig /all
```

## Why are batch files useful?
Batch scripts automate repetitive collection tasks and ensure consistent execution.

## Why document failed commands?
Failed commands provide investigative context and can indicate connectivity issues, firewall restrictions, incorrect addresses, or unavailable services.

## Risk of suspicious executables in PATH
Windows executes the first matching file found in PATH, making PATH hijacking a common attack technique.

---

# Conclusion

This lab successfully demonstrated fundamental Windows command-line skills required for digital forensics. The exercises reinforced evidence preservation, documentation, host triage, network investigation, process analysis, and task automation techniques that are commonly applied during forensic examinations and incident-response investigations.
