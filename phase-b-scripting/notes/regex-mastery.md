# 🔍 Regular Expressions (Regex) for Pentesting

> **Phase:** [phase-b-scripting](../) | **Tags:** `#regex` `#parsing` `#recon` `#grep`  
> **Objective:** ดึงข้อมูลสำคัญ (IP, Domain, API Keys, Passwords, Hashes) จาก Output ขนาดใหญ่

---

## 📑 Key Regex Patterns for Pentesting
- **IPv4 Address:** `\b(?:\d{1,3}\.){3}\d{1,3}\b`
- **Subdomains:** `([a-zA-Z0-9_-]+\.)*example\.com`
- **Email Addresses:** `[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}`
- **MD5 Hash:** `\b[a-fA-F0-9]{32}\b`
- **SHA-256 Hash:** `\b[a-fA-F0-9]{64}\b`
- **JWT Token:** `eyJ[a-zA-Z0-9_-]+\.eyJ[a-zA-Z0-9_-]+\.[a-zA-Z0-9_-]+`

---

## 💻 CLI Text Wrangling (`grep`, `sed`, `awk`)
```bash
# สกัด IP ทั้งหมดจาก log file
grep -oE '\b([0-9]{1,3}\.){3}[0-9]{1,3}\b' access.log | sort -u

# สกัด API Keys ที่อาจหลุดใน JavaScript
grep -oE '(?i)(api[_-]?key|secret|token)[\"\'\s]*[:=][\"\'\s]*[a-zA-Z0-9_-]{20,}' main.js
```
