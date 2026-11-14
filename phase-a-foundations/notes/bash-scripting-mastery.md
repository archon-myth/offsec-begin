# 💻 Bash Scripting & Automation Mastery

> **Phase:** `Phase A — Core Foundations` | **Tags:** `#bash` `#scripting` `#automation` `#tool-building`  
> **Status:** ✅ Completed | **Focus:** Bash Automation, Variables, Conditionals, Loops & Functions

---

## 📑 Core Modules Overview

- [x] [[#Module 01: Shebang, Permissions & First Script|Module 01: Shebang, Permissions & First Script]]
- [x] [[#Module 02: Variables, User Input & CLI Arguments|Module 02: Variables & Arguments ($1, $@)]]
- [x] [[#Module 03: Environment Variables & Arithmetic|Module 03: Environment Variables & Math Operations]]
- [x] [[#Module 04: Conditionals & Flow Control|Module 04: if, elif, case Flow Control (Game Engine)]]
- [x] [[#Module 05: Loops & Automation Iteration|Module 05: while, until, for Loops (Network Scanner)]]

---

## Module 01: Shebang, Permissions & First Script
- **Status:** ✅ Completed
- **Core Concept:** โครงสร้างพื้นฐานของ Bash script: Shebang (`#!/bin/bash`), สิทธิ์การรัน (`chmod +x`), และการเรียกใช้งาน (`./script.sh`)

### 📝 Key Notes & Mindset
- Shebang (`#!`) คือตัวบอก Kernel ว่าต้องใช้ Interpreter ตัวไหนมารันสคริปต์นี้
- คำสั่ง `chmod +x` เป็นการเพิ่มสิทธิ์ Execute ให้ไฟล์สคริปต์สามารถทำงานได้เหมือนโปรแกรม Binary
- Exit Code: `$?` คืนค่า `0` เมื่อคำสั่งก่อนหน้าทำงานสำเร็จ และค่าที่ไม่ใช่ `0` (เช่น `1`, `127`) เมื่อเกิด Error

### 💻 Essential Commands
```bash
# ตรวจสอบ Path ของ Default Login Shell ที่ใช้งานอยู่ (บน Kali มักจะได้ /usr/bin/zsh)
which $SHELL

# ตรวจสอบ Path ของ Bash Interpreter สำหรับใช้อ้างอิงใน Shebang
which bash

# ตรวจสอบว่า /bin เป็น Symlink ไปยัง /usr/bin หรือไม่ (แนวคิด usr-merge)
ls -ld /bin                     # ถ้าเห็น /bin -> usr/bin แสดงว่า /bin/bash และ /usr/bin/bash คือไฟล์เดียวกัน

# สร้างหรือเปิดแก้ไขไฟล์สคริปต์ด้วย Text Editor (nano)
nano name.sh                    # กด Ctrl+O เพื่อบันทึก (Save) แล้วกด Ctrl+X เพื่อออก

# รันสคริปต์ผ่าน Interpreter โดยตรง (ไม่ต้องตั้งสิทธิ์ Execute และข้ามการใช้ Shebang)
bash name.sh

# ตั้งสิทธิ์ Execute ให้ไฟล์ แล้วรันแบบ Executable (Kernel จะอ่าน Shebang เพื่อเลือก Interpreter)
chmod +x name.sh
./name.sh
```

### 💻 Template Code
```bash
#!/bin/bash

# สคริปต์เริ่มต้น: แสดงข้อความและเวอร์ชัน Shell
echo "=== OffSec Automation Engine Initialized ==="
sleep n                         # หน่วงเวลา n วินาที (sleep <n>) ก่อนทำงานคำสั่งถัดไป

echo "Running on Shell: $BASH_VERSION"
sleep 5                         # ตัวอย่างการหน่วงเวลา 5 วินาที

echo "Current Date: $(date)"

# Exit ด้วย status code 0 (Success)
exit 0
```

### 🧠 Feynman Takeaway & Reflection
#### 💡 แก่นความคิดและกลไกสำคัญ (Core Mental Models)
1. **Shebang (`#!`) คือการระบุ Interpreter ของสคริปต์ ไม่ใช่ Shell ที่กำลังพิมพ์อยู่:** แม้บน Kali Linux คำสั่ง `which $SHELL` จะคืนค่า `/usr/bin/zsh` (Interactive Login Shell) แต่เวลาเขียน Bash Script เรายังคงต้องใส่ `#!/bin/bash` (หรือ `#!/usr/bin/env bash`) เพื่อสั่งให้ Kernel เรียก Bash มารันสคริปต์นี้โดยตรง
2. **`/bin/bash` vs `/usr/bin/bash` และกลไก `usr-merge`:** คำว่า `/usr` ย่อมาจาก **Unix System Resources** (ไม่ใช่โฟลเดอร์ส่วนตัวของ User ซึ่งอยู่ที่ `/home/<user>`) บน Linux สมัยใหม่ `/bin` ถูกทำเป็น Symlink ชี้ไปยัง `/usr/bin` ทำให้ทั้งสอง Path ชี้ไปที่ Binary ตัวเดียวกัน 100%
3. **ทำไม `bash name.sh` รันได้เลย แต่ `./name.sh` ต้อง `chmod +x` ก่อน (Read vs Execute):**
   - `bash name.sh`: ตัวที่ถูก Execute จริงๆ คือ Binary `/bin/bash` (ที่มีสิทธิ์ `x` อยู่แล้ว) ส่วน `name.sh` เป็นเพียง **Input File** ที่ส่งให้ `bash` เปิดอ่านทีละบรรทัด จึงใช้แค่สิทธิ์ **Read (`r`)** และไม่สนใจ Shebang
   - `./name.sh`: เป็นการสั่งให้ **Kernel รันไฟล์ `name.sh` เป็น Executable โดยตรง** Kernel จึงบังคับตรวจสิทธิ์ **Execute (`x`)** ก่อนเสมอ (ไม่มีจะติด `Permission denied`) แล้วจึงอ่าน Shebang บรรทัดแรกเพื่อเรียก Interpreter มารัน — *นัยสำคัญทาง OffSec:* หากอยู่ในโฟลเดอร์ที่ถูก Mount แบบ `noexec` หรือตั้งสิทธิ์ `x` ไม่ได้ เรายังสามารถรันสคริปต์ผ่าน `bash name.sh` ได้เสมอ
4. **`#!/bin/sh` vs `#!/bin/bash` (สำคัญมากในงาน OffSec / Post-Exploitation):**
   - **`#!/bin/sh` (POSIX Shell / `dash`):** มีเฉพาะฟีเจอร์พื้นฐานตามมาตรฐาน POSIX (ใช้ได้แค่ `[ ... ]` และ `=`, ไม่มี `[[ ... ]]`, `(( ... ))`, Array, Regex `=~`) แต่จุดเด่นคือ **เบาและมีอยู่บนเครื่อง Unix/Linux แทบทุกเครื่องบนโลก**
   - **`#!/bin/bash` (Bourne Again Shell):** อัปเกรดฟีเจอร์ครบเครื่อง (`[[ ... ]]`, `(( ... ))`, Array, Regex `=~`, `{1..10}`) เหมาะสำหรับเขียนเครื่องมือ Automation บน Kali หรือ Server ทั่วไป
   - *นัยสำคัญทาง OffSec:* เมื่อเจาะเข้า Target ขนาดเล็ก เช่น **Alpine Docker Container, IoT, Router, หรือ Embedded Linux (BusyBox)** เครื่องเหล่านั้น **มักไม่มี `/bin/bash` ติดตั้งอยู่เลย** มีแค่ `/bin/sh` — หากสคริปต์ Enumeration/Exploit ที่ส่งไปรันใช้ `#!/bin/bash` จะพังทันที (ต้องใช้ `#!/bin/sh` และเขียนแบบ POSIX `[ ... ]` โดยครอบ `"$var"` เสมอ, ใช้ `=` แทน `==`, และเชื่อมหลายเงื่อนไขด้วย `[ ... ] && [ ... ]`)

#### ⚠️ กับดักทางเทคนิคและข้อควรระวัง (Gotchas & Pitfalls)
1. **กับดักใส่ `#!/usr/bin/zsh` ตาม `which $SHELL`:** หากใส่ Shebang เป็น `/usr/bin/zsh` สคริปต์จะถูกรันด้วย Zsh แทน Bash ซึ่งมีพฤติกรรมต่างกัน (เช่น Array ของ Zsh เริ่มที่ Index 1 ส่วน Bash เริ่มที่ 0) และเมื่อนำสคริปต์ไปรันบน Target Server ที่ไม่มี `zsh` ติดตั้งไว้ (เช่น Ubuntu/Alpine) สคริปต์จะรันไม่ได้ทันที ให้ใช้ `which bash` ตรวจสอบ Path ของ Bash หรือใช้ `#!/usr/bin/env bash` เพื่อความ Portable ข้ามระบบ
2. **กับดัก `#!/bin/bash` vs `#!/usr/bin/bash` บน Target รุ่นเก่า:** แม้บนเครื่องสมัยใหม่จะเหมือนกัน แต่บน Target ระบบเก่าที่ยังไม่ทำ `usr-merge` จะมี `/bin/bash` เสมอ แต่อาจไม่มี `/usr/bin/bash` (ทำให้เกิด Error `bad interpreter`) ดังนั้นในงาน OffSec ควรยึด **`#!/bin/bash`** เป็นค่ามาตรฐาน หรือใช้ **`#!/usr/bin/env bash`** เมื่อต้องการให้สคริปต์ทำงานได้ข้าม OS ที่เก็บ Bash ไว้คนละ Path

---

## Module 02: Variables, User Input & CLI Arguments
- **Status:** ✅ Completed
- **Core Concept:** การประกาศตัวแปร, การรับ Input จากผู้ใช้ด้วย `read`, และการส่ง Argument เข้ามาในสคริปต์ (`$1`, `$2`, `$@`)

### 📝 Key Notes & Mindset
- การตั้งชื่อตัวแปรใน Bash ห้ามมีเว้นวรรคข้างเครื่องหมายเท่ากับ (`NAME="Alice"` ห้าม `NAME = "Alice"`)
- Double Quotes (`"..."`) ยอมให้เกิด Variable Expansion (`$VAR`), Single Quotes (`'...'`) มองทุกอย่างเป็นตัวอักษรตายตัว (Literal)
- Argument ตัวแทน: `$0` = ชื่อไฟล์สคริปต์, `$1` = Target IP, `$#` = จำนวน Argument ทั้งหมด, `$@` = รายการ Argument ทั้งหมด

### 💻 Practical Script: Target Selection Bot
```bash
#!/bin/bash

# การประกาศตัวแปร (ห้ามมีเว้นวรรคข้างเครื่องหมาย =)
name="ReconBot"
echo "[+] Active Tool: $name"        # ใช้ $name เพื่อดึงค่าภายในตัวแปรออกมา (ถ้าไม่ใส่ $ จะพิมพ์คำว่า name ตรงๆ)

# Command Substitution: รันคำสั่งข้างใน $() แล้วนำ Output มาเก็บใส่ตัวแปร
user=$(whoami)
echo "[+] Running as: $user"

# การรับค่าจากผู้ใช้ (User Input) — แบบแยกบรรทัด vs แบบบรรทัดเดียว (-p)
echo "My name:"                     # echo จะขึ้นบรรทัดใหม่ ทำให้ผู้ใช้ต้องพิมพ์คำตอบในบรรทัดถัดไป
read name

read -p "My name: " name            # -p (prompt) แสดงข้อความถามแล้วรอรับค่าต่อท้ายในบรรทัดเดียวกันทันที

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

### 💻 Practical Script: System & API Info Fetcher
```bash
#!/bin/bash

# ดึงข้อมูล Private/Public IP, สภาพอากาศ และ API ด้วย Command Substitution
PRIVATE_IP=$(hostname -I | awk '{print $1}')
PUBLIC_IP=$(curl -s ifconfig.me)
WEATHER=$(curl -s "wttr.in/?0")
JOKE=$(curl -s -H "Accept: text/plain" https://icanhazdadjoke.com/)

echo "Your Private IP Address is $PRIVATE_IP and your Public IP Address is $PUBLIC_IP"
echo "The weather today is:"
echo "$WEATHER"
echo ""
echo "Here is your dad joke of the day: $JOKE"
```

### 🧠 Feynman Takeaway & Reflection
#### 💡 แก่นความคิดและกลไกสำคัญ (Core Mental Models)
1. **ทำไมประกาศใช้ `name="..."` แต่ตอนเรียกใช้ต้องมี `$name` (Variable Expansion):**
   - **ตอนกำหนดค่า (`name="ReconBot"`):** เรากำลังอ้างถึง **ตัวกล่อง/ชื่อตัวแปร** เพื่อเก็บข้อมูลลงไป จึงไม่ต้องมี `$`
   - **ตอนเรียกใช้ (`$name`):** ใน Bash ทุกคำที่พิมพ์จะถูกมองเป็น **ข้อความธรรมดา (Literal String)** โดยอัตโนมัติ (เช่น `echo name` จะได้คำว่า `name`) การเติม **`$`** ข้างหน้าคือคำสั่งให้ Shell ทำ **Variable Expansion** เพื่อเปิดกล่องแล้วดึง "ค่าข้างใน" ออกมาแทนที่
2. **การรับ Input ด้วย `echo` + `read` vs `read -p`:**
   - **`echo "My name:"` + `read name`:** `echo` จะขึ้นบรรทัดใหม่ (Newline) ให้อัตโนมัติ ทำให้ Cursor ไปรอรับค่าที่ **บรรทัดถัดไป** ด้านล่างข้อความ
   - **`read -p "My name: " name`:** Flag **`-p` (Prompt)** สั่งให้ `read` พิมพ์ข้อความถามและรอรับ Input **ต่อท้ายในบรรทัดเดียวกันทันที** (นิยมเว้นวรรค 1 ช่องท้ายข้อความใน Quote เพื่อไม่ให้ตัวอักษรติดกัน) — หากต้องการรับรหัสผ่าน/Token แบบไม่โชว์บนหน้าจอ ให้เพิ่ม `-s` (Silent) เป็น `read -sp "Password: " PASS`
3. **กลไก Positional Parameters (`$0`, `$1`, `$2`):**
   - เมื่อรัน `./name.sh Alice 8080` Bash จะแยกคำที่พิมพ์ตามช่องว่างแล้วโยนใส่กล่องหมายเลขตามลำดับให้โดยอัตโนมัติ: **`$0`** = `./name.sh` (ชื่อสคริปต์), **`$1`** = `Alice` (Argument ตัวที่ 1), **`$2`** = `8080` (Argument ตัวที่ 2)
   - การเขียน `name=$1` ในสคริปต์จึงเป็นการสั่งให้ **"หยิบค่าจากกล่อง `$1` มาเก็บไว้ในตัวแปร `name`"** เพื่อให้อ่านความหมายในโค้ดได้ง่ายขึ้นแทนที่จะเรียก `$1` ไปตลอดทั้งไฟล์
4. **Command Substitution (`$(command)`):**
   - ถ้าเขียน `user="whoami"` ตัวแปร `user` จะได้แค่ข้อความคำว่า `"whoami"` ตรงๆ ไม่ได้รันคำสั่ง
   - การครอบด้วย **`$(...)`** (เช่น `user=$(whoami)`) คือการสั่งให้ Bash **รันคำสั่งข้างในก่อน แล้วหยิบผลลัพธ์ (Stdout) ที่คำสั่งนั้นพิมพ์ออกมา มาเก็บใส่ตัวแปร** แทนที่จะเก็บชื่อคำสั่ง (เช่น รัน `whoami` ได้ `kali` แล้วเก็บ `kali` ลงใน `user`)

#### ⚠️ กับดักทางเทคนิคและข้อควรระวัง (Gotchas & Pitfalls)
1. **กับดักเว้นวรรครอบเครื่องหมาย `=`:** หากพิมพ์ `name = "ReconBot"` สคริปต์จะ Error (`command not found`) เพราะ Bash มองว่า `name` คือชื่อคำสั่ง แล้วส่ง `=` กับ `"ReconBot"` เป็น Argument
2. **กับดัก Quotes (`"$name"` vs `'$name'` vs `$name`):** ใน Bash ช่องว่าง (Spacebar) คือตัวแบ่งไฟล์คนละตัว (เช่น `cat a.txt b.txt` คือเปิด 2 ไฟล์) สมมติให้ `file="my target.txt"` (ชื่อไฟล์ที่มีเว้นวรรคตรงกลาง)
   - `'$file'` (**Single Quote**): ล็อกทุกอย่างเป็นตัวอักษรตรงๆ ไม่ดึงค่าตัวแปร → `echo '$file'` จะพิมพ์คำว่า `$file` ออกมาเฉยๆ
   - `"$file"` (**Double Quote — แนะนำให้ใช้เสมอ**): แปลงเป็น `cat "my target.txt"` → มีฟันหนูครอบ ทำให้ Bash รู้ว่าเป็นไฟล์เดียวที่มีเว้นวรรค
   - `$file` (**ไม่ใส่ Quote**): Bash หยิบค่ามาแปะตรงๆ กลายเป็น `cat my target.txt` (เหมือนพิมพ์โดยไม่มีฟันหนู) → Bash เข้าใจผิดว่าสั่งเปิด 2 ไฟล์คือ `my` กับ `target.txt` แล้ว Error ทันที

---

## Module 03: Environment Variables & Arithmetic
- **Status:** ✅ Completed
- **Core Concept:** System Variables (`$USER`, `$PWD`, `$SHELL`, `$HOSTNAME`, `$PATH`, `$RANDOM`), การคำนวณเลขคณิตด้วย `$(( ))`

### 📝 Key Notes & Mindset
- Built-in Environment Variables ของระบบจะใช้ **ตัวพิมพ์ใหญ่ทั้งหมด (UPPERCASE)** เสมอ (ดูทั้งหมดได้ด้วยคำสั่ง `env` หรือ `printenv`)
- `$RANDOM` สร้างตัวเลขสุ่ม 0 – 32767 มีประโยชน์มากในการสร้าง Port สุ่ม, รหัสชั่วคราว, หรือการสุ่มโจมตี
- การคำนวณเลขคณิตใน Bash ใช้ไวยากรณ์ `$(( expression ))` เช่น `$(( 10 * 5 ))`

### 💻 Essential Commands
```bash
# แสดงและค้นหาตัวแปร Environment Variables ทั้งหมดใน Session ปัจจุบัน
env                             # แสดง Environment Variables ทั้งหมด
printenv PATH                   # แสดงค่าของตัวแปรที่ระบุแบบเจาะจง

# ประกาศตัวแปรให้ Subshell / Child Process มองเห็น และลบตัวแปร
export TARGET_IP="10.10.10.1"   # ส่งต่อตัวแปรไปยัง Child Process ที่ถูกเรียกจาก Shell นี้
unset TARGET_IP                 # ลบตัวแปรออกจาก Environment

# โหลดไฟล์คอนฟิกของ Shell ใหม่ทันทีหลังเพิ่ม export แบบถาวร (Kali ใช้ .zshrc / Ubuntu ใช้ .bashrc)
source ~/.zshrc                 # สำหรับ Zsh (Kali Linux)
source ~/.bashrc                # สำหรับ Bash (Ubuntu / Debian)

# เรียกดู Built-in Environment Variables สำคัญ (ต้องพิมพ์ตัวใหญ่ทั้งหมด)
echo "$PWD"                     # ไดเรกทอรีปัจจุบัน (Present Working Directory)
echo "$USER"                    # ชื่อผู้ใช้งานปัจจุบัน
echo "$UID"                     # User ID (0 = root)
echo "$SHELL"                   # Default Login Shell ของผู้ใช้
echo "$HOSTNAME"                # ชื่อเครื่อง (Host Name)
echo "$HOME"                    # Home Directory ของผู้ใช้
echo "$PATH"                    # รายชื่อโฟลเดอร์ที่ระบบใช้ค้นหาคำสั่ง
echo "$RANDOM"                  # ตัวเลขสุ่ม 0 - 32767
echo "$?"                       # Exit Code ของคำสั่งล่าสุด (0 = สำเร็จ)

# การคำนวณเลขคณิตด้วย Arithmetic Expansion (เว้นวรรคหรือไม่เว้นวรรคก็ได้)
echo $(( 10 + 5 ))              # 15 (บวก)
echo $((10+5))                  # 15 (ไม่เว้นวรรคก็ทำงานได้เหมือนกัน)
echo $(( 10 % 3 ))              # 1 (Modulo หาเศษจากการหาร)
echo $(( 2 ** 8 ))              # 256 (ยกกำลัง)

# สุ่มตัวเลขในช่วงกว้างเกินเพดาน 32767 ของ $RANDOM ด้วย shuf (Shuffle)
shuf -i 10000-65535 -n 1        # -i (input-range) ช่วงตัวเลข 10000-65535, -n 1 สุ่มออกมา 1 ค่า
```

### 💻 Practical Code: Random Generator & Math
```bash
#!/bin/bash

# สุ่มพอร์ต Listener สำหรับ Reverse Shell (ช่วง 10000 - 29999 ด้วย $RANDOM และ 10000 - 65535 ด้วย shuf)
RANDOM_PORT=$(( (RANDOM % 20000) + 10000 ))
FULL_PORT=$(shuf -i 10000-65535 -n 1)
echo "[+] Generated Dynamic Listener Port: $RANDOM_PORT (Full range: $FULL_PORT)"

# คำนวณ Subnet Host Count: 2^(32 - CIDR) - 2
CIDR=24
HOSTS=$(( (2 ** (32 - CIDR)) - 2 ))
echo "[+] Subnet /$CIDR contains: $HOSTS usable host addresses"
```

### 🧠 Feynman Takeaway & Reflection
#### 💡 แก่นความคิดและกลไกสำคัญ (Core Mental Models)
1. **ทำไม `echo "$pwd"` ได้ค่าว่าง แต่ `echo "$PWD"` ใช้งานได้ (Case-Sensitive & Naming Convention):**
   - Bash แยกตัวพิมพ์เล็ก-ใหญ่ชัดเจน (`$pwd` กับ `$PWD` คือคนละตัวแปรกันโดยสิ้นเชิง)
   - ตัวแปรสภาพแวดล้อมที่ระบบเตรียมไว้ให้ (Environment Variables เช่น `$PWD`, `$USER`, `$SHELL`, `$HOSTNAME`, `$HOME`, `$PATH`) ใช้ **ตัวพิมพ์ใหญ่ทั้งหมด** ตามมาตรฐาน POSIX
   - ดังนั้น `echo "$pwd"` จึงไปเรียกตัวแปรที่ไม่ได้ประกาศไว้ (เลยได้ค่าว่าง) — อย่าสับสนกับ `pwd` เฉยๆ ซึ่งเป็น **คำสั่ง** (หากจะเรียกใช้คำสั่งต้องเขียน `$(pwd)`)
2. **ทำไม `$((expression))` ใช้แบบไม่เว้นวรรคได้ (Arithmetic Mode):**
   - เครื่องหมาย `$(( ... ))` เป็น **Arithmetic Evaluation Mode** ซึ่ง Parser ของ Bash จะอ่านตามกฎคณิตศาสตร์ (แยกเครื่องหมาย `+`, `-`, `*`, `/`, `%`, `**` ออกจากตัวเลข/ตัวแปรอัตโนมัติ) ไม่ได้ใช้ช่องว่างเป็นตัวแบ่งคำสั่งเหมือนโหมด Terminal ปกติ ดังนั้น `$(( 5 + 3 ))` และ `$((5+3))` จึงทำงานได้เหมือนกัน 100% (การเว้นวรรคเป็นเพียง Style เพื่อให้อ่านง่าย)
3. **Local Variable vs Environment Variable (`export` และ Parent/Child Subshell):**
   - เมื่อตั้งตัวแปรปกติใน Terminal เช่น `target_host="lab-server-01"` แล้วสั่ง `echo $target_host` จะเห็นค่าตามปกติ **แต่** เมื่อรัน `./script.sh` ที่มี `echo $target_host` ข้างใน กลับพิมพ์ออกมาเป็นค่าว่าง!
   - **เหตุผล:** การรัน `./script.sh` คือการให้ Shell หลัก (Parent Shell) สร้าง **Subshell (Child Process)** ขึ้นมาใหม่เพื่อรันสคริปต์ ตัวแปรที่ไม่ได้ `export` จะเป็นแค่ **Local Variable** ที่อยู่เฉพาะใน Parent Shell ไม่ถูกส่งต่อไปยัง Child Process (ต่างจาก `$PWD`, `$USER`, `$SHELL`, `$HOSTNAME` ที่ระบบ `export` ไว้ให้แล้ว)
   - เมื่อสั่ง **`export target_host`** จะเป็นการเลื่อนขั้นตัวแปรให้เป็น **Environment Variable** ทำให้ Child Process/สคริปต์ที่ถูกเรียกหลังจากนั้นมองเห็นค่า `lab-server-01` ทันที *(ส่งต่อแบบทางเดียว Parent → Child เท่านั้น และหายไปเมื่อปิด Terminal เว้นแต่จะเขียนลง `~/.bashrc` หรือ `~/.zshrc`)*
4. **การไม่ใส่ `$` หน้าตัวแปรใน `$(( ... ))` และสูตรสุ่มช่วงด้วย Modulo (`%`):**
   - **ทำไมเขียน `RANDOM` แทน `$RANDOM` ใน `$(( ... ))`:** เพราะอยู่ในโหมดคำนวณเลขคณิต (Arithmetic Mode) Bash รู้ว่าข้างในคือตัวแปรตัวเลข จึงเรียกชื่อตัวแปรได้ตรงๆ และปลอดภัยกว่า (หากตัวแปร `x` ว่างเปล่า `$(( x * 2 ))` หรือ `$(( 10 + x ))` จะตีความ `x` เป็น `0` อัตโนมัติ แต่ถ้าใส่ `$` เป็น `$(( $x * 2 ))` หรือ `$(( 10 + $x ))` ตัว `$x` จะหายไปเหลือแค่ `$(( * 2 ))` หรือ `$(( 10 + ))` แล้วพังทันทีด้วย `syntax error: operand expected` — ยกเว้น `$1`, `$2` ที่ยังต้องใส่ `$`)
   - **สูตรสุ่มช่วง `[min, max]`:** `(( (RANDOM % (max - min + 1)) + min ))` — เครื่องหมาย `% N` จะได้เศษ **`0` ถึง `N - 1`** เสมอ แล้วบวก `min` เพื่อเลื่อนจุดเริ่มต้นขึ้นไป (เช่น `RANDOM % 20000` ได้ `0–19999`, พอ `+ 10000` จะได้ `10000–29999`)

#### ⚠️ กับดักทางเทคนิคและข้อควรระวัง (Gotchas & Pitfalls)
1. **กับดักตั้งชื่อตัวแปรตัวใหญ่ชนกับระบบ:** ในสคริปต์ทั่วไปควรตั้งชื่อตัวแปรส่วนตัวเป็น **ตัวพิมพ์เล็ก** (เช่น `target_ip`, `user_name`) เพื่อป้องกันการเผลอเขียนทับ Environment Variables ของระบบ (เช่น ถ้าเผลอตั้ง `PATH="something"` คำสั่งพื้นฐานอย่าง `ls`, `cat` จะหาไม่เจอทันที!)
2. **กับดักจำสลับระหว่าง `$((...))` กับ `[ ... ]` ใน `if`:** แม้ `$((expression))` จะไม่ต้องเว้นวรรค แต่วงเล็บเงื่อนไข `[ -z "$VAR" ]` **บังคับต้องเว้นวรรคหลัง `[` และก่อน `]` เสมอ** เพราะ `[` ใน Bash เป็นคำสั่ง (Command) ไม่ใช่ Arithmetic Mode!
3. **Bash Arithmetic รองรับเฉพาะจำนวนเต็ม (Integer Only):** `$(( 5 / 2 ))` จะได้ `2` (ปัดเศษทศนิยมทิ้ง) หากต้องการคำนวณทศนิยมใน Bash ต้องส่งเข้า `bc` หรือ `awk` แทน
4. **กับดัก `~/.bashrc` vs `~/.zshrc` บน Kali Linux:**
   - **ทำไม Kali ต้องใช้ `~/.zshrc` แทน `~/.bashrc`:** ไฟล์คอนฟิกที่จะถูกอ่านตอนเปิด Terminal ขึ้นอยู่กับ Interactive Shell ที่เราใช้อยู่ (`which $SHELL`) — ระบบที่ใช้ Bash (`/bin/bash` เช่น Ubuntu / Debian) จะอ่าน **`~/.bashrc`** ส่วนระบบที่ใช้ Zsh (`/usr/bin/zsh` เช่น **Kali Linux** / macOS) จะอ่าน **`~/.zshrc`** (และ **ไม่อ่าน** `~/.bashrc`) ดังนั้นบน Kali ต้องนำไปใส่ใน `~/.zshrc` แล้วรัน `source ~/.zshrc` เพื่อให้มีผลทันที
   - **ทำไมใส่ใน `~/.zshrc` แล้วสคริปต์ `#!/bin/bash` ถึงมองเห็นด้วย:** เพราะตอนเราพิมพ์ `./script.sh` ตัว **Zsh** คือ **Parent Shell** ส่วนสคริปต์ `#!/bin/bash` คือ **Child Process** ที่แตกออกมา — ค่าที่ `export` ใน `~/.zshrc` จึงถูกส่งต่อลงไปให้สคริปต์ Bash มองเห็นได้ตามปกติ
5. **กับดักเพดาน 15-bit ของ `$RANDOM` (`0 – 32767`):** `$RANDOM` สุ่มเลขได้สูงสุดแค่ `32767` เท่านั้น จึงไม่ครอบคลุมเลขพอร์ตสูงถึง `65535` (หากใช้ `RANDOM % 55536 + 10000` ค่าสูงสุดจะได้แค่ `42767`) สำหรับงาน OffSec ที่ต้องการสุ่มพอร์ตเต็มช่วง `10000–65535` ให้ใช้ `shuf -i 10000-65535 -n 1` แทน

---

## Module 04: Conditionals & Flow Control
- **Status:** ✅ Completed
- **Core Concept:** การตัดสินใจเงื่อนไข: `if-elif-else`, ตรวจสอบตัวเลข, String, การมีอยู่ของไฟล์ และการใช้ `case` Statement

### 📝 Key Comparison Operators Cheat Sheet
| Operator          | Type   | Meaning                                                                        |
| :---------------- | :----- | :----------------------------------------------------------------------------- |
| `-eq`, `-ne`      | Number | เท่ากับ (`equal`), ไม่เท่ากับ (`not equal`)                                    |
| `-gt`, `-lt`      | Number | มากกว่า (`greater than`), น้อยกว่า (`less than`)                               |
| `-ge`, `-le`      | Number | มากกว่าหรือเท่ากับ (`greater or equal`), น้อยกว่าหรือเท่ากับ (`less or equal`) |
| `=` / `==`, `!=`  | String | ข้อความตรงกัน, ไม่ตรงกัน                                                       |
| `-z`, `-n`        | String | ข้อความว่างเปล่า (`zero length`), ข้อความไม่ว่าง (`non-zero`)                  |
| `-e`              | File   | มีอยู่จริง (ทั้งไฟล์, โฟลเดอร์, หรือ Symlink)                                  |
| `-f`              | File   | ไฟล์นี้มีอยู่จริงและเป็น Regular file                                          |
| `-d`              | File   | โฟลเดอร์นี้มีอยู่จริง                                                          |
| `-s`              | File   | ไฟล์มีขนาดมากกว่า 0 bytes (ไม่ว่างเปล่า)                                       |
| `-r`, `-w`, `-x`  | File   | มีสิทธิ์ อ่าน (`read`), เขียน (`write`), รัน (`execute`)                       |
| `-L`              | File   | เป็น Symbolic Link (Symlink)                                                   |
| `&&`, `\|\|`, `!` | Logic  | และ (`AND`), หรือ (`OR`), นิเสธ (`NOT`)                                        |

> **POSIX `#!/bin/sh` Note:** ตัวดำเนินการ Number (`-eq`..`-le`), File (`-e`..`-L`), และ `-z`, `-n`, `!=` ใช้ใน `#!/bin/sh` ได้ **เหมือนเดิมทั้งหมด** ยกเว้น 2 จุด: เทียบ String เท่ากันต้องใช้ **`=`** ตัวเดียว (ห้าม `==`) และ `&&` / `||` ต้องเชื่อมข้างนอก `[ ... ] && [ ... ]` เท่านั้น

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

### 💻 Practical Code: `case` Multi-Pattern & Word Matching
```bash
#!/bin/bash

read -p "Action [start/stop/status] or confirm (y/n): " ACTION

case "$ACTION" in
    start|START)                    # คำทั่วไปที่ไม่มีเว้นวรรค ไม่ต้องใส่ "" ก็ได้ (คั่นหลายค่าด้วย |)
        echo "[+] Service started"
        ;;
    y|Y|yes|YES)                    # จับหลายคำตอบพร้อมกันในเคสเดียว
        echo "[+] Confirmed"
        ;;
    "dry run")                      # คำที่มีเว้นวรรค ต้องครอบ "" ไว้
        echo "[*] Simulating..."
        ;;
    10.*)                           # รองรับ Wildcard (จับ IP ที่ขึ้นต้นด้วย 10.)
        echo "[+] Internal lab subnet matched"
        ;;
    *)                              # Default Case (เหมือน else)
        echo "[-] Unknown action: $ACTION"
        ;;
esac
```

### 💻 Practical Code: `if` / `if-else` / `if-elif-else` Patterns
```bash
#!/bin/bash

read -p "Confirm target scope? (y/n): " ans

# โครงสร้าง if-else (ใช้ [[ ... ]] ที่ปลอดภัยต่อค่าว่างและเว้นวรรค)
if [[ $ans == "y" ]]; then
    echo "[+] Scope confirmed. Proceeding..."
elif [[ $ans == "n" ]]; then
    echo "[-] Aborted by operator."
    exit 0
else
    echo "[!] Invalid input."
    exit 1
fi

# การเชื่อมหลายเงื่อนไข (AND / OR / NOT) — เลือกใช้ตามลักษณะข้อมูล
num=110
if (( num > 100 && num < 120 )); then                 # 1. ตัวเลขล้วน: ใช้ (( ... )) เขียนเหมือน C/Java ได้เลย
    echo "[+] Numeric range matched"
fi

if [[ $num -gt 100 && $num -lt 120 ]]; then           # 2. ทั่วไป (String/File/Number): ใช้ [[ ... ]] เชื่อม &&, || ข้างในได้
    echo "[+] Double-bracket condition matched"
fi

if [ "$num" -gt 100 ] && [ "$num" -lt 120 ]; then     # 3. POSIX [ ... ]: ต้องแยกวงเล็บแล้วเชื่อม &&, || ข้างนอก
    echo "[+] POSIX condition matched"
fi

# Nested if (if ซ้อน if) — เปิด if กี่ตัว ต้องปิดด้วย fi ครบทุกตัว (ตัวในปิดก่อน ตัวนอกปิดทีหลัง)
if [[ $EUID -eq 0 ]]; then
    if [[ -f "/etc/shadow" ]]; then
        echo "[+] Running as root and /etc/shadow is accessible"
    else
        echo "[!] Running as root, but target file missing"
    fi
fi
```

### 🧠 Feynman Takeaway & Reflection
#### 💡 แก่นความคิดและกลไกสำคัญ (Core Mental Models)
1. **โครงสร้าง `if` / `elif` / `else` และทำไมต้อง `; then` กับ `fi`:**
   - `; then` ใช้คั่นคำสั่งเมื่อเขียน `then` ต่อท้ายบรรทัดเดียวกับ `if` (หากไม่ใส่ `;` ต้องขึ้นบรรทัดใหม่)
   - `fi` คือคำว่า `if` เขียนกลับหลัง เพื่อประกาศจุดสิ้นสุดบล็อกเงื่อนไข (หลักการเดียวกับ `case` ที่ปิดด้วย `esac`)
2. **`[ ... ]` (Single Bracket) vs `[[ ... ]]` (Double Bracket) vs `(( ... ))`:**
   - `[ ... ]` คือคำสั่ง `test` ดั้งเดิม (POSIX) ใช้ได้กับ `#!/bin/sh` แต่ต้องครอบ `"$var"` เสมอ และใช้ `&&` / `||` ข้างในไม่ได้ (ต้องแยกเป็น `[ cond1 ] && [ cond2 ]`)
   - `[[ ... ]]` เป็น Bash/Zsh Keyword ที่อัปเกรดแล้ว: ปลอดภัยกว่าแม้ตัวแปรว่างหรือมีเว้นวรรค, ใช้ `&&` / `||` / `( ... )` ข้างในได้ตรงๆ, และรองรับ Wildcard (`== y*`) กับ Regex (`=~`) — สำหรับสคริปต์ `#!/bin/bash` แนะนำให้ใช้ `[[ ... ]]` เป็นหลัก
   - `(( ... ))` สำหรับเปรียบเทียบ **ตัวเลขล้วน** — ใช้ `>`, `<`, `>=`, `<=`, `==`, `&&`, `||`, `!` ได้เหมือนภาษา C/Java โดยไม่ต้องใส่ `$` หน้าตัวแปร *(ใช้ได้เฉพาะ `#!/bin/bash` เช่นเดียวกับ `[[ ... ]]` ใช้ใน `#!/bin/sh` ไม่ได้)*
3. **Exit Codes (`exit 0` vs `exit 1` และรหัสมาตรฐาน `0–255`):**
   - `exit 0` = **สำเร็จ (Success)** ทำให้คำสั่งที่เชื่อมด้วย `&&` ทำงานต่อ; `exit 1` (หรือเลขที่ไม่ใช่ `0`) = **ล้มเหลว (Failure)** ทำให้ `&&` หยุดทำงานทันที (เช็คค่าล่าสุดด้วย `$?`)
   - **รหัสมาตรฐานที่พบบ่อย:** `0` (Success), `1` (General Error), `2` (Invalid Usage/Syntax), `126` (Permission Denied — ไฟล์ไม่มีสิทธิ์ `x`), `127` (Command Not Found), `130` (กด `Ctrl+C` / `128+2`), `137` (ถูก `kill -9` / `128+9`), `143` (ถูก `kill` / `128+15`) — *กฎ `128 + N`:* Process ที่ถูกฆ่าด้วย Signal `N` จะคืน Exit Code เป็น `128 + N` เสมอ
4. **กลไก `case` Statement (ตัวอักษร, คำ, `|`, Wildcard และ POSIX):**
   - `case` จับคู่ได้ทั้งตัวเลข, ตัวอักษร, และคำ — **คำทั่วไปที่ไม่มีเว้นวรรคไม่ต้องใส่ `""`** (เช่น `start|restart)`), แต่ **ถ้ามีเว้นวรรคต้องใส่ `""`** (เช่น `"dry run")`) และถ้าอยากจับเครื่องหมาย `*` ตรงๆ โดยไม่ให้เป็น Wildcard ต้องใส่ `"*"`
   - ใช้ `|` คั่นหลายค่าในเคสเดียว (`y|Y|yes)`), ใช้ Wildcard (`10.*)`), ปิดแต่ละเคสด้วย `;;`, ใช้ `*)` เป็น Default, และปิดบล็อกด้วย `esac` — **รองรับทั้ง `#!/bin/bash` และ `#!/bin/sh`**

#### ⚠️ กับดักทางเทคนิคและข้อควรระวัง (Gotchas & Pitfalls)
1. **ทำไม `[ ... ]` และ `[[ ... ]]` บังคับต้องเว้นวรรคหลัง `[` และก่อน `]`:** เพราะใน Bash **`[` คือชื่อคำสั่ง (Command)** (เทียบเท่า `test`) ส่วน `]` คือ Argument ตัวสุดท้าย หากพิมพ์ติดกัน เช่น `[$ans == "y"]` ระบบจะหาคำสั่งชื่อ `[$ans` ไม่เจอแล้วพังด้วย `command not found` ทันที (ต่างจาก `$((...))` ที่เป็นโหมดคณิตศาสตร์จึงไม่ต้องเว้นวรรคได้)
2. **กับดักลืม `"` ใน `[ ... ]`:** หากใช้ Single Bracket `[ $ans == "y" ]` แล้วผู้ใช้กด Enter เปล่าๆ จะกลายเป็น `[ == "y" ]` แล้ว Error ทันที (ต้องเขียน `[ "$ans" == "y" ]` หรือเปลี่ยนไปใช้ `[[ ... ]]`)
3. **กับดัก Nested `if` (`if` ซ้อน `if`) ลืมปิด `fi`:** เปิด `if` กี่ชั้นต้องปิดด้วย **`fi`** (ไม่ใช่ `if`) ให้ครบตามจำนวนจากชั้นในออกชั้นนอก — หากไม่จำเป็นต้องแยกข้อความ Error คนละเคส ให้ยุบด้วย `&&` หรือใช้ `exit 1` ดักไว้ก่อนเพื่อลดความลึกของโค้ด

---

## Module 05: Loops & Automation Iteration
- **Status:** ✅ Completed
- **Core Concept:** การวนซ้ำ: `while`, `until`, `for` loop และการอ่านข้อมูลจากไฟล์ทีละบรรทัดสำหรับงานสแกนเป้าหมายจำนวนมาก

### 💻 Essential Commands
```bash
# ตรวจสอบว่า Port ปลายทางเปิดอยู่หรือไม่ด้วย Netcat (ใช้บ่อยคู่กับ until/if)
nc -z -w 1 10.10.10.5 80 2>/dev/null    # -z สแกนไม่ส่งข้อมูล, -w 1 รอไม่เกิน 1 วินาที, 2>/dev/null ทิ้ง Error

# สร้างชุดลำดับตัวเลขสำหรับ Loop (หรือใช้ Brace Expansion {1..254})
seq 1 254                       # สร้างเลข 1-254 (รองรับตัวแปร เช่น seq 1 "$MAX" และใช้ใน #!/bin/sh ได้)
seq 1 2 10                      # นับทีละ 2 (1, 3, 5, 7, 9)
seq -w 1 10                     # เติม 0 นำหน้าให้หลักเท่ากัน (01..10)

# ควบคุมการทำงานภายใน Loop
continue                        # ข้ามรอบปัจจุบันไปเริ่มรอบถัดไปทันที
break                           # หยุดและออกจาก Loop ทันที
```

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

### 💻 Practical Code: `while`, `until`, `for` & File Line-by-Line Patterns
```bash
#!/bin/bash

# 1. while loop: วนซ้ำ "ตราบใดที่" เงื่อนไขเป็นจริง (True)
count=1
while (( count <= 5 )); do
    echo "[+] Attempt #$count"
    ((count++))
done

# 2. until loop: วนซ้ำ "จนกว่า" เงื่อนไขจะเป็นจริง (ตรงข้ามกับ while — เหมาะกับการรอ Service เปิด)
until nc -z -w 1 10.10.10.5 80 2>/dev/null; do
    echo "[*] Waiting for target port 80..."
    sleep 2
done
echo "[+] Port 80 is OPEN!"

# 3. for loop: วนตามรายการค่าที่กำหนด หรือช่วงตัวเลข {start..end}
for port in 21 22 80 443 445 3389; do
    echo "[*] Checking port $port..."
done

for i in {1..5}; do
    echo "[+] Iteration $i"
done

# 4. อ่านไฟล์ทีละบรรทัดแบบพื้นฐาน (นับเลขบรรทัดแสดงผล)
x=1
while read -r line; do
    echo "Line $x: $line"
    (( x ++ ))
done < targets.txt

# 5. อ่านไฟล์เป้าหมายทีละบรรทัดแบบ Production (เก็บช่องว่างครบ + ข้ามบรรทัดว่าง/Comment)
TARGET_FILE="targets.txt"
while IFS= read -r target; do
    [[ -z "$target" || "$target" == \#* ]] && continue    # ข้ามบรรทัดว่างและบรรทัดที่ขึ้นต้นด้วย #
    echo "[*] Scanning $target..."
    nmap -F "$target"
done < "$TARGET_FILE"

# 6. ตรวจสอบสถานะหลายเป้าหมายด้วย for + if ping (เช็ค Exit Code โดยตรงและทิ้ง Output ลง /dev/null)
for host in 10.10.10.1 10.10.10.2 google.com; do
    if ping -q -c 2 -W 1 "$host" > /dev/null 2>&1; then
        echo "[+] $host is UP"
    else
        echo "[-] $host is DOWN"
    fi
done

# 7. break vs continue: ควบคุมจังหวะ Loop (continue = ข้ามรอบนี้, break = ออกจาก Loop ทันที)
for port in 21 22 80 443 8080; do
    if [[ $port -eq 22 ]]; then
        continue                # ข้ามรอบนี้ (Skip port 22) ไม่ทำคำสั่งด้านล่างแล้วไปพอร์ตถัดไปทันที
    fi
    if nc -z -w 1 10.10.10.5 "$port" 2>/dev/null; then
        echo "[+] Found open port: $port — stopping scan."
        break                   # เจอพอร์ตเปิดแล้วหยุด Loop ทันที (ถ้า Loop ซ้อน Loop ใช้ break 2 ได้)
    fi
done
```

### 🧠 Feynman Takeaway & Reflection
#### 💡 แก่นความคิดและกลไกสำคัญ (Core Mental Models)
1. **แกะทีละชิ้น — กลไกอ่านไฟล์ทีละบรรทัด (`while IFS= read -r target ... done < "$TARGET_FILE"`):**
   - **`done < "$TARGET_FILE"`:** สั่งให้ Bash เปิดไฟล์ `targets.txt` ค้างไว้ แล้วป้อนข้อมูล **ทีละบรรทัดเข้าทาง Standard Input (`stdin`)** ของ Loop `while` — เมื่ออ่านครบถึงท้ายไฟล์ `read` จะคืนค่า False ทำให้ Loop จบเองอัตโนมัติ
   - **`IFS=` (Internal Field Separator ว่าง):** ปกติ `read` จะตัดเว้นวรรคหัว-ท้ายบรรทัดทิ้ง การตั้ง `IFS=` ไว้หน้า `read` (มีผลเฉพาะบรรทัดนี้) คือการสั่งให้เก็บข้อความทั้งบรรทัดตามจริง
   - **`read -r target` (`-r` = Raw):** หยิบ **1 บรรทัด** ที่ส่งเข้ามาจาก `done < "$TARGET_FILE"` มาเก็บไว้ในตัวแปร **`target`** เพื่อใช้ในรอบนั้นๆ (และเขียนทับค่าเดิมในรอบถัดไป; เปลี่ยนชื่อเป็น `ip` หรือ `line` ก็ได้) ส่วน `-r` ป้องกันไม่ให้ `read` กินเครื่องหมาย Backslash (`\`)
   - **`[[ -z "$target" || "$target" == \#* ]] && continue`:** ถ้าเจอ **บรรทัดว่าง (`-z`)** หรือ **บรรทัดที่ขึ้นต้นด้วย `#` (`\#*` — ต้องใส่ `\` หน้า `#` เพื่อไม่ให้ Bash คิดว่าเป็น Comment)** ให้สั่ง **`continue`** เพื่อกระโดดข้ามไปอ่านบรรทัดถัดไปทันทีโดยไม่รัน `nmap`
2. **กลไกคำสั่ง Netcat ใน Loop (`nc -z -w 1 10.10.10.5 80 2>/dev/null`):**
   - **`nc` (Netcat):** เครื่องมืออเนกประสงค์ด้าน Network — **`-z`** (Zero-I/O mode) ใช้เช็คแค่ว่า Port เปิดไหมโดยไม่ส่งข้อมูล, **`-w 1`** ตั้ง Timeout 1 วินาที แล้วให้ `until` ตรวจสอบจาก Exit Code (`0` = Port เปิดแล้ว → ออกจาก Loop)
3. **`seq` vs `{1..N}` (การสร้างลำดับตัวเลข):**
   - **`{1..254}` (Brace Expansion):** ทำงานใน Bash โดยตรง (เร็วกว่า) **แต่ใส่ตัวแปรข้างในไม่ได้** (`{1..$MAX}` จะไม่ทำงาน)
   - **`$(seq 1 "$MAX")`:** เรียกคำสั่ง `seq` (Sequence) จึง **ใส่ตัวแปรได้** และใช้ได้ใน `#!/bin/sh`
4. **การกำหนดเงื่อนไขใน `while` / `until` / `for` เทียบกับ `if`:**
   - **`while` และ `until` ใช้เงื่อนไขเหมือน `if` ทุกอย่าง 100%:** รับได้ทั้ง **`(( ... ))`** (ตัวเลขล้วน), **`[[ ... ]]`** (String/File), **`[ ... ]`** (POSIX), รวมถึง **คำสั่ง Linux โดยตรง** (เช่น `until nc ...;` หรือ `while read ...;` โดยไม่ต้องมีวงเล็บ — เช็คจาก Exit Code `0` = True)
   - **`for`:** ปกติใช้ `for x in <รายการ>; do` (ไม่มีเงื่อนไข) ยกเว้นแบบ C-style ที่ใช้ **`for (( i=1; i<=5; i++ )); do`** (เฉพาะ `#!/bin/bash`)
5. **กลไก Redirection และ `/dev/null` (`> /dev/null`, `2>/dev/null`, `> /dev/null 2>&1`):**
   - ทุกคำสั่งใน Linux มีท่อแสดงผล 2 ท่อ: **ท่อ `1` (`stdout`)** = ผลลัพธ์ปกติ, **ท่อ `2` (`stderr`)** = ข้อความ Error และ **`/dev/null`** คือ **"หลุมดำ"** ที่ส่งอะไรเข้าไปก็จะหายวับไปเลย
   - **`> /dev/null`** *(ย่อมาจาก `1>/dev/null`)*: ทิ้งเฉพาะผลลัพธ์ปกติ (`1`) แต่ Error (`2`) ยังโชว์อยู่
   - **`2>/dev/null`**: ทิ้งเฉพาะ Error (`2`) แต่ผลลัพธ์ปกติ (`1`) ยังโชว์ (เช่น `find / -perm -4000 2>/dev/null` เพื่อซ่อน `Permission denied`)
   - **`> /dev/null 2>&1`** *(หรือ `&>/dev/null`)*: ส่ง `1` ลงหลุมดำ แล้วเอา `2` มาต่อเข้าปลายทางเดียวกับ `1` (`2>&1`) ทำให้ **เงียบสนิท 100% ทั้งผลปกติและ Error** แต่ `if` / `until` ยังตรวจ Exit Code ได้ตามปกติ

#### ⚠️ กับดักทางเทคนิคและข้อควรระวัง (Gotchas & Pitfalls)
1. **กับดักใช้ `for line in $(cat file)` แทน `while IFS= read -r`:** การใช้ `for` กับ `$(cat ...)` จะโหลดทั้งไฟล์เข้าหน่วยความจำทีเดียว **และหั่นข้อความทุกจุดที่มีเว้นวรรค** (เช่น `"10.10.10.1 web"` จะถูกแยกเป็น 2 รอบ) — การใช้ `while IFS= read -r` จึงเป็นมาตรฐานเดียวที่อ่านทีละบรรทัดได้ถูกต้องและประหยัด RAM
2. **กับดักไฟล์ที่ไม่มี Newline ท้ายบรรทัดสุดท้าย:** หากบรรทัดสุดท้ายของไฟล์ไม่มีการกด Enter ปิดท้าย (No trailing newline) คำสั่ง `read` จะอ่านข้อความใส่ `target` ได้แต่คืนค่า False ทันทีเพราะชนจบไฟล์ (EOF) ทำให้ Loop จบก่อนประมวลผล IP/เป้าหมายตัวสุดท้าย — แก้ได้โดยเขียนเป็น `while IFS= read -r target || [[ -n "$target" ]]; do` (เป็นการเช็คแบบ `A || B`: แม้ `read` คืนค่า False แต่ `[[ -n "$target" ]]` เห็นว่ายังมีข้อความอยู่จึงคืนค่า True ให้ทำรอบสุดท้ายจนครบ)
3. **กับดักเขียน `count++` เปล่าๆ โดยไม่มี `(( ... ))`:** หากเขียน `count++` ลอยๆ ในสคริปต์ Bash จะเข้าใจว่าเป็นชื่อคำสั่งแล้วพังด้วย `count++: command not found` ต้องครอบด้วย **`((count++))`** เพื่อเข้าสู่ Arithmetic Mode (หรือใช้ `count=$(( count + 1 ))` หากต้องการให้รันบน POSIX `#!/bin/sh` ได้ด้วย)
