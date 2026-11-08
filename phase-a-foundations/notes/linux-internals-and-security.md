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
- **Core Concept:** ปรัชญาการออกแบบความปลอดภัยของ Linux: Discretionary Access Control (DAC), Principle of Least Privilege

---

## Module 02: Inodes & Filesystem Structure
- **Status:** ⏳ Not Started
- **Core Concept:** Inodes, Metadata, Hard links vs Symbolic links, และการเก็บข้อมูลบน Disk blocks

---

## Module 03: Network Interfaces & Configuration
- **Status:** ⏳ Not Started
- **Core Concept:** Network Configuration ผ่าน CLI: `ip addr`, `ip route`, `/etc/resolv.conf`, `/etc/hosts`

---

## Module 04: Source Compilation & Toolchain Build
- **Status:** ⏳ Not Started
- **Core Concept:** การติดตั้งเครื่องมือที่ไม่มีใน APT: `make`, `gcc`, `./configure`, Python Virtual Environments (`venv`)

---

## Module 05: Kernel Modules & Environment Tuning
- **Status:** ⏳ Not Started
- **Core Concept:** จัดการ Kernel Modules ด้วย `lsmod`, `modprobe`, `insmod`

---

## Module 06: Special Permissions & Text Processing
- **Status:** ⏳ Not Started
- **Core Concept:** พิชิต SUID (`4000`), SGID (`2000`), Sticky Bit (`1000`) และคำสั่งจัดการข้อความระดับเทพ (`awk`, `sed`, `grep`, `cut`, `sort`, `uniq`)

---

## Module 07: Bootloader & Partitioning Mechanics
- **Status:** ⏳ Not Started
- **Core Concept:** กระบวนการ Boot ของ Linux: BIOS/UEFI $\rightarrow$ GRUB Bootloader $\rightarrow$ Kernel $\rightarrow$ Init (Systemd)

---

## Module 08: Shell History Audit & Forensic Control
- **Status:** ⏳ Not Started
- **Core Concept:** การควบคุมประวัติคำสั่ง Shell: `history`, `HISTSIZE`, `HISTFILESIZE`, `Ctrl + R` (reverse-i-search), และการตั้งค่าความปลอดภัยไม่ให้บันทึกรหัสผ่าน
