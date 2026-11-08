# 🛠️ Linux Kernel Internals & Security Mechanisms

> **Playlist:** [Linux for Hackers with OTW](https://www.youtube.com/playlist?list=PLhfrWIlLOoKMj3K_p3fl7Jb9qlmm1Z46_) & Core Systems Shell Mastery  
> **Phase:** [phase-a-foundations](../) | **Tags:** `#linux` `#internals` `#security-internals` `#kernel` `#boot` `#history`  
> **Master Curriculum:** [[foundations-execution-blueprint#1.3 Core Systems + Kernel Security — Linux Basics for Hackers (6 ตอนระดับ Internals)|Curriculum Checklist (1.3 & 1.4)]]

---

## 📑 Course Episodes Overview

### ส่วนที่ 1.3: Linux Basics with Kernel Security (6 ตอน)
- [ ] [[#EP 1 — Linux for Hackers Tutorial & Philosophy|EP 1: ปรัชญาแฮกเกอร์กับ Linux จาก OTW]]
- [ ] [[#EP 2 — Deep File System Architecture|EP 2: สถาปัตยกรรมระบบไฟล์เชิงลึก]]
- [ ] [[#EP 3 — Linux Networking You Need to Know|EP 3: Network Interfaces, Routing & DNS]]
- [ ] [[#EP 4 — Compiling Tools from Source (Pip, Git, Apt)|EP 4: คอมไพล์ Toolchain จาก Source Code]]
- [ ] [[#EP 5 — Kernel Internals & Environment|EP 5: เคล็ดลับ Kernel & Environment Variables]]
- [ ] [[#EP 6 — Advanced Permissions (SUID/SGID) & Text Parsing|EP 6: SUID/SGID, awk, sed, grep]]

### ส่วนที่ 1.4: Shell Setup & Boot Mastery (2 ตอน)
- [ ] [[#EP 7 — Dual Boot & Bootloader Partitioning Mechanics|EP 7: โครงสร้าง Bootloader & Partitioning (Dual Boot)]]
- [ ] [[#EP 8 — Shell History Audit & Reverse Search|EP 8: Shell History Mastery & Reverse-i-search]]

---

# ส่วนที่ 1.3: OTW Linux Internals

## EP 1 — Linux for Hackers Tutorial & Philosophy
- **Video Link:** [YouTube — Linux for Hackers Tutorial // EP 1](https://www.youtube.com/watch?v=YJUVNlmIO6E)
- **Status:** ⏳ รอคิว
- **Core Concept:** ปรัชญาและมุมมองการมองระบบปฏิบัติการ Linux ของผู้เชี่ยวชาญสายความปลอดภัย

---

## EP 2 — Deep File System Architecture
- **Video Link:** [YouTube — Linux for Hackers Tutorial with OTW! (EP 2)](https://www.youtube.com/watch?v=8z-s5KQ9DbI)
- **Status:** ⏳ รอคิว
- **Core Concept:** Inodes, Hard links vs Symbolic links, และการเก็บข้อมูลบน Disk blocks

---

## EP 3 — Linux Networking You Need to Know
- **Video Link:** [YouTube — Linux Networking that you need to know (EP 3)](https://www.youtube.com/watch?v=t96iOUxC27M)
- **Status:** ⏳ รอคิว
- **Core Concept:** Network Configuration: `ip a`, `ip route`, `/etc/resolv.conf`, และ `/etc/hosts`

---

## EP 4 — Compiling Tools from Source (Pip, Git, Apt)
- **Video Link:** [YouTube — NEW Tools Install with OTW! (EP 4)](https://www.youtube.com/watch?v=EeGjmTx2diM)
- **Status:** ⏳ รอคิว
- **Core Concept:** การ Build Tool จาก Source code ด้วย `make`, `cmake`, และการแก้ Dependency ติดขัด

---

## EP 5 — Kernel Internals & Environment
- **Video Link:** [YouTube — Did you know this about Linux? (EP 5)](https://www.youtube.com/watch?v=nbznYEAdZfk)
- **Status:** ⏳ รอคิว
- **Core Concept:** สถาปัตยกรรม Linux Kernel, Loadable Kernel Modules (LKM - `lsmod`, `insmod`), `/proc` filesystem

---

## EP 6 — Advanced Permissions (SUID/SGID) & Text Parsing
- **Video Link:** [YouTube — LINUX commands you need to know (EP 6)](https://www.youtube.com/watch?v=DiGangfhJ4g)
- **Status:** ⏳ รอคิว
- **Core Concept:** สิทธิ์พิเศษ SUID (`4000`), SGID (`2000`), Sticky Bit (`1000`) สำหรับ Privilege Escalation และ Text wrangling ด้วย `awk`, `sed`, `grep`

---

# ส่วนที่ 1.4: Shell Setup & Boot Mastery

## EP 7 — Dual Boot & Bootloader Partitioning Mechanics
- **Video Link:** [YouTube — Dual Boot Windows and Ubuntu Linux in 10 Minutes](https://www.youtube.com/watch?v=UTqDuWHbZkw)
- **Status:** ⏳ รอคิว
- **Core Concept:** การทำงานของ GRUB 2 Bootloader, EFI System Partition (ESP), การแบ่งพาร์ทิชันแบบ MBR vs GPT และลำดับการบูตของ BIOS/UEFI

### 💻 Key Partition & Disk Inspection Commands
```bash
# ตรวจสอบพาร์ทิชันและไดรฟ์ทั้งหมด
lsblk -f

# ดู Partition Table แบบละเอียด (MBR/GPT)
sudo fdisk -l

# ตรวจสอบลำดับการบูตใน UEFI Boot Manager
sudo efibootmgr -v
```

---

## EP 8 — Shell History Audit & Reverse Search
- **Video Link:** [YouTube — Learn Linux in 180s - history command](https://www.youtube.com/watch?v=3e2oYpEKrXE)
- **Status:** ⏳ รอคิว
- **Core Concept:** การเรียกดู ค้นหา และทำความสะอาดประวัติคำสั่ง Shell (`history`, `!`, reverse-i-search)

### 💻 Essential Shell History Shortcuts
```bash
# ค้นหาคำสั่งเก่าแบบโต้ตอบ (Interactive Reverse Search)
# กดปุ่ม: Ctrl + R แล้วพิมพ์ keyword

# รันคำสั่งล่าสุดซ้ำทันที
!!

# รันคำสั่งล่าสุดโดยใส่ sudo นำหน้า (สุดยอดคีย์ลัด)
sudo !!

# รันคำสั่งเก่าตามหมายเลขใน history
!105

# OPSEC: ป้องกันไม่ให้คำสั่งถูกบันทึกลง history
# (เคาะ Spacebar 1 ครั้งหน้าคำสั่ง - ต้องเปิด setopt hist_ignore_space ไว้)
 hydra -l admin -P pass.txt 10.10.10.5 ssh
```
