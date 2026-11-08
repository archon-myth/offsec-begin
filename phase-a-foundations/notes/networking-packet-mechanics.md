# 🌐 Networking & Packet Mechanics (TCP/IP & Wire Analysis)

> **Phase:** `Phase A — Core Foundations` | **Tags:** `#networking` `#tcpip` `#wireshark` `#subnetting`  
> **Status:** ⏳ Upcoming | **Focus:** IP Addressing, Packet Header Analysis, 3-Way Handshake, ARP Poisoning & Port Scanning

---

## 📑 Core Modules Overview

- [ ] [[#Module 01: IPv4 Addressing & Subnetting|Module 01: Subnetting Masterclass & Binary Addressing]]
- [ ] [[#Module 02: Layer 2 vs Layer 3 Mechanics|Module 02: Switches, Routers & Default Gateway]]
- [ ] [[#Module 03: TCP 3-Way Handshake Deep Dive|Module 03: Wire Analysis (SYN, ACK, FIN, Window Size)]]
- [ ] [[#Module 04: ARP Protocol & MITM Analysis|Module 04: ARP Poisoning & Cleartext Traffic Sniffing]]
- [ ] [[#Module 05: Port Scanning Wire Signatures|Module 05: Nmap Scan Signatures on Wireshark (SYN vs Connect)]]

---

## Module 01: IPv4 Addressing & Subnetting
- **Status:** ⏳ Not Started
- **Core Concept:** โครงสร้าง IPv4 (32 bits), Network ID vs Host ID, Subnet Mask (/24, /16, /8), และการคำนวณ Subnetting ด้วยกระดาษเปล่า

---

## Module 02: Layer 2 vs Layer 3 Mechanics
- **Status:** ⏳ Not Started
- **Core Concept:** ความแตกต่างระหว่าง MAC Address Table (Layer 2) และ IP Routing Table (Layer 3)

---

## Module 03: TCP 3-Way Handshake Deep Dive
- **Status:** ⏳ Not Started
- **Core Concept:** กลไกการเชื่อมต่อระดับ Transport Layer: `SYN` $\rightarrow$ `SYN-ACK` $\rightarrow$ `ACK`, Sequence Number Tracking, Window Size

---

## Module 04: ARP Protocol & MITM Analysis
- **Status:** ⏳ Not Started
- **Core Concept:** Layer 2 Attack: การส่ง Gratuitous ARP เพื่อปลอม MAC Address บน Switch, การ Forward packet และดักอ่าน Credentials ด้วย Wireshark

---

## Module 05: Port Scanning Wire Signatures
- **Status:** ⏳ Not Started
- **Core Concept:** แกะรอยลายเซ็นบนสายของ Nmap: ความแตกต่างระหว่าง Stealth SYN Scan (`-sS`) กับ Full Connect Scan (`-sT`) บน Wireshark
