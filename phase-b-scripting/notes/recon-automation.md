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
