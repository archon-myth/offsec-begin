# 🐧 Linux Fundamentals & CLI Architecture

> **Playlist:** [Linux for Hackers (FREE Course)](https://www.youtube.com/playlist?list=PLIhvC56v63IJIujb5cyE13oLuyORZpdkL)  
> **Phase:** [phase-a-foundations](../) | **Tags:** `#linux` `#foundations` `#cli` `#linux-foundations`  
> **Master Curriculum:** [[foundations-execution-blueprint#1.1 Linux Mastery — Linux for Hackers (ครบทั้ง 10 ตอน)|Curriculum Checklist]]

---

## 📑 Course Episodes Overview

- [x] [[#EP 1 — Linux for Hackers // EP 1 (FREE Linux course for beginners)|EP 1: ปรัชญา Linux & เริ่มต้น CLI]]
- [ ] [[#EP 2 — The Linux File System Explained|EP 2: โครงสร้าง File Hierarchy]]
- [ ] [[#EP 3 — HELP!! (for when you suck at Linux)|EP 3: การใช้ man, --help และ Troubleshooting]]
- [ ] [[#EP 4 — sudo = POWER!! (Managing Users)|EP 4: Users, Groups & sudoers]]
- [ ] [[#EP 5 — apt, dpkg, git, Python PiP|EP 5: Package Management]]
- [ ] [[#EP 6 — Services & Daemon Hunting|EP 6: ควบคุม Systemd Services]]
- [ ] [[#EP 7 — KILL Linux Processes|EP 7: Process Management & Signals]]
- [ ] [[#EP 8 — 2 Cool (Web) Things to do in Linux|EP 8: Web Server & curl]]
- [ ] [[#EP 9 — Terminal Hacks You Need|EP 9: คีย์ลัด Terminal เร่งความเร็ว]]
- [ ] [[#EP 10 — I KILLED my Linux Computer|EP 10: วิเคราะห์และแก้ปัญหาเมื่อระบบพัง]]

---

## EP 1 — Linux for Hackers // EP 1 (FREE Linux course for beginners)
- **Video Link:** [YouTube — Linux for Hackers // EP 1](https://www.youtube.com/watch?v=VbEx7B_PTOE)
- **Status:** 🔄 กำลังเรียน
- **Core Concept:** ทำไมแฮกเกอร์ถึงใช้ Linux? (Open-source, ควบคุมระดับ Kernel, น้ำหนักเบา, Automate ผ่าน CLI ได้ 100%)

### 📝 Key Notes & Mindset
- Linux ไม่ใช่แค่ OS เดียว แต่คือ **Kernel** ที่ถูกนำมาแจกจ่ายเป็น Distributions (Distros) เช่น Debian, Kali, Ubuntu, Arch
- แฮกเกอร์ใช้ CLI เพราะรวดเร็ว, ใช้ทรัพยากรน้อย, และสามารถส่ง Output จากคำสั่งหนึ่งต่อไปยังอีกคำสั่งหนึ่งผ่าน Pipe (`|`) ได้

### 💻 Essential Commands Learned
```bash
# ตรวจสอบตัวตนผู้ใช้งานปัจจุบัน
whoami

# ตรวจสอบไดเรกทอรีปัจจุบัน (Print Working Directory)
pwd

# ดูรายชื่อไฟล์และโฟลเดอร์
ls -la

# สลับไปยังไดเรกทอรีที่ต้องการ
cd /etc

# ตรวจสอบเวอร์ชันของ Kernel และสถาปัตยกรรมระบบ
uname -a

# ล้างหน้าจอ Terminal (หรือกด Ctrl + L)
clear
```

### 🧠 Feynman Takeaway & Reflection
- *สรุปด้วยคำพูดตัวเอง:* 
- *จุดที่น่าสนใจ / ข้อควรระวัง:* 

---

## EP 2 — The Linux File System Explained
- **Video Link:** [YouTube — Linux File System Explained in 1,233 seconds](https://www.youtube.com/watch?v=A3G-3hp88mo)
- **Status:** ⏳ รอคิว
- **Core Concept:** ผ่าโครงสร้างระบบไฟล์ของ Linux ทุกสิ่งทุกอย่างใน Linux คือ "ไฟล์" (Everything is a file)

### 📝 Directory Hierarchy Reference
| Directory | Purpose / Typical Contents |
| :--- | :--- |
| `/` | Root ของระบบไฟล์ทั้งหมด (จุดสูงสุด) |
| `/bin`, `/usr/bin` | เก็บ Binary คำสั่งมาตรฐานของระบบ (`ls`, `cat`, `grep`) |
| `/sbin`, `/usr/sbin` | เก็บ Binary สำหรับผู้ดูแลระบบ (System Admin binaries) |
| `/etc` | เก็บไฟล์การตั้งค่า (Configuration files) |
| `/home` | พื้นที่จัดเก็บไฟล์ส่วนตัวของแต่ละ User (`/home/username`) |
| `/root` | Home directory ของผู้ใช้สิทธิ์สูงสุด (Root user) |
| `/var` | เก็บข้อมูลที่เปลี่ยนแปลงตลอดเวลา เช่น Log files (`/var/log`) |
| `/tmp` | เก็บไฟล์ชั่วคราว ล้างทิ้งอัตโนมัติเมื่อรีบูต |
| `/mnt`, `/media` | จุดสำหรับ Mount ไดรฟ์ภายนอก หรือ Shared Folders |

### 💻 Essential Commands Learned
```bash
# ตัวอย่างคำสั่งสำรวจระบบไฟล์
cd /
ls -la
tree -L 1 /
```

### 🧠 Feynman Takeaway & Reflection
- *สรุปด้วยคำพูดตัวเอง:* 

---

## EP 3 — HELP!! (for when you suck at Linux)
- **Video Link:** [YouTube — HELP!! // EP 3](https://www.youtube.com/watch?v=Y17KTiJLcyQ)
- **Status:** ⏳ รอคิว
- **Core Concept:** การหาตัวช่วยเมื่อจำคำสั่งไม่ได้ (`man`, `--help`, `whatis`, `apropos`)

### 💻 Commands Learned
```bash
# ดูคู่มือฉบับเต็ม
man ls

# ดูตัวเลือกคำสั่งแบบย่อ
grep --help

# ค้นหาคำสั่งที่เกี่ยวข้องกับ keyword
apropos "firewall"
```

---

## EP 4 — sudo = POWER!! (Managing Users)
- **Video Link:** [YouTube — sudo = POWER!! // EP 4](https://www.youtube.com/watch?v=jwnvKOjmtEA)
- **Status:** ⏳ รอคิว
- **Core Concept:** User, Group, และการให้สิทธิ์ Root ผ่าน `/etc/sudoers`

---

## EP 5 — apt, dpkg, git, Python PiP
- **Video Link:** [YouTube — Package Management // EP 5](https://www.youtube.com/watch?v=vX3krP6JmOY)
- **Status:** ⏳ รอคิว
- **Core Concept:** การติดตั้งและจัดการโปรแกรมด้วย APT, Debian packages, Git clone และ PIP

---

## EP 6 — Services & Daemon Hunting
- **Video Link:** [YouTube — Daemon HUNTING // EP 6](https://www.youtube.com/watch?v=wOWhfNB_r-0)
- **Status:** ⏳ รอคิว
- **Core Concept:** Systemd, การเปิด-ปิด Service (`systemctl start/stop/status/enable`)

---

## EP 7 — KILL Linux Processes
- **Video Link:** [YouTube — KILL Linux processes // EP 7](https://www.youtube.com/watch?v=LfC6pv8VISk)
- **Status:** ⏳ รอคิว
- **Core Concept:** Process IDs (PID), ส่อง Process (`ps aux`, `top`, `htop`), การส่ง Signals (`kill -9`, `killall`)

---

## EP 8 — 2 Cool (Web) Things to do in Linux
- **Video Link:** [YouTube — 2 cool (web) things // EP 8](https://www.youtube.com/watch?v=HSRghjwTTOQ)
- **Status:** ⏳ รอคิว
- **Core Concept:** การเปิด Local Web Server และการทดสอบยิง Request ด้วย `curl` / `wget`

---

## EP 9 — Terminal Hacks You Need
- **Video Link:** [YouTube — Linux Terminal hacks // EP 9](https://www.youtube.com/watch?v=ACM36qtHIEg)
- **Status:** ⏳ รอคิว
- **Core Concept:** คีย์ลัดเร่งความเร็ว (`Ctrl + R`, `Tab`, `Ctrl + A`, `Ctrl + E`, Aliases)

---

## EP 10 — I KILLED my Linux Computer
- **Video Link:** [YouTube — i KILLED my Linux computer // EP 10](https://www.youtube.com/watch?v=ugt3PBeqHIo)
- **Status:** ⏳ รอคิว
- **Core Concept:** จำลองระบบพัง วิเคราะห์ Error logs และวิธีคืนชีพระบบ
