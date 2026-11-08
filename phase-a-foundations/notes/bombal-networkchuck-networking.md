# 🌐 Networking & Packet Mechanics Notes (Bombal & NetworkChuck)

> **Phases:** [phase-a-foundations](../) | **Tags:** `#networking` `#tcpip` `#wireshark` `#subnetting`  
> **Master Curriculum:** [[curriculum-bombal-networkchuck#Phase 2 แก่นแท้ของระบบเครือข่ายและการดักจับข้อมูล Networking Packet Mechanics|Curriculum Checklist]]

---

## 📑 Core Modules Overview

- [ ] [[#Module 1 — Networking Fundamentals & Subnetting|Module 1: NetworkChuck Subnetting Masterclass]]
- [ ] [[#Module 2 — TCP 3-Way Handshake Deep Dive|Module 2: David Bombal Wire Mechanics (SYN, ACK, FIN)]]
- [ ] [[#Module 3 — ARP Poisoning & Man-in-the-Middle|Module 3: David Bombal arpspoof & Cleartext Sniffing]]
- [ ] [[#Module 4 — Port Scanning Wire Analysis|Module 4: David Bombal Nmap Signatures on Wireshark]]

---

## Module 1 — Networking Fundamentals & Subnetting (NetworkChuck)
- **Video Link:** [YouTube — what is an IP Address? // EP 1](https://www.youtube.com/watch?v=5WfiTHiU4x8)
- **Status:** ⏳ รอคิว
- **Core Concept:** โครงสร้าง IPv4 (32 bits), Network ID vs Host ID, Subnet Mask (/24, /16, /8), และการคำนวณ Subnetting ด้วยกระดาษเปล่า

---

## Module 2 — TCP 3-Way Handshake Deep Dive (David Bombal)
- **Video Link:** [YouTube — TCP 3-Way Handshake with Wireshark](https://www.youtube.com/watch?v=bW_IPfPmswI)
- **Status:** ⏳ รอคิว
- **Core Concept:** กลไกการเชื่อมต่อระดับ Transport Layer: `SYN` $\rightarrow$ `SYN-ACK` $\rightarrow$ `ACK`, Sequence Number Tracking, Window Size

---

## Module 3 — ARP Poisoning & Man-in-the-Middle (David Bombal)
- **Video Link:** [YouTube — ARP Spoofing / Poisoning Explained](https://www.youtube.com/watch?v=n_4K6qfM_xY)
- **Status:** ⏳ รอคิว
- **Core Concept:** Layer 2 Attack: การส่ง Gratuitous ARP เพื่อปลอม MAC Address บน Switch, การ Forward packet และดักอ่าน Credentials ด้วย Wireshark

---

## Module 4 — Port Scanning Wire Analysis (David Bombal)
- **Video Link:** [YouTube — Nmap Scanning under the hood with Wireshark](https://www.youtube.com/watch?v=HkKGqKk0uL4)
- **Status:** ⏳ รอคิว
- **Core Concept:** ความแตกต่างระดับ Packet ระหว่าง TCP Connect scan (`-sT`), SYN Stealth scan (`-sS`), และ UDP scan (`-sU`)
