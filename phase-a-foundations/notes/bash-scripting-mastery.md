# 💻 Bash Scripting & Automation Mastery

> **Phase:** `Phase A — Core Foundations` | **Tags:** `#bash` `#scripting` `#automation` `#tool-building`  
> **Status:** ⏳ Upcoming | **Focus:** Bash Automation, Variables, Conditionals, Loops & Functions

---

## 📑 Core Modules Overview

- [ ] [[#Module 01: Shebang, Permissions & First Script|Module 01: Shebang, Permissions & First Script]]
- [ ] [[#Module 02: Variables, User Input & CLI Arguments|Module 02: Variables & Arguments ($1, $@)]]
- [ ] [[#Module 03: Environment Variables & Arithmetic|Module 03: Environment Variables & Math Operations]]
- [ ] [[#Module 04: Conditionals & Flow Control|Module 04: if, elif, case Flow Control (Game Engine)]]
- [ ] [[#Module 05: Loops & Automation Iteration|Module 05: while, until, for Loops (Network Scanner)]]

---

## Module 01: Shebang, Permissions & First Script
- **Status:** ⏳ Not Started
- **Core Concept:** โครงสร้างพื้นฐานของ Bash script: Shebang (`#!/bin/bash`), สิทธิ์การรัน (`chmod +x`), และการเรียกใช้งาน (`./script.sh`)

### 📝 Key Notes & Mindset
- Shebang (`#!`) คือตัวบอก Kernel ว่าต้องใช้ Interpreter ตัวไหนมารันสคริปต์นี้
- คำสั่ง `chmod +x` เป็นการเพิ่มสิทธิ์ Execute ให้ไฟล์สคริปต์สามารถทำงานได้เหมือนโปรแกรม Binary
- Exit Code: `$?` คืนค่า `0` เมื่อคำสั่งก่อนหน้าทำงานสำเร็จ และค่าที่ไม่ใช่ `0` (เช่น `1`, `127`) เมื่อเกิด Error

### 💻 Template Code
```bash
#!/bin/bash

# สคริปต์เริ่มต้น: แสดงข้อความและเวอร์ชัน Shell
echo "=== OffSec Automation Engine Initialized ==="
echo "Running on Shell: $BASH_VERSION"
echo "Current Date: $(date)"

# Exit ด้วย status code 0 (Success)
exit 0
```

### 🧠 Feynman Takeaway & Reflection
- *สรุปด้วยคำพูดตัวเอง:* 
- *จุดที่น่าสนใจ / ข้อควรระวัง:* 

---

## Module 02: Variables, User Input & CLI Arguments
- **Status:** ⏳ Not Started
- **Core Concept:** การประกาศตัวแปร, การรับ Input จากผู้ใช้ด้วย `read`, และการส่ง Argument เข้ามาในสคริปต์ (`$1`, `$2`, `$@`)

### 📝 Key Notes & Mindset
- การตั้งชื่อตัวแปรใน Bash ห้ามมีเว้นวรรคข้างเครื่องหมายเท่ากับ (`NAME="Alice"` ห้าม `NAME = "Alice"`)
- Double Quotes (`"..."`) ยอมให้เกิด Variable Expansion (`$VAR`), Single Quotes (`'...'`) มองทุกอย่างเป็นตัวอักษรตายตัว (Literal)
- Argument ตัวแทน: `$0` = ชื่อไฟล์สคริปต์, `$1` = Target IP, `$#` = จำนวน Argument ทั้งหมด, `$@` = รายการ Argument ทั้งหมด

### 💻 Practical Script: Target Selection Bot
```bash
#!/bin/bash

# รับค่า Target ผ่าน Argument แรก หรือถามผ่าน read
TARGET=$1

if [ -z "$TARGET" ]; then
    read -p "Enter Target IP / Domain: " TARGET
fi

read -p "Enter Target Port [Default 80]: " PORT
PORT=${PORT:-80}

echo "[+] Initializing scan against Target: $TARGET on Port: $PORT"
echo "[+] Command that will be executed: nc -zv $TARGET $PORT"
```

### 🧠 Feynman Takeaway & Reflection
- *สรุปด้วยคำพูดตัวเอง:* 
- *จุดที่น่าสนใจ / ข้อควรระวัง:* 

---

## Module 03: Environment Variables & Arithmetic
- **Status:** ⏳ Not Started
- **Core Concept:** System Variables (`$USER`, `$PATH`, `$RANDOM`), การคำนวณเลขคณิตด้วย `$(( ))`

### 📝 Key Notes & Mindset
- `$RANDOM` สร้างตัวเลขสุ่ม 0 – 32767 มีประโยชน์มากในการสร้าง Port สุ่ม, รหัสชั่วคราว, หรือการสุ่มโจมตี
- การคำนวณเลขคณิตใน Bash ใช้ไวยากรณ์ `$(( expression ))` เช่น `$(( 10 * 5 ))`

### 💻 Practical Code: Random Generator & Math
```bash
#!/bin/bash

# สุ่มพอร์ต Listener สำหรับ Reverse Shell (ระหว่าง 10000 - 65535)
RANDOM_PORT=$(( (RANDOM % 55535) + 10000 ))
echo "[+] Generated Dynamic Listener Port: $RANDOM_PORT"

# คำนวณ Subnet Host Count: 2^(32 - CIDR) - 2
CIDR=24
HOSTS=$(( (2 ** (32 - CIDR)) - 2 ))
echo "[+] Subnet /$CIDR contains: $HOSTS usable host addresses"
```

### 🧠 Feynman Takeaway & Reflection
- *สรุปด้วยคำพูดตัวเอง:* 
- *จุดที่น่าสนใจ / ข้อควรระวัง:* 

---

## Module 04: Conditionals & Flow Control
- **Status:** ⏳ Not Started
- **Core Concept:** การตัดสินใจเงื่อนไข: `if-elif-else`, ตรวจสอบตัวเลข, String, การมีอยู่ของไฟล์ และการใช้ `case` Statement

### 📝 Key Comparison Operators Cheat Sheet
| Operator | Type | Meaning |
| :--- | :--- | :--- |
| `-eq`, `-ne` | Number | เท่ากับ, ไม่เท่ากับ |
| `-gt`, `-lt` | Number | มากกว่า, น้อยกว่า |
| `==`, `!=` | String | ข้อความตรงกัน, ไม่ตรงกัน |
| `-z`, `-n` | String | ข้อความว่างเปล่า, ข้อความไม่ว่าง |
| `-f` | File | ไฟล์นี้มีอยู่จริงและเป็น Regular file |
| `-d` | File | โฟลเดอร์นี้มีอยู่จริง |
| `-s` | File | ไฟล์มีขนาดมากกว่า 0 bytes |

### 💻 Practical Script: Port Scan Decision Engine
```bash
#!/bin/bash

TARGET=$1

if [ -z "$TARGET" ]; then
    echo "Usage: $0 <target-ip>"
    exit 1
fi

echo "Select Scan Profile:"
echo "1) Fast Scan (Top 100 ports)"
echo "2) Comprehensive Full Scan (All 65535 ports)"
echo "3) Vulnerability Script Scan"
read -p "Option [1-3]: " CHOICE

case $CHOICE in
    1)
        echo "[*] Launching Fast Scan on $TARGET..."
        nmap -F "$TARGET"
        ;;
    2)
        echo "[*] Launching Comprehensive Scan on $TARGET..."
        nmap -p- -T4 "$TARGET"
        ;;
    3)
        echo "[*] Launching Vuln Script Scan on $TARGET..."
        nmap --script=vuln "$TARGET"
        ;;
    *)
        echo "[-] Invalid option selected."
        exit 1
        ;;
esac
```

### 🧠 Feynman Takeaway & Reflection
- *สรุปด้วยคำพูดตัวเอง:* 
- *จุดที่น่าสนใจ / ข้อควรระวัง:* 

---

## Module 05: Loops & Automation Iteration
- **Status:** ⏳ Not Started
- **Core Concept:** การวนซ้ำ: `while`, `until`, `for` loop และการอ่านข้อมูลจากไฟล์ทีละบรรทัดสำหรับงานสแกนเป้าหมายจำนวนมาก

### 💻 Practical Script: Ping Sweeper (Host Discovery)
```bash
#!/bin/bash

# สแกน Subnet /24 เพื่อค้นหา Host ที่เปิดอยู่ (Host Discovery)
SUBNET="192.168.1"

echo "[*] Scanning active hosts in $SUBNET.0/24..."

for ip in $(seq 1 254); do
    # ส่ง Ping 1 ครั้ง รอไม่เกิน 1 วินาที ถ้ารอดให้แสดงผล
    ping -c 1 -W 1 "$SUBNET.$ip" > /dev/null 2>&1 && echo "[+] Host is UP: $SUBNET.$ip" &
done

# รอ Background processes ทั้งหมดทำงานเสร็จ
wait
echo "[*] Sweep completed."
```

### 🧠 Feynman Takeaway & Reflection
- *สรุปด้วยคำพูดตัวเอง:* 
- *จุดที่น่าสนใจ / ข้อควรระวัง:*
