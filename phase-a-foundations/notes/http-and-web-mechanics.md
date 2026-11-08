# 🌐 HTTP Mechanics, Headers & Web Protocols

> **Phase:** [phase-a-foundations](../) | **Tags:** `#http` `#networking` `#web` `#curl`  
> **Target:** เข้าใจกลไกเว็บระดับ Protocol Header ก่อนเริ่มโจมตี Web Pentesting

---

## 📑 Core Topics
- [ ] 1. HTTP Methods (`GET`, `POST`, `PUT`, `DELETE`, `HEAD`, `OPTIONS`, `PATCH`)
- [ ] 2. Request Headers & Response Headers (Host, User-Agent, Cookie, Content-Type)
- [ ] 3. Status Codes (2xx Success, 3xx Redirect, 4xx Client Error, 5xx Server Error)
- [ ] 4. Authentication Mechanisms: Basic, Bearer Token, Session Cookie, JWT
- [ ] 5. State Management & Cookies (`Set-Cookie`, `HttpOnly`, `Secure`, `SameSite`)

---

## 💻 Essential curl & Inspection Commands
```bash
# ดู Headers ทั้งหมดแบบ Verbose
curl -v https://example.com

# ส่ง Custom Header
curl -H 'X-Forwarded-For: 127.0.0.1' https://example.com

# ส่ง POST Request พร้อม Data
curl -X POST -d 'user=admin&pass=secret' https://example.com/login

# บันทึกและส่ง Cookies
curl -c cookies.txt -b cookies.txt https://example.com
```

---

## 🧠 Feynman Takeaway & Reflection
- *สรุปด้วยคำพูดตัวเอง:*
