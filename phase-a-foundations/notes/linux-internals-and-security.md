# 🛠️ Linux Kernel Internals & Security Mechanisms

> **Phase:** `Phase A — Core Foundations` | **Tags:** `#linux` `#internals` `#kernel` `#security`  
> **Status:** ⏳ Upcoming | **Focus:** File Permissions, SUID/SGID, Text Manipulation, LKM & Bootloader

---

## 📑 Core Modules Overview

### หมวดที่ 1: System Internals & Permissions
- [ ] [[#Module 01: Security Philosophy & Architecture|Module 01: ปรัชญาและโครงสร้างความปลอดภัยของ Linux]]
- [ ] [[#Module 02: Inodes & Filesystem Structure|Module 02: Inodes, Hardlinks vs Symlinks]]
- [ ] [[#Module 03: Network Interfaces & Configuration|Module 03: Network Interfaces, Routing Table & DNS]]
- [ ] [[#Module 04: Source Compilation & Toolchain Build|Module 04: การคอมไพล์เครื่องมือจาก Source Code]]
- [ ] [[#Module 05: Kernel Modules & Environment Tuning|Module 05: Loadable Kernel Modules (LKM) & Environment]]
- [ ] [[#Module 06: Special Permissions & Text Processing|Module 06: SUID/SGID, Sticky Bit & awk, sed, grep]]

### หมวดที่ 2: System Boot & History Auditing
- [ ] [[#Module 07: Bootloader & Partitioning Mechanics|Module 07: โครงสร้าง Bootloader & MBR/GPT Partitioning]]
- [ ] [[#Module 08: Shell History Audit & Forensic Control|Module 08: Shell History Management & Reverse Search]]

---

## Module 01: Security Philosophy & Architecture
- **Status:** ⏳ Not Started
- **Core Concept:** ปรัชญาการออกแบบความปลอดภัยของ Linux: Discretionary Access Control (DAC), Principle of Least Privilege และการแบ่ง Ring 0 (Kernel) vs Ring 3 (User)

### 📝 Key Notes & Mindset
- ทุกสิ่งทุกอย่างมี Owner (User, Group, Others)
- Root (UID 0) มีอำนาจข้ามผ่านข้อกำหนด DAC ทุกประการ เป้าหมายของ Red Teamer คือการไต่ระดับสิทธิ์จาก User ทั่วไปขึ้นไปสู่ Root

---

## Module 02: Inodes & Filesystem Structure
- **Status:** ⏳ Not Started
- **Core Concept:** Inodes คือ Data structure ที่เก็บ Metadata ของไฟล์ (Permissions, Owner, Size, Pointer to data blocks) แต่ไม่เก็บชื่อไฟล์!

### 💻 Essential Commands
```bash
# ดู Inode Number ของแต่ละไฟล์
ls -li

# ดู Inode usage ของฮาร์ดดิสก์ (กรณีไฟล์เต็มแม้ขนาดดิสก์ยังเหลือ)
df -i

# เปรียบเทียบ Hard link vs Symbolic Link
echo "Secret Data" > original.txt
ln original.txt hardlink.txt        # ชี้ Inode เดียวกัน ลบไฟล์เดิม ข้อมูลยังอยู่
ln -s original.txt symlink.txt      # ชี้ที่ชื่อไฟล์ ลบไฟล์เดิม ลิงก์พัง (Dangling link)

# ค้นหาไฟล์ตามหมายเลข Inode
find / -inum 131072 2>/dev/null
```

---

## Module 03: Network Interfaces & Configuration
- **Status:** ⏳ Not Started
- **Core Concept:** การตั้งค่าและสำรวจเครือข่ายระดับ Low-level ผ่านคำสั่ง `ip` ยุคใหม่ (ทดแทน `ifconfig` ที่ล้าสมัย)

### 💻 Essential Commands
```bash
# ดู IP Address และ MAC Address ของทุก Interface
ip -c addr

# ดู Routing Table (Gateway หลัก)
ip route show

# ตรวจสอบ Sockets ที่เปิดรับการเชื่อมต่อ (Listening Ports) พร้อม Process ID
ss -tulpn

# ตรวจสอบ DNS Resolver ปัจจุบัน
cat /etc/resolv.conf
resolvectl status
```

---

## Module 04: Source Compilation & Toolchain Build
- **Status:** ⏳ Not Started
- **Core Concept:** การคอมไพล์โค้ดต้นฉบับ C/C++ เป็น Binary Executable สำหรับเครื่องมือที่ไม่มีใน APT Repository

### 💻 Essential Commands
```bash
# ขั้นตอนมาตรฐานในการคอมไพล์ซอฟต์แวร์
tar -xzvf exploit_tool.tar.gz
cd exploit_tool
./configure
make
sudo make install

# คอมไพล์ไฟล์ C ตัวเดียวด้วย GCC
gcc -Wall -O2 exploit.c -o exploit
```

---

## Module 05: Kernel Modules & Environment Tuning
- **Status:** ⏳ Not Started
- **Core Concept:** Loadable Kernel Modules (LKM) — ไดรเวอร์และโมดูลที่โหลดเข้าสู่ Kernel Space แบบสด ๆ โดยไม่ต้องรีสตาร์ทเครื่อง

### 💻 Essential Commands
```bash
# ตรวจสอบโมดูล Kernel ที่กำลังโหลดทำงานอยู่ทั้งหมด
lsmod

# ดูข้อมูลรายละเอียดของโมดูลเฉพาะเจาะจง
modinfo overlay

# โหลดโมดูลเข้า Kernel หรือถอดโมดูลออก
sudo modprobe <module_name>
sudo rmmod <module_name>
```

---

## Module 06: Special Permissions & Text Processing
- **Status:** ⏳ Not Started
- **Core Concept:** พิชิต SUID (`4000`), SGID (`2000`), Sticky Bit (`1000`) และคำสั่งจัดการข้อความระดับเทพ (`awk`, `sed`, `grep`, `cut`, `sort`, `uniq`)

### 📝 Special Permissions Matrix
| Bit | Name | Octal | Effect on Execution | Privilege Escalation Value |
| :---: | :--- | :---: | :--- | :--- |
| **SUID** | Set User ID | `4000` | รันโปรแกรมด้วยสิทธิ์ของเจ้าของไฟล์ (เช่น root) | ⭐⭐⭐⭐⭐ ล่าสุดยอดช่องโหว่ PrivEsc |
| **SGID** | Set Group ID | `2000` | รันโปรแกรมด้วยสิทธิ์ของกลุ่มเจ้าของไฟล์ | ⭐⭐⭐ ยึดสิทธิ์ระดับกลุ่ม |
| **Sticky**| Sticky Bit | `1000` | ลบไฟล์ได้เฉพาะเจ้าของเท่านั้น (เช่น `/tmp`) | ⭐ ป้องกันการแอบลบไฟล์คนอื่น |

### 💻 SUID Hunt & Text Manipulation Commands
```bash
# [คำสั่งระดับตำนาน] ค้นหา SUID Binaries ที่ Root เป็นเจ้าของทั้งหมดในเครื่อง
find / -perm -u=s -type f 2>/dev/null

# Text Parsing: กรองเฉพาะ Username และ Shell จาก /etc/passwd
cut -d: -f1,7 /etc/passwd | grep "/bin/bash"

# AWK: คัดเลือกเฉพาะคอลัมน์ IP และ Port จากผลลัพธ์
ss -ant | awk '{print $4, $5}'

# SED: ค้นหาและแทนที่ข้อความในไฟล์ทันที
sed -i 's/DEBUG=True/DEBUG=False/g' config.env
```

---

## Module 07: Bootloader & Partitioning Mechanics
- **Status:** ⏳ Not Started
- **Core Concept:** กระบวนการ Boot ของ Linux: BIOS/UEFI $\rightarrow$ GRUB2 $\rightarrow$ Kernel (vmlinuz) $\rightarrow$ Initramfs $\rightarrow$ Systemd

### 💻 Essential Commands
```bash
# ดูรายการพาร์ติชันดิสก์และ Filesystem Type
lsblk -f

# ดู Partition Table แบบละเอียด (MBR vs GPT)
sudo fdisk -l

# อัปเดตการตั้งค่า GRUB Bootloader
sudo update-grub
```

---

## Module 08: Shell History Audit & Forensic Control
- **Status:** ⏳ Not Started
- **Core Concept:** การควบคุมประวัติคำสั่ง Shell: การป้องกันไม่ให้บันทึกรหัสผ่านในประวัติคำสั่ง, การค้นหาคำสั่งย้อนหลัง, และหลักการด้าน Forensic

### 💻 Essential Commands
```bash
# ตั้งค่าไม่ให้บันทึกคำสั่งที่ขึ้นต้นด้วยเว้นวรรค
export HISTCONTROL=ignorespace

# ค้นหาคำสั่งที่เคยพิมพ์ย้อนหลังอย่างรวดเร็ว
history | grep "ssh"

# ปิดไม่ให้บันทึก History ชั่วคราวใน Session ปัจจุบัน
set +o history
```

### 🧠 Feynman Takeaway & Reflection
#### 💡 แก่นความคิดและกลไกสำคัญ (Core Mental Models)

#### ⚠️ กับดักทางเทคนิคและข้อควรระวัง (Gotchas & Pitfalls)
