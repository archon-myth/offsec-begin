# 👔 Technical Interview Readiness & Scenario Defense

> **Phase:** `Phase H — Internship Niche` | **Tags:** `#interview` `#career` `#scenarios`  
> **Goal:** มั่นใจ 100% ในทุกคำถามสัมภาษณ์ทางเทคนิคของบริษัท Security ชั้นนำ

---

## 📑 Top 10 Core Technical Scenarios & Ideal Answers

### 1. "อธิบายสิ่งที่เกิดขึ้นตั้งแต่พิมพ์ URL ในเบราว์เซอร์จนหน้าเว็บโหลดขึ้นมา (Packet & Protocol Level)"
- **คำตอบที่คาดหวัง:** DNS Resolution (Cache $\rightarrow$ Recursive Resolver $\rightarrow$ Root $\rightarrow$ TLD $\rightarrow$ Authoritative) $\rightarrow$ TCP 3-Way Handshake (SYN, SYN-ACK, ACK) $\rightarrow$ TLS Handshake (Client Hello, Server Certificate, Key Exchange) $\rightarrow$ HTTP GET Request $\rightarrow$ Server Response (200 OK + HTML/JS/CSS) $\rightarrow$ DOM Rendering

### 2. "หากติด WAF (Web Application Firewall) คุณมีแนวคิดในการวิเคราะห์และ Bypass อย่างไร?"
- **คำตอบที่คาดหวัง:** ตรวจสอบ Signature ของ WAF จาก Response Headers / Block Page $\rightarrow$ ทดสอบ Encoding Techniques (URL Encoding, Double Encoding, Hex Encoding, Unicode) $\rightarrow$ ทดสอบ Chunked Transfer Encoding $\rightarrow$ ปรับแต่ง Parameter Pollution (HPP)

### 3. "อธิบายความแตกต่างระหว่าง XSS กับ CSRF และวิธีป้องกันของแต่ละแบบ"
- **XSS (Cross-Site Scripting):** โค้ด JavaScript อันตรายถูกรันบนเบราว์เซอร์ของเหยื่อ $\rightarrow$ ป้องกันด้วย Context-aware Output Encoding และ Content Security Policy (CSP)
- **CSRF (Cross-Site Request Forgery):** เหยื่อถูกหลอกให้ส่ง Request ที่มีสิทธิ์ของตนไปยัง Server ปลายทาง $\rightarrow$ ป้องกันด้วย Anti-CSRF Token และ Cookie `SameSite=Strict/Lax`

### 4. "หากคุณได้ Initial Foothold บน Windows เครื่องหนึ่งในวง Domain คุณมีขั้นตอนทำ Reconnaissance อย่างไร?"
- **คำตอบที่คาดหวัง:** เช็กตัวตนและสิทธิ์ (`whoami /all`) $\rightarrow$ ตรวจสอบการเชื่อมต่อเครือข่ายและเส้นทาง Routing (`ipconfig /all`, `route print`) $\rightarrow$ สำรวจ Domain Controller (`nltest /dclist:domain`) $\rightarrow$ ตรวจสอบ Antivirus/EDR ที่รันอยู่ $\rightarrow$ สำรวจ Password ใน Memory ด้วย Mimikatz หรือทำ Kerberoasting
