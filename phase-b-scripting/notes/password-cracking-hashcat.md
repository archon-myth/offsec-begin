# 🔓 Password Cracking & GPU Acceleration Mastery (Hashcat)

> **Phase:** `Phase B — Scripting & Recon` | **Tags:** `#hashcat` `#passwords` `#cracking` `#gpu` `#wifi`  
> **Status:** ⏳ Upcoming | **Focus:** Hash Identification, Attack Modes, Rule-based Cracking & WPA2/3 PMKID Mode 22000

---

## 📑 Core Modules Overview

- [ ] [[#Module 01: Hash Architecture & GPU Acceleration|Module 01: Hash Architecture & GPU Acceleration]]
- [ ] [[#Module 02: Hashcat Attack Modes & Rules Engine|Module 02: Attack Modes (-a 0, -a 3) & Rules Engine]]
- [ ] [[#Module 03: Modern Wi-Fi PMKID Mode 22000|Module 03: WPA2/3 Mode 22000 & Cloud Cracking]]

---

## Module 01: Hash Architecture & GPU Acceleration
- **Status:** ⏳ Not Started
- **Core Concept:** กลไก One-Way Cryptographic Hash และเหตุผลที่ GPU มีความเร็วในการถอดรหัสสูงกว่า CPU หลายพันเท่า (Massive Parallel SIMD Cores)

### 📝 Common Hashcat Modes
| Hash Type | Hashcat Mode (`-m`) | Example Format |
| :--- | :---: | :--- |
| **MD5** | `-m 0` | `8743b52063cd84097a65d1633f5c74f5` |
| **SHA-256** | `-m 1400` | 64 ตัวอักษร Hexadecimal |
| **NTLM (Windows)** | `-m 1000` | `b4b9b02e6f09a9bd760f388b67351e2b` |
| **SHA-512 Crypt (Linux /etc/shadow)** | `-m 1800` | `$6$saltsalt$hashhash...` |
| **WPA-PBKDF2-PMKID/EAPOL** | `-m 22000` | `WPA*02*...` |

---

## Module 02: Hashcat Attack Modes & Rules Engine
- **Status:** ⏳ Not Started
- **Core Concept:** กลยุทธ์การโจมตีรหัสผ่าน: Dictionary Attack, Mask / Brute-force Attack, และการใช้ Rules Engine เพื่อแปลงคำศัพท์อัตโนมัติ

### 💻 Essential Hashcat Commands
```bash
# Dictionary Attack (-a 0) ยิง NTLM hash ด้วย RockYou wordlist
hashcat -m 1000 -a 0 ntlm_hashes.txt /usr/share/wordlists/rockyou.txt

# Rule-based Attack: ใช้กฎ best64.rule ดัดแปลงคำศัพท์ (เติมตัวเลข, สลับตัวพิมพ์ใหญ่)
hashcat -m 1000 -a 0 ntlm_hashes.txt /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule

# Mask Attack (-a 3): กำหนดรูปแบบตัวอักษร เช่น ตัวพิมพ์ใหญ่ 1 + ตัวพิมพ์เล็ก 4 + ตัวเลข 3 (Password123)
hashcat -m 1000 -a 3 ntlm_hashes.txt "?u?l?l?l?l?d?d?d"

# ดูรหัสผ่านที่แคร็กสำเร็จแล้วทั้งหมด
hashcat -m 1000 --show ntlm_hashes.txt
```

---

## Module 03: Modern Wi-Fi PMKID Mode 22000
- **Status:** ⏳ Not Started
- **Core Concept:** การแคร็ก Wi-Fi ยุคใหม่โดยไม่ต้องรอ Client ต่อ Wi-Fi (ไม่ต้องรอ 4-Way Handshake) ผ่านการดักจับ PMKID จาก Access Point โดยตรง

### 💻 PMKID Capture & Crack Workflow
```bash
# 1. แคปเจอร์ PMKID ด้วย hcxdumptool
sudo hcxdumptool -i wlan0mon -o capture.pcapng --active_beacon --enable_status=1

# 2. แปลงไฟล์ pcapng เป็นฟอร์แมต Hashcat Mode 22000 ด้วย hcxpcapngtool
hcxpcapngtool -o hash.22000 capture.pcapng

# 3. แคร็กรหัสผ่านด้วย Hashcat GPU Mode 22000
hashcat -m 22000 hash.22000 /usr/share/wordlists/rockyou.txt -d 1
```

### 🧠 Feynman Takeaway & Reflection
#### 💡 แก่นความคิดและกลไกสำคัญ (Core Mental Models)

#### ⚠️ กับดักทางเทคนิคและข้อควรระวัง (Gotchas & Pitfalls)
