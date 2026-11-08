# 🌐 Networking & Packet Mechanics (TCP/IP & Wire Analysis)

> **Phases:** [phase-a-foundations](../) | **Tags:** `#networking` `#tcpip` `#wireshark` `#subnetting`  
> **Master Curriculum:** [[foundations-execution-blueprint#Phase 2 แก่นแท้ของระบบเครือข่ายและการดักจับข้อมูล Networking Packet Mechanics|Curriculum Checklist]]

---

## 📑 Core Modules Overview

- [ ] [[#Module 1 — Networking Fundamentals & Subnetting|Module 1: Subnetting Subnetting Masterclass]]
- [ ] [[#Module 2 — TCP 3-Way Handshake Deep Dive|Module 2: Wire Mechanics Wire Mechanics (SYN, ACK, FIN)]]
- [ ] [[#Module 3 — ARP Poisoning & Man-in-the-Middle|Module 3: Wire Mechanics arpspoof & Cleartext Sniffing]]
- [ ] [[#Module 4 — Port Scanning Wire Analysis|Module 4: Wire Mechanics Nmap Signatures on Wireshark]]

---

## Module 1 — Networking Fundamentals & Subnetting (Subnetting)
- **Video Link:** [YouTube — what is an IP Address? // EP 1](https://www.youtube.com/watch?v=5WfiTHiU4x8)
- **Status:** ⏳ รอคิว
- **Core Concept:** โครงสร้าง IPv4 (32 bits), Network ID vs Host ID, Subnet Mask (/24, /16, /8), และการคำนวณ Subnetting ด้วยกระดาษเปล่า

---

## Module 2 — TCP 3-Way Handshake Deep Dive (Wire Mechanics)
- **Video Link:** [YouTube — TCP 3-Way Handshake with Wireshark](https://www.youtube.com/watch?v=bW_IPfPmswI)
- **Status:** ⏳ รอคิว
- **Core Concept:** กลไกการเชื่อมต่อระดับ Transport Layer: `SYN` $\rightarrow$ `SYN-ACK` $\rightarrow$ `ACK`, Sequence Number Tracking, Window Size

---

## Module 3 — ARP Poisoning & Man-in-the-Middle (Wire Mechanics)
- **Video Link:** [YouTube — ARP Spoofing / Poisoning Explained](https://www.youtube.com/watch?v=n_4K6qfM_xY)
- **Status:** ⏳ รอคิว
- **Core Concept:** Layer 2 Attack: การส่ง Gratuitous ARP เพื่อปลอม MAC Address บน Switch, การ Forward packet และดักอ่าน Credentials ด้วย Wireshark

---

## Module 4 — Port Scanning Wire Analysis (Wire Mechanics)
- **Video Link:** [YouTube — Nmap Scanning under the hood with Wireshark](https://www.youtube.com/watch?v=HkKGqKk0uL4)
- **Status:** ⏳ รอคิว
- **Core Concept:** ความแตกต่างระดับ Packet ระหว่าง TCP Connect scan (`-sT`), SYN Stealth scan (`-sS`), และ UDP scan (`-sU`)


---

## Module 5 — HTTP Protocol Mechanics & Web Headers (curl & Inspection)
> **Focus:** เข้าใจกลไกเว็บระดับ Protocol Header ก่อนเริ่มโจมตี Web Pentesting

### 📑 Core Topics
- [ ] 1. HTTP Methods (`GET`, `POST`, `PUT`, `DELETE`, `HEAD`, `OPTIONS`, `PATCH`)
- [ ] 2. Request Headers & Response Headers (Host, User-Agent, Cookie, Content-Type)
- [ ] 3. Status Codes (2xx Success, 3xx Redirect, 4xx Client Error, 5xx Server Error)
- [ ] 4. Authentication Mechanisms: Basic, Bearer Token, Session Cookie, JWT
- [ ] 5. State Management & Cookies (`Set-Cookie`, `HttpOnly`, `Secure`, `SameSite`)

### 💻 Essential curl & Inspection Commands
```bash
# ดู Headers ทั้งหมดแบบ Verbose
curl -v https://example.com

# ส่ง Custom Header
curl -H 'X-Forwarded-For: 127.0.0.1' https://example.com

# ส่ง POST Request พร้อม Data
curl -X POST -d 'user=admin&pass=secret' https://example.com/login

# บันทึกและส่ง Cookies
curl -c cookies.txt -b cookies.txt https://example.com
```

### 🧠 Feynman Takeaway & Reflection
- *สรุปด้วยคำพูดตัวเอง:*
