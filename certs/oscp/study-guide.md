# 🥇 OSCP (OffSec Certified Professional) Study Guide

> **Target Window:** `Month 22–24` | **Prerequisites:** Phase 0 – Phase H  
> **Exam Format:** 24-Hour Practical Exam + 24-Hour Professional Documentation (70 Points to Pass)

---

## 🎯 Exam Point Distribution Architecture

| Target Machine / Set | Total Points | Passing Requirements |
| :--- | :---: | :--- |
| **Active Directory Set (3 Machines)** | **40 Points** | ต้องเจาะยึด Domain Controller ได้ครบทั้ง 3 เครื่อง (All or Nothing) |
| **Standalone Machine 1** | **20 Points** | 10 pts (Initial Shell) + 10 pts (Root / Admin) |
| **Standalone Machine 2** | **20 Points** | 10 pts (Initial Shell) + 10 pts (Root / Admin) |
| **Standalone Machine 3** | **20 Points** | 10 pts (Initial Shell) + 10 pts (Root / Admin) |
| **Bonus Points (Lab Exercises)** | **10 Points** | ทำโจทย์และส่ง Writeup ตามเกณฑ์ล่วงหน้า |

> **กลยุทธ์ผ่าน:** AD Set (40 pts) + Standalone 1 เครื่อง (20 pts) + Initial Shell 1 เครื่อง (10 pts) = **70 Points ผ่านทันที!**

---

## 🛠️ The OSCP Golden Attack Cycle

```
[1. Deep Recon] ──────► [2. Initial Access] ──────► [3. Local Enum] ──────► [4. PrivEsc to Root]
nmap -p- -sV           Identify Web/Service Vuln   linpeas.sh / winpeas      SUID, Sudo -l, CVE
gobuster / feroxbuster Exploit via PoC / Custom    Internal listening ports   Token Impersonation
```

---

## 📋 Critical Exam Invariants
- **Screenshot Everything:** ถ่ายรูป `whoami` และ `ipconfig`/`ip a` คู่กับ Flag (`proof.txt`, `local.txt`) เสมอ
- **No Automated Exploits:** ห้ามใช้ SQLmap หรือ Metasploit Auto-exploit (อนุญาต Metasploit ให้ใช้ได้เพียง 1 เครื่องตลอดการสอบ)
- **Report Writing is 50% of the Battle:** มีเวลา 24 ชั่วโมงหลังจบแล็บเพื่อเขียนรายงานระดับ Enterprise Pentest Report ตาม Template
