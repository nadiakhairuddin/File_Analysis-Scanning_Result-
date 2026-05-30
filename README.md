# File_Analysis-Scanning_Result-
 
## Vulnerability Analysis Lab - W6 & W7  

---

# 📌 Question 1: Analyse packet1.pcap and find the flag

Packet analysis is performed using Wireshark to inspect captured traffic.

### 📷 Result

<img width="1012" height="469" alt="image" src="https://github.com/user-attachments/assets/60ed8404-0ff3-4a61-bd18-b6291a4a3ad4" />

<img width="1141" height="674" alt="image" src="https://github.com/user-attachments/assets/0358c45d-82b9-41d2-9c33-0ba4fd2ca300" />

### 📌 Observation

The packet is analysed using Wireshark and the flag is successfully extracted from the captured network stream.

---

# 📌 Question 2: Analyse packet2.pcap and find the flag

Packet2 is analysed using Wireshark to inspect network traffic.

### 📷 Result

<img width="1906" height="994" alt="image" src="https://github.com/user-attachments/assets/9b1901e8-63f8-481b-a93d-f75fefec198f" />

### 📌 Observation

The flag is found inside the packet payload after inspecting the captured traffic.

---

# 📌 Question 3: Interpret an Nmap Output

```bash
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 2.3.4
22/tcp   open  ssh         OpenSSH 5.3p1
80/tcp   open  http        Apache 2.2.8
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds Windows 7 Professional 7601 SP1
```

## 📌 1. What can an attacker do with each port?

### 🔹 Port 21 (FTP - vsftpd 2.3.4)
- Upload and download files  
- Perform brute force attacks  
- Exploit vsftpd 2.3.4 backdoor for root access  
- Access files if anonymous login is enabled  

---

### 🔹 Port 22 (SSH - OpenSSH 5.3p1)
- Brute force login credentials  
- Remote command execution after successful login  
- File transfer using SCP / SFTP  
- Maintain persistence via backdoors  

---

### 🔹 Port 80 (HTTP - Apache 2.2.8)
- SQL Injection, XSS, LFI, RFI attacks  
- Directory enumeration  
- Upload webshell for remote access  
- Exploit outdated Apache vulnerabilities  

---

### 🔹 Port 139 (NetBIOS)
- Enumerate users and shared resources  
- Gather system and network information  
- SMB relay attacks  
- Legacy protocol exploitation  

---

### 🔹 Port 445 (SMB - Windows 7 SP1)
- Access shared files and printers  
- Exploit EternalBlue (MS17-010)  
- Dump credentials using tools like Mimikatz  
- Lateral movement in network  
- Deploy ransomware (e.g. WannaCry)  

---

## 📌 2. Likely Vulnerabilities Based on Version

| Service  | Version | Vulnerability |
|----------|--------|--------------|
| vsftpd   | 2.3.4  | Backdoor (CVE-2011-2523) |
| OpenSSH  | 5.3p1  | User enumeration / weak crypto |
| Apache   | 2.2.8  | DoS / outdated modules vulnerabilities |
| Windows  | 7 SP1  | EternalBlue (MS17-010) |

---

## 📌 3. Highest Risk Service

The highest risk is **Port 21 (FTP - vsftpd 2.3.4)**.

This version contains a known backdoor vulnerability that can be triggered using a malicious username input. When exploited, it opens port 6200 and provides root shell access without authentication.

This makes it extremely critical because it allows full system compromise easily and remotely.

---

## 📌 4. Attack Path Scenario

- Perform Nmap reconnaissance  
- Identify FTP vsftpd 2.3.4  
- Exploit backdoor via malicious username input  
- Gain root shell on port 6200  
- Pivot into internal network  
- Target SMB (EternalBlue) on Windows 7  
- Gain SYSTEM access  
- Perform credential dumping and lateral movement  

---

## 📌 5. Remediation

### 🔹 FTP (vsftpd)
- Upgrade vsftpd to latest version  
- Disable anonymous login  
- Restrict access via firewall  
- Use SFTP instead of FTP  

---

### 🔹 SSH
- Upgrade OpenSSH  
- Disable root login  
- Use key-based authentication  
- Enable fail2ban  

---

### 🔹 HTTP (Apache)
- Upgrade Apache  
- Install Web Application Firewall (WAF)  
- Disable directory listing  
- Patch web applications regularly  

---

### 🔹 NetBIOS (139)
- Disable NetBIOS if not needed  
- Block port 139  

---

### 🔹 SMB (445)
- Upgrade Windows OS (Windows 10/11)  
- Patch MS17-010 immediately  
- Disable SMBv1  
- Apply network segmentation  
- Block external access to port 445  

---

## 📌 Question 4: OS Fingerprinting (TTL)

### 📷 Result

<img width="1150" height="405" alt="image" src="https://github.com/user-attachments/assets/2d5880d7-93c2-4bcc-9363-26976b7fb4fb" />

**Answer:** Linux-based system (Unix-like OS)

---

### 📷 Result

<img width="303" height="207" alt="image" src="https://github.com/user-attachments/assets/ebe210e7-35ec-4873-9725-df9b8bbf7ea8" />

**Answer:** Windows OS (TTL indicates initial value around 128)

---

### 📷 Result

<img width="847" height="195" alt="image" src="https://github.com/user-attachments/assets/b8cd51ea-0a56-43ad-8bd5-83393e710eb1" />

**Answer:** Windows OS (default TTL 128 confirmed)

---

## 📌 Question 5: Analyse Nessus File (Ghostcat)

### 📷 Result

<img width="970" height="634" alt="image" src="https://github.com/user-attachments/assets/e8ff2ac5-5334-4215-8e3d-d7133470b239" />

<img width="784" height="336" alt="image" src="https://github.com/user-attachments/assets/92f6ff33-e9ba-43b3-b2db-5f1d37a5b372" />

<img width="342" height="477" alt="image" src="https://github.com/user-attachments/assets/79fae865-dd0b-407d-8c12-f29dab933757" />

<img width="339" height="691" alt="image" src="https://github.com/user-attachments/assets/38afa358-dd19-49bc-a55d-3a62e98207f6" />

---

## 📌 1. Affected Port
- 8009  

## 📌 2. Protocol
- AJP (Apache JServ Protocol)  

## 📌 3. CVSS Score
- 9.8 (Critical)  

## 📌 4. Exploit Availability
- Yes, public exploit is available  

## 📌 5. CVE Reference
- CVE-2020-1938 (Ghostcat)  
- CVE-2020-1745 (related CVE)  

---

## 📌 Conclusion

This lab covers multiple cybersecurity concepts including:

- Packet analysis using Wireshark  
- Network scanning using Nmap  
- OS fingerprinting using TTL  
- Vulnerability analysis (CVE-based)  
- Critical exploitation scenarios (EternalBlue, Ghostcat)  

Proper patch management and secure configuration are essential to reduce system vulnerabilities.
