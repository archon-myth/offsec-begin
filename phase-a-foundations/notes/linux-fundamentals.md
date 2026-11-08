# 🐧 Linux Fundamentals & CLI Architecture

> **Phase:** `Phase A — Core Foundations` | **Tags:** `#linux` `#foundations` `#cli` `#sysadmin`  
> **Status:** 🔄 In Progress | **Focus:** Linux Command Line, Filesystem, Permissions & Daemons

---

## 📑 Core Modules Overview

- [ ] [[#Module 01: Linux CLI Philosophy & Navigation|Module 01: ปรัชญา Linux & เริ่มต้น CLI]]
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
- **Status:** ⏳ Not Started
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

# ตรวจสอบเวอร์ชันของ Kernel และสถาปัตยกรรมระบบ
uname -a

# ล้างหน้าจอ Terminal (หรือกด Ctrl + L)
clear
```

### 🧠 Feynman Takeaway & Reflection
- *สรุปด้วยคำพูดตัวเอง:* 
- *จุดที่น่าสนใจ / ข้อควรระวัง:* 

---

## Module 02: Linux Filesystem Hierarchy
- **Status:** ⏳ Not Started
- **Core Concept:** ผ่าโครงสร้างระบบไฟล์ของ Linux ทุกสิ่งทุกอย่างใน Linux คือ 'ไฟล์' (Everything is a file)

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

### 💻 Essential Commands
```bash
cd /
ls -la
tree -L 1 /
```

### 🧠 Feynman Takeaway & Reflection
- *สรุปด้วยคำพูดตัวเอง:* 

---

## Module 03: System Manuals & Troubleshooting
- **Status:** ⏳ Not Started
- **Core Concept:** การหาตัวช่วยเมื่อจำคำสั่งไม่ได้ (`man`, `--help`, `whatis`, `apropos`)

### 💻 Essential Commands
```bash
# ดูคู่มือฉบับเต็ม
man ls

# ดูตัวเลือกคำสั่งแบบย่อ
grep --help

# ค้นหาคำสั่งที่เกี่ยวข้องกับ keyword
apropos "firewall"
```

---

## Module 04: User Privileges & Sudoers Architecture
- **Status:** ⏳ Not Started
- **Core Concept:** User, Group, และการให้สิทธิ์ Root ผ่าน `/etc/sudoers`

---

## Module 05: Package Management & Tool Installations
- **Status:** ⏳ Not Started
- **Core Concept:** การติดตั้งและจัดการโปรแกรมด้วย APT, Debian packages, Git clone และ PIP

---

## Module 06: Systemd Services & Daemon Management
- **Status:** ⏳ Not Started
- **Core Concept:** Systemd, การเปิด-ปิด Service (`systemctl start/stop/status/enable`)

---

## Module 07: Process Monitoring & Signal Handling
- **Status:** ⏳ Not Started
- **Core Concept:** Process IDs (PID), ส่อง Process (`ps aux`, `top`, `htop`), การส่ง Signals (`kill -9`, `killall`)

---

## Module 08: Lightweight Web Services & HTTP Querying
- **Status:** ⏳ Not Started
- **Core Concept:** การเปิด Local Web Server และการทดสอบยิง Request ด้วย `curl` / `wget`

---

## Module 09: Terminal Productive Workflows & Aliases
- **Status:** ⏳ Not Started
- **Core Concept:** คีย์ลัดเร่งความเร็ว (`Ctrl + R`, `Tab`, `Ctrl + A`, `Ctrl + E`, Aliases)

---

## Module 10: System Diagnostic Breakdown & Recovery
- **Status:** ⏳ Not Started
- **Core Concept:** จำลองระบบพัง วิเคราะห์ Error logs และวิธีคืนชีพระบบ
