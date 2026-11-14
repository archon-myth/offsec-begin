# 🛰️ Reconnaissance & Target Footprinting Automation

> **Phase:** `Phase B — Scripting & Recon` | **Tags:** `#recon` `#nmap` `#osint` `#burp` `#hydra`  
> **Status:** ⏳ Upcoming | **Focus:** Network Discovery, Service Enumeration, Web Interception & Brute-force

---

## 📑 Core Modules Overview

- [ ] [[#Module 01: Nmap Scanning Architecture|Module 01: Nmap Scanning Architecture & NSE Scripts]]
- [ ] [[#Module 02: Open Source Intelligence & Google Dorking|Module 02: OSINT & Google Hacking Methodology]]
- [ ] [[#Module 03: Web Pentesting Setup with Burp Suite|Module 03: Burp Suite Interception & HTTP Analysis]]
- [ ] [[#Module 04: Online Password Brute-Forcing with Hydra|Module 04: Hydra Protocol Brute-forcing]]

---

## Module 01: Nmap Scanning Architecture
- **Status:** ⏳ Not Started
- **Core Concept:** การกวาดตรวจหาพอร์ตและช่องโหว่: Host Discovery, Timing Templates, Service Fingerprinting และ Nmap Scripting Engine (NSE)

### 💻 Essential Nmap Command Suite
```bash
# Host Discovery: สแกนแบบไม่แตะ Port เพื่อหาเครื่องที่เปิดอยู่
nmap -sn 192.168.1.0/24

# Full Port Scan: ตรวจสอบพอร์ตทั้งหมด 65,535 พอร์ตด้วยความเร็วระดับ T4
nmap -p- -T4 -v 10.10.10.10

# Service & Version Detection พร้อมรัน Default Safe Scripts
nmap -sV -sC -O -p 21,22,80,443 10.10.10.10

# Vulnerability Scan: สแกนตรวจหา CVE ช่องโหว่ที่เป็นที่รู้จัก
nmap --script=vuln -p 80,445 10.10.10.10

# บันทึกผลลัพธ์ครบทุกรูปแบบ (-oA: .nmap, .gnmap, .xml)
nmap -sV -sC -p- -oA nmap_scan_target 10.10.10.10
```

---

## Module 02: Open Source Intelligence & Google Dorking
- **Status:** ⏳ Not Started
- **Core Concept:** การรวบรวมข้อมูลผ่านเสิร์ชเอนจินเพื่อค้นหาข้อมูลรั่วไหล (Exposed Secrets), ไฟล์ Config, หน้า Admin ที่ลืมปิดสาธารณะ

### 📝 Essential Google Dorks
```text
# ค้นหาไฟล์ Configuration หรือ Database Backups
site:target.com filetype:env | filetype:sql | filetype:bak

# ค้นหาหน้าเข้าสู่ระบบ Admin Portal
site:target.com inurl:admin | inurl:login | inurl:dashboard

# ค้นหาเอกสารภายในหรือข้อมูลลับที่เผลอ公開
site:target.com filetype:pdf "confidential" | "internal use only"
```

---

## Module 03: Web Pentesting Setup with Burp Suite
- **Status:** ⏳ Not Started
- **Core Concept:** การทำ Man-in-the-Middle ระหว่าง Browser กับ Web Application เพื่อตรวจสอบ Request/Response Headers และแก้ไข Parameters

### 📝 Key Workflow
1. ติดตั้ง **FoxyProxy** บน Firefox แล้วชี้ Proxy ไปที่ `127.0.0.1:8080`
2. ดาวน์โหลดและติดตั้ง **Burp CA Certificate** (`http://burp` $ightarrow$ Settings $ightarrow$ Import Certificate)
3. ใช้ **Burp Repeater (`Ctrl + R`)** เพื่อปรับแต่ง Header/Payload ส่งซ้ำทดสอบช่องโหว่

---

## Module 04: Online Password Brute-Forcing with Hydra
- **Status:** ⏳ Not Started
- **Core Concept:** การโจมตีคาดเดารหัสผ่านผ่าน Network Services (SSH, FTP, HTTP POST Form) ด้วย Wordlist

### 💻 Essential Hydra Commands
```bash
# ยิงรหัสผ่าน SSH ด้วย User เฉพาะเจาะจงและ Rockyou wordlist
hydra -l admin -P /usr/share/wordlists/rockyou.txt ssh://10.10.10.10 -t 4

# ยิงรหัสผ่าน FTP
hydra -L users.txt -P passwords.txt ftp://10.10.10.10 -vV

# ยิง Web Login Form (HTTP POST Form)
hydra 10.10.10.10 http-post-form "/login.php:user=^USER^&pass=^PASS^:F=Incorrect" -l admin -P /usr/share/wordlists/rockyou.txt
```

### 🧠 Feynman Takeaway & Reflection
#### 💡 แก่นความคิดและกลไกสำคัญ (Core Mental Models)

#### ⚠️ กับดักทางเทคนิคและข้อควรระวัง (Gotchas & Pitfalls)
