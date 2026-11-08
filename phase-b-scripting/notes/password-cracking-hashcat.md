# 🔑 Password Cracking & Hashcat Mastery

> **Phase:** [phase-b-scripting](../) | **Tags:** `#hashcat` `#passwords` `#cracking` `#wpa2` `#gpu`  

---

## 📑 Core Modules Overview
- [ ] [[#4.1 Hash Recovery — Hashcat Core Mastery|4.1 Hashcat Core Mastery (Modes, Rules, Wordlists)]]
- [ ] [[#4.2 Hash Recovery — WPA2/WPA3 Mode 22000 & Cloud Cracking|4.2 Wi-Fi WPA2/WPA3 PMKID Cracking]]

---

## 4.1 Hash Recovery — Hashcat Core Mastery
- **Status:** ⏳ รอคิว
- **Core Concept:** การระบุประเภทแฮชด้วย `hash-identifier` หรือ `hashid`, สถาปัตยกรรมการแคร็กด้วย GPU

### 💻 Common Hashcat Attack Modes (`-a`)
- `-a 0` = Straight (Wordlist dictionary attack)
- `-a 1` = Combination (นำคำจาก 2 wordlist มาต่อกัน)
- `-a 3` = Mask / Brute-force (กำหนดรูปแบบ เช่น `?u?l?l?d?s`)

```bash
# ตัวอย่างการแคร็ก MD5 (-m 0) ด้วย rockyou.txt
hashcat -m 0 -a 0 hashes.txt /mnt/d/Security/Wordlists/rockyou.txt

# ตัวอย่างการแคร็ก NTLM (-m 1000) ของ Windows
hashcat -m 1000 -a 0 ntlm_hashes.txt /mnt/d/Security/Wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule
```

---

## 4.2 Hash Recovery — WPA2/WPA3 Mode 22000 & Cloud Cracking
- **Status:** ⏳ รอคิว
- **Core Concept:** การดักจับ PMKID จาก Access Point โดยไม่ต้องรอผู้ใช้ล็อกอิน (Clientless Attack) และแคร็กด้วย Hashcat Mode 22000
