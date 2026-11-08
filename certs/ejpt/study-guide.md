# 🥉 eJPT (eLearnSecurity Junior Penetration Tester) Study Guide

> **Target Window:** `Month 8–10` | **Prerequisites:** Phase 0 – Phase D  
> **Exam Format:** 48-Hour Hands-on Practical Exam (Dynamic Multi-Network Lab, 35 Questions)

---

## 🎯 Exam Syllabus Breakdown

| Domain | Weight | Core Competency | Key Tools |
| :--- | :---: | :--- | :--- |
| **Assessment Methodologies** | 25% | Information gathering, OSINT, Port Scanning | Nmap, Netcat, Gobuster |
| **Host & Network Auditing** | 25% | Service fingerprinting, Banner grabbing, SMB Enumeration | Enum4linux, SMBClient, Nmap NSE |
| **Host & Network Penetration** | 35% | Exploitation, Metasploit, Brute-forcing, Hash cracking | Metasploit, Hydra, Hashcat, John |
| **Web Application Penetration** | 15% | SQLi, XSS, Directory Brute-force, Authentication bypass | Burp Suite, SQLMap, Nikto |

---

## 🛠️ Essential Command Cheatsheet for eJPT

```bash
# 1. Host Discovery & Full Port Scan
nmap -sn 172.16.1.0/24
nmap -p- -T4 -v 172.16.1.10

# 2. SMB Share Enumeration
smbclient -L //172.16.1.10/ -N
enum4linux -a 172.16.1.10

# 3. Web Directory Fuzzing
gobuster dir -u http://172.16.1.10/ -w /usr/share/wordlists/dirb/common.txt

# 4. Routing & Network Pivoting (เมื่อยึดเครื่อง Dual-Homed ได้)
# ฝั่ง Kali: เพิ่มเส้นทางเข้าสู่ Subnet ภายในผ่าน Meterpreter
route add 172.16.2.0 255.255.255.0 <session_id>
```

---

## 📝 48-Hour Exam Strategy & Mindset
1. **Pivoting is Key:** ข้อสอบ eJPT ไม่ได้อยู่บน Subnet เดียว คุณต้องหาวิธี Pivot จาก Network ขาแรกข้ามไปเจาะเครื่องใน Network วงที่สอง
2. **Take Systematic Notes:** ทำตาม Methodology เสมอ บันทึกทุก IP, ทุก Port, และ Credential ทุกตัวลงตาราง
3. **No Fluff:** คำถามเป็นแบบข้อสอบช้อยส์อิงผลลัพธ์จริงบนเครื่อง (เช่น "User password ของเครื่อง Database คืออะไร?")
