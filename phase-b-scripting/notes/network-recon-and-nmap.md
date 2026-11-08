# 🎯 Network Reconnaissance & Nmap Mastery

> **Phase:** [phase-b-scripting](../) | **Tags:** `#recon` `#nmap` `#networkchuck` `#bombal`  
> **Master Curriculum:** [[foundations-execution-blueprint#Phase 3 การรวบรวมข้อมูลและเครื่องมือเจาะระบบแกนหลัก Recon Core Toolchains|Curriculum Checklist]]

---

## 📑 Core Modules Overview
- [ ] [[#3.1 Active Recon — Reconnaissance & Nmap|3.1 Active Recon: Nmap Scanner Deep Dive]]
- [ ] [[#3.4 Service Auditing — Web Testing & Kali Arsenal|3.4 Service Auditing: Kali Linux Arsenal & Web Recon]]

---

## 3.1 Active Recon — Reconnaissance & Nmap
- **Video Link:** [YouTube — you need to learn NMAP right now!!](https://www.youtube.com/watch?v=4t4kBkMsDbY)
- **Status:** ⏳ รอคิว
- **Core Concept:** การส่ง Packet สำรวจพอร์ต (SYN Stealth Scan, TCP Connect, UDP), การตรวจจับ OS และเวอร์ชันบริการ (`-sV`)

### 💻 Key Nmap Scans & Flags
```bash
# ตรวจสอบพอร์ตแบบเร็วพิเศษ
nmap -T4 -F <target_ip>

# ตรวจสอบพอร์ตทั้งหมด 1-65535 พร้อม Service Version และ Default Scripts
nmap -p- -sC -sV -oN nmap_full.txt <target_ip>

# ใช้ Nmap Scripting Engine (NSE) หมวดหาช่องโหว่
nmap -p 80,443 --script vuln <target_ip>
```

---

## 3.4 Service Auditing — Web Testing & Kali Arsenal
- **Video Link:** [YouTube — Top 5 Free Hacking Tools in Kali](https://www.youtube.com/watch?v=e43aWc_L-v4)
- **Status:** ⏳ รอคิว
- **Core Concept:** การเลือกใช้อาวุธพื้นฐานใน Kali: Gobuster (Directory Brute-forcing), Nikto (Web Server scanning), WhatWeb (Technology fingerprinting)
