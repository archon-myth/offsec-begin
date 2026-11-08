# ⚔️ offsec-begin — Master Dashboard

---

## 🎯 Current Mission Status (Phase 0 — Setup Complete: 50%)

> [!TIP]
> **Active Focus:** Phase 0 — Setup & Mindset (Month 1)  
> **Progress:** [█████░░░░░] 50% (Lab Setup Complete · Theory & Legal Framework Pending)  
> **Status:** 🔄 **In Progress** | **Last Updated:** 2026-11-08  
> **Full Tracker Details:** [[phase-tracker|เปิดดูตาราง Phase Tracker แบบละเอียด]]  
>  
> **✅ 1. สภาพแวดล้อม Lab และสถาปัตยกรรมระบบ (พร้อมรบ 100%):**  
> - VirtualBox Kali Linux Golden Baseline (Golden_Base_Clean) & Windows 11 Lab  
> - External SSD Storage Architecture (D:\\Engine, D:\\Security, E:\\Active_Labs) เชื่อมต่อเรียบร้อย  
> - ระบบ Shared Clipboard และ Drag-and-Drop ใช้งานได้ทั้ง 2 ทิศทาง  
> - เอกสารคู่มือระบบปฏิบัติการ: [[phase-0-setup/notes/Setup-VirtualBox|Setup-VirtualBox]] และ [[phase-0-setup/notes/Setup-HybridSSD|Setup-HybridSSD]]  
>  
> **⏳ 2. ทฤษฎีและกรอบปฏิบัติการ (Next Actions — กำลังดำเนินการ):**  
> - เริ่มต้นบันทึกเนื้อหาลงในสมุดเล่มแรก: [[phase-a-foundations/notes/linux-fundamentals|linux-fundamentals]]  
> - ศึกษากรอบ พ.ร.บ. คอมพิวเตอร์ และจริยธรรมควบคู่กับการทดลองบน Lab

---

## 🗂️ Quick Navigators

| File / Folder | Description & Direct Link |
| :--- | :--- |
| **Phase Tracker** | [[phase-tracker|phase-tracker.md]] — ความคืบหน้าระดับ Phase, เปอร์เซ็นต์, และ KPI Gates |
| **Bug Bounty Log** | [[bug-bounty-log|bug-bounty-log.md]] — บันทึกผลการล่าช่องโหว่และ Bug Bounty |
| **All Templates** | [[templates/template-lab-writeup|templates/]] — 7 แม่แบบรายงานและบันทึกแล็บมาตรฐาน (Standard Security Reporting) |

---

## 🗺️ 22-Month Roadmap (Phases 0 → H)

| Phase | Folder | Focus Domain | Key Notes Available |
| :---: | :--- | :--- | :--- |
| **0** | phase-0-setup/ | Setup & Mindset | [[phase-0-setup/notes/Setup-VirtualBox\|Setup-VirtualBox]], [[phase-0-setup/notes/Setup-HybridSSD\|Setup-HybridSSD]] |
| **A** | phase-a-foundations/ | Linux, Networking, Bash | [[phase-a-foundations/notes/linux-fundamentals\|linux-fundamentals]], [[phase-a-foundations/notes/bash-scripting-mastery\|bash-scripting-mastery]], [[phase-a-foundations/notes/linux-internals-and-security\|linux-internals-and-security]], [[phase-a-foundations/notes/networking-packet-mechanics\|networking-packet-mechanics]] |
| **B** | phase-b-scripting/ | Python, Regex, Recon | [[phase-b-scripting/notes/python-for-offsec\|python-for-offsec]], [[phase-b-scripting/notes/regex-mastery\|regex-mastery]], [[phase-b-scripting/notes/recon-automation\|recon-automation]], [[phase-b-scripting/notes/reverse-shells-and-metasploit\|reverse-shells-and-metasploit]], [[phase-b-scripting/notes/password-cracking-hashcat\|password-cracking-hashcat]], [[phase-b-scripting/notes/hardware-badusb-attacks\|hardware-badusb-attacks]] |
| **C** | phase-c-web/ | Web Exploitation & OWASP | [[phase-c-web/notes/owasp-top-10-deep-dive\|owasp-top-10-deep-dive]], [[phase-c-web/notes/portswigger-web-security-academy\|portswigger-web-security-academy]], [[phase-c-web/notes/burp-suite-mastery\|burp-suite-mastery]], [[phase-c-web/notes/bug-bounty-methodology\|bug-bounty-methodology]] |
| **D** | phase-d-exploit/ | Binary Exploitation & BoF | [[phase-d-exploit/notes/x86-x64-assembly-internals\|x86-x64-assembly-internals]], [[phase-d-exploit/notes/buffer-overflow-fundamentals\|buffer-overflow-fundamentals]], [[phase-d-exploit/notes/modern-binary-protections\|modern-binary-protections]] |
| **E** | phase-e-cloud/ | Cloud Offensive Security | [[phase-e-cloud/notes/aws-offensive-security\|aws-offensive-security]], [[phase-e-cloud/notes/azure-and-gcp-offensive-security\|azure-and-gcp-offensive-security]], [[phase-e-cloud/notes/container-and-k8s-security\|container-and-k8s-security]] |
| **F** | phase-f-redteam/ | Red Teaming & Active Directory | [[phase-f-redteam/notes/active-directory-attacks\|active-directory-attacks]], [[phase-f-redteam/notes/c2-and-post-exploitation\|c2-and-post-exploitation]], [[phase-f-redteam/notes/evasion-and-defense-evasion\|evasion-and-defense-evasion]] |
| **G** | phase-g-portfolio/ | Professionalization & OSCP | [[phase-g-portfolio/notes/oscp-methodology-and-notes\|oscp-methodology-and-notes]], [[phase-g-portfolio/notes/professional-pentest-reporting\|professional-pentest-reporting]], [[phase-g-portfolio/notes/portfolio-and-github-showcase\|portfolio-and-github-showcase]] |
| **H** | phase-h-internship/ | Internship Niche Sprint | [[phase-h-internship/notes/pnpt-methodology-and-notes\|pnpt-methodology-and-notes]], [[phase-h-internship/notes/niche-specialization\|niche-specialization]], [[phase-h-internship/notes/interview-readiness-and-scenarios\|interview-readiness-and-scenarios]] |

---

## 📜 Certification Target Milestones

| Target | Certification | Study Guide | Target Window | Core Competency & Milestone |
| :---: | :--- | :--- | :---: | :--- |
| 🥉 | **eJPT** (eLearnSecurity Junior Pentester) | [[certs/ejpt/study-guide\|eJPT Study Guide]] | **Month 8–10** | Junior Pentester Baseline · Practical Network Penetration Testing |
| ☁️ | **AWS SAA** (Solutions Architect Associate) | *(Cloud Architecture)* | **Month 11–12** | Cloud Infrastructure Security & Offensive Cloud Foundations |
| 🥈 | **PNPT** (Practical Network Pentester) | [[certs/pnpt/study-guide\|PNPT Study Guide]] | **Month 13–16** | Practical Network Pentesting · Real-world Enterprise Engagement |
| 🥈 | **CRTO** (Certified Red Team Operator) | [[certs/crto/study-guide\|CRTO Study Guide]] | **Month 19–21** | Active Directory Attacks, C2 Operations & Evasion Tradecraft |
| 🥇 | **OSCP** (OffSec Certified Professional) | [[certs/oscp/study-guide\|OSCP Study Guide]] | **Month 22–24** | Advanced Hands-on Penetration Testing · Industry Gold Standard |

---

## 📝 Lab Writeups Archive

| Date | Phase | Target / Platform | Writeup Link |
| :--- | :---: | :--- | :--- |
| — | — | *ยังไม่มีบันทึก (รอเขียนเมื่อเริ่มเจาะ Lab หรือ CTF เครื่องแรก)* | — |
