# 🎯 Recon Methodology & Attack Surface Automation

> **Phase:** [phase-b-scripting](../) | **Tags:** `#recon` `#osint` `#subdomains` `#nmap`  
> **Template Companion:** `templates/template-recon-checklist.md`

---

## 📑 Recon Pipeline Checklist
- [ ] 1. Passive Recon: ASN enumeration, Certificate Transparency (`crt.sh`), Whois, DNSDumpster
- [ ] 2. Active Subdomain Enumeration: `sublist3r`, `amass`, `subfinder`
- [ ] 3. Subdomain Permutation & Brute-force: `shuffledns`, `dnsx`
- [ ] 4. Port & Service Discovery: `naabu`, `nmap`
- [ ] 5. Web Technology Fingerprinting: `httpx`, `wappalyzer`
- [ ] 6. Visual Recon: `gowitness`, `aquatone`

---

## 💻 Standard Nmap Scanning Sequences
```bash
# 1. Fast Full-Port Discovery
nmap -p- --min-rate 1000 -T4 -oN scans/all_ports.txt <target>

# 2. Targeted Service & Script Scan
nmap -p <open_ports> -sC -sV -oA scans/detailed_services <target>

# 3. UDP Quick Sweep
nmap -sU --top-ports 50 -oN scans/udp_top.txt <target>
```

---

## 3.1 & 3.4 Active Scanning & Service Enumeration (Nmap & Kali Arsenal)
> **Curriculum Modules:** 3.1 Nmap Scanning & 3.4 Web Arsenal / Password Brute-force

### 💻 Nmap Scanning Playbook
```bash
# ตรวจสอบพอร์ตแบบเร็วพิเศษ
nmap -T4 -F <target_ip>

# ตรวจสอบพอร์ตทั้งหมด 1-65535 พร้อม Service Version และ Default Scripts
nmap -p- -sC -sV -oN scans/nmap_full.txt <target_ip>

# ใช้ Nmap Scripting Engine (NSE) หมวดหาช่องโหว่
nmap -p 80,443 --script vuln <target_ip>
```

### 💻 Web Testing & Brute-force Toolkit
- **Gobuster:** `gobuster dir -u http://<target> -w /usr/share/wordlists/dirb/common.txt`
- **Nikto:** `nikto -h http://<target>`
- **Hydra:** `hydra -l admin -P /mnt/d/Security/Wordlists/rockyou.txt <target_ip> ssh`
