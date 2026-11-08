# 🐍 Python for Offensive Security & Automation

> **Phase:** [phase-b-scripting](../) | **Tags:** `#python` `#scripting` `#sockets` `#requests` `#pwntools`  
> **Objective:** เขียน Toolchain อัตโนมัติช่วยเจาะระบบและสร้าง Exploit Wrappers เองได้

---

## 📑 Core Modules
- [ ] 1. Socket Programming (TCP/UDP client, Port banner grabber)
- [ ] 2. HTTP Automation with `requests` (Brute-force, Session handling, Header manipulation)
- [ ] 3. Web Scraping & HTML Parsing with `BeautifulSoup`
- [ ] 4. Packet Crafting with `scapy` (ARP spoofing, SYN scanner)
- [ ] 5. Binary Exploit Automation with `pwntools`

---

## 💻 Boilerplate: Basic Port Scanner (Sockets)
```python
import socket

def scan_port(ip, port):
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.settimeout(1.0)
    try:
        s.connect((ip, port))
        print(f"[+] Port {port} is OPEN")
        s.close()
    except:
        pass
```

---

## 🧠 Feynman Takeaway & Projects
- *Script 1:* Target Scope Ping & Port Checker
- *Script 2:* HTTP Directory Brute-Forcer
