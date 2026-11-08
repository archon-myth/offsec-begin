# Lab Environment Setup: Kali Linux & Windows 11 (VirtualBox)

> **Repository:** `A:\offsec-begin` | **Vault:** Obsidian  
> **Master Reference:** [A:\ARCHONOS\drop\setup\setup-kali-linux-baseline.md](file:///A:/ARCHONOS/drop/setup/setup-kali-linux-baseline.md)  
> **Host OS:** Windows 11 | **Hypervisor:** Oracle VM VirtualBox 7.x  
> **Lab Storage:** `E:\Active_Labs\` (MYTH_ORIGIN) | **Engine & Tools:** `D:\` (ARCHON_CORE)

---

## 🏗️ Architecture & Storage Layout

| Component | Host Location | Specifications / Role |
| :--- | :--- | :--- |
| **Kali Linux VM** | `E:\Active_Labs\Kali_Linux\` | Offensive OS · 4 GB RAM · 2 Cores · 80 GB VDI · XFCE |
| **Windows 11 VM** | `E:\Active_Labs\Windows_11\` | Target / Windows Analysis · 8 GB RAM · 4 Cores · 100 GB VDI |
| **Archon Core** | `D:\Engine\`, `D:\Security\` | Portable scripts, VeraCrypt, KeePassXC, Wordlists |
| **ISO Repository** | `D:\Boot_Images\` | Kali, Windows 11, Live forensic images |

---

# PART 1: Kali Linux Baseline Setup

### STEP 1: การสร้างและตั้งค่า Virtual Machine
1. เปิด **VirtualBox** $\rightarrow$ คลิก **New**
   - **Name:** `Kali_Linux`
   - **Folder:** `E:\Active_Labs\Kali_Linux`
   - **ISO Image:** เลือกไฟล์ Kali Linux Installer ISO จาก `D:\Boot_Images\`
   - **Type:** `Linux` | **Version:** `Debian (64-bit)`
   - ❌ **ไม่ติ๊ก** "Proceed with Unattended Installation" $\rightarrow$ กด **Next**
2. **Hardware Configuration (Settings):**
   - **System $\rightarrow$ Motherboard:** RAM **4096 MB** (4 GB) | Boot Order: Optical, Hard Disk
   - **System $\rightarrow$ Processor:** CPU **2 Cores** | ติ๊ก ✅ **Enable PAE/NX**
   - **Display $\rightarrow$ Screen:** Video Memory **128 MB** | Graphics Controller: `VMSVGA`
   - **Storage:** Hard Disk ขนาด **80 GB** (VDI, Dynamically allocated บนไดรฟ์ `E:\`)
   - **General $\rightarrow$ Advanced:**
     - Shared Clipboard: **Bidirectional**
     - Drag'n'Drop: **Bidirectional**

---

### STEP 2: การติดตั้งระบบปฏิบัติการ (OS Installation)
1. กด **Start** $\rightarrow$ เลือกเมนู **Graphical Install**
2. **Language:** English | **Location:** Thailand | **Keyboard:** American English
3. **Network Configuration:** Hostname: `kali` (หรือ `archon`) | Domain: ปล่อยว่าง
4. **User Setup:** ตั้ง Username และ Password ที่รัดกุม (ต้องจำได้เพราะใช้สำหรับ `sudo`)
5. **Partition Disks:**
   - เลือก **Guided - use entire disk** $\rightarrow$ เลือกดิสก์ขนาด 80GB
   - Scheme: **All files in one partition (recommended for new users)**
   - ยืนยัน **Finish partitioning and write changes to disk** $\rightarrow$ ตอบ **Yes**
6. **Software Selection:**
   - Desktop environment: ✅ **XFCE** (แนะนำสำหรับ VM — เบา เสถียรที่สุด)
   - Tools: ✅ **default -- recommended tools**
7. **GRUB Boot Loader:**
   - "Install GRUB boot loader to your primary drive?" $\rightarrow$ ตอบ **Yes**
   - Device: เลือก **/dev/sda**
8. เมื่อเสร็จสิ้น เลือก **Continue** เพื่อ Reboot เข้าสู่ Kali Linux

---

### STEP 3: แก้ไข APT Repository & Mirror (แก้ปัญหา 403 Forbidden / Hash Sum Mismatch)
> ⚠️ **ปัญหาพบบ่อย:** ค่าเริ่มต้นของ Kali อาจสุ่มเชื่อมต่อไปยัง Mirror ในไทย เช่น `mirror.kku.ac.th` ซึ่งมักเกิดข้อผิดพลาด `403 Forbidden` หรือ `Hash Sum mismatch`

แก้ไขโดยการล้างแคชและล็อคชี้ไปยัง Mirror หลักของ Kali ผ่าน HTTPS:

```bash
# 1. ล้างรายการแคชที่เสียหาย
sudo rm -rf /var/lib/apt/lists/*
sudo rm -rf /var/cache/apt/archives/partial/*
sudo apt clean

# 2. บังคับชี้ไปยัง Official HTTPS Mirror
echo "deb https://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware" | sudo tee /etc/apt/sources.list

# 3. อัปเดตรายการแพ็กเกจ
sudo apt update

# 4. อัปเกรดระบบทั้งหมด
sudo apt full-upgrade -y --fix-missing
```

---

### STEP 4: Guest Additions & การแก้ปัญหา Shared Clipboard (Copy ข้ามเครื่องไม่ได้)

#### 1. ติดตั้ง Guest Integration Tools
```bash
sudo apt install -y virtualbox-guest-x11 keepassxc
```

#### 2. แก้ปัญหา Shared Clipboard & Drag-and-Drop ไม่ทำงาน
ใน VirtualBox 7.x คำสั่งรุ่นเก่าอย่าง `VBoxClient-all` ถูกยกเลิก หาก Copy ข้ามระหว่าง Windows กับ Kali ไม่ได้ ให้ตรวจสอบและแก้ไขตามนี้:

**ก. ตรวจสอบการตั้งค่าบน Host (Windows):**
- ขณะ VM ปิดอยู่ หรือรันอยู่ ให้สั่งผ่าน PowerShell / CMD:
  ```powershell
  # รันคำสั่งนี้บน Windows Host:
  & "C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" controlvm "Kali_Linux" clipboard mode bidirectional
  & "C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" controlvm "Kali_Linux" draganddrop bidirectional
  ```

**ข. สั่งรัน Daemon ภายใน Kali Linux:**
```bash
# รันใน Kali Terminal เพื่อสตาร์ท Service:
killall VBoxClient 2>/dev/null
VBoxClient --clipboard
VBoxClient --draganddrop
```
*(ระบบจะตอบกลับว่า `Service: Shared Clipboard` และ `Daemonizing service ...`)*

**ค. กฎการ Paste ใน Terminal (สำคัญ):**
- **ฝั่ง Windows:** ก๊อบปี้ด้วย `Ctrl + C`
- **ฝั่ง Kali Terminal:** วางด้วย **`Ctrl + Shift + V`** หรือ **คลิกขวา $\rightarrow$ Paste** (การกด `Ctrl + V` เฉยๆ ใน Terminal ของ Linux จะไม่ทำงาน)

---

### STEP 5: ปรับแต่ง Zsh History & Terminal OPSEC

เพื่อไม่ให้ประวัติคำสั่งสูญหายเมื่อเปิด Terminal หลายแท็บ และรองรับเทคนิคซ่อนคำสั่ง:

แก้ไขไฟล์ `~/.zshrc`:
```bash
# เพิ่มการตั้งค่าท้ายไฟล์ ~/.zshrc
HISTFILE=~/.zsh_history
HISTSIZE=10000
SAVEHIST=10000
setopt hist_expire_dups_first # ตัดคำสั่งซ้ำเมื่อประวัติเต็ม
setopt hist_ignore_dups       # ไม่บันทึกคำสั่งที่พิมพ์ซ้ำติดกัน
setopt hist_ignore_space      # OPSEC: เคาะเว้นวรรค 1 ครั้งหน้าคำสั่ง จะไม่ถูกบันทึกลง history
setopt hist_verify            # ตรวจสอบก่อนขยาย history
setopt inc_append_history     # เขียนคำสั่งลงดิสก์ทันทีที่กด Enter
setopt share_history          # แชร์ประวัติคำสั่งข้ามทุกแท็บ Terminal ทันที
```

ติดตั้งตัวช่วยพิมพ์คำสั่ง (Auto-suggestions & Command-not-found):
```bash
sudo apt install -y zsh-autosuggestions command-not-found
source ~/.zshrc
```
*(เวลามีคำสั่งแนะนำสีเทาปรากฏ ให้กดปุ่ม **ลูกศรขวา ($\rightarrow$)** หรือปุ่ม **End** เพื่อเติมคำสั่งอัตโนมัติ)*

---

### STEP 6: Shared Folders & Archon Engine Launch (`/mnt`)

เชื่อมต่อไดรฟ์พกพา `D:\` (ARCHON_CORE) เข้ากับ Kali:
1. VirtualBox Menu: **Devices $\rightarrow$ Shared Folders $\rightarrow$ Shared Folders Settings...**
   - Folder Path: `D:\`
   - Folder Name: `D_DRIVE` (หรือตั้ง Auto-mount)
   - Access: Read/Write
2. เพิ่มสิทธิ์ User ใน Kali:
   ```bash
   sudo adduser $USER vboxsf
   ```
3. สั่งรันสคริปต์เชื่อมต่อ Archon Engine:
   ```bash
   cd /mnt/*/Engine
   bash archon_launch.sh
   source ~/.zshrc
   ```

**Shortcuts ที่ใช้งานได้หลังรันสคริปต์:**
- `engine` $\rightarrow$ สลับไปยังโฟลเดอร์ `/mnt/.../Engine`
- `security` $\rightarrow$ สลับไปยัง `/mnt/.../Security` (Tools & Wordlists)
- `tools` $\rightarrow$ สลับไปยัง `/mnt/.../Security/Tools` (VeraCrypt, KeePassXC)
- `isos` $\rightarrow$ สลับไปยัง `/mnt/.../Boot_Images`
- `fix-time` $\rightarrow$ ซิงค์เวลากับ Hardware RTC ของ Host Windows
- `update` $\rightarrow$ รันอัปเดตระบบอัตโนมัติ

---

### STEP 7: การเข้าถึง Kali ผ่าน SSH จาก Host (Optional)

1. ตั้งค่า Port Forwarding ใน VirtualBox (Settings $\rightarrow$ Network $\rightarrow$ Advanced $\rightarrow$ Port Forwarding):
   - Name: `SSH` | Protocol: `TCP` | Host IP: `127.0.0.1` | Host Port: `2222` | Guest Port: `22`
2. เปิด Service ใน Kali:
   ```bash
   sudo systemctl enable --now ssh
   ```
3. เชื่อมต่อจาก Windows PowerShell:
   ```powershell
   ssh -p 2222 <username>@127.0.0.1
   ```

---

### STEP 8: ขั้นตอนคลีนระบบและสร้าง Golden Baseline Snapshot

ก่อนสร้าง Snapshot เก็บสถานะเครื่อง ให้ทำความสะอาดไฟล์ขยะเพื่อให้ขนาดไฟล์ Delta Snapshot เล็กที่สุด:

```bash
# 1. ทำความสะอาดแคชของ APT
sudo apt autoremove -y && sudo apt clean

# 2. ล้างไฟล์ชั่วคราวและ System Journal
sudo rm -rf /tmp/* /var/tmp/*
sudo journalctl --vacuum-time=1d

# 3. ล้างประวัติ Shell ให้เป็นศูนย์
cat /dev/null > ~/.zsh_history

# 4. สั่ง Shutdown เครื่องอย่างสมบูรณ์ (ห้าม Snapshot ตอนเปิดเครื่องอยู่)
sudo poweroff
```

**สร้าง Snapshot ใน VirtualBox:**
1. ตรวจสอบว่าสถานะของ `Kali_Linux` เป็น **Powered Off**
2. ไปที่แท็บ **Snapshots** ของ VirtualBox
3. กด **Take Snapshot**:
   - **Name:** `Golden_Base_Clean`
   - **Description:** `Kali baseline fully updated, Guest Additions active, clipboard bidirectional, Zsh history optimized, APT cache purged.`

---

# PART 2: Windows 11 Target Lab Setup

### STEP 1: การสร้าง VM Windows 11
1. VirtualBox $\rightarrow$ กด **New**
   - **Name:** `Windows11_Lab`
   - **Folder:** `E:\Active_Labs\Windows_11`
   - **ISO Image:** เลือกไฟล์ Windows 11 ISO จาก `D:\Boot_Images\`
   - **Type:** `Microsoft Windows` | **Version:** `Windows 11 (64-bit)`
   - ตรวจสอบให้เปิดใช้งาน **TPM 2.0** และ **Secure Boot**
2. **Hardware Configuration:**
   - **RAM:** **8192 MB** (8 GB) ขึ้นไป
   - **CPU:** **4 Cores** ขึ้นไป
   - **Storage:** Hard Disk ขนาด **100 GB** (VDI, Dynamically allocated บน `E:\Active_Labs\`)

### STEP 2: การติดตั้ง OS & Guest Additions
1. กด **Start** และดำเนินการติดตั้ง Windows 11 ตามขั้นตอนปกติ
2. ปิด Telemetry และฟังก์ชันที่ไม่จำเป็นสำหรับ Lab
3. ไปที่เมนู VirtualBox: **Devices $\rightarrow$ Insert Guest Additions CD image...**
4. เปิด File Explorer ใน Windows 11 VM $\rightarrow$ ดับเบิลคลิก `VBoxWindowsAdditions.exe` ติดตั้งให้เสร็จสิ้น
5. สั่ง Restart VM หนึ่งครั้ง เพื่อให้ระบบ Resizing จอภาพและ Shared Clipboard ทำงานได้อย่างสมบูรณ์
6. ทำการ Shutdown แล้วกดสร้าง Snapshot: `Windows11_Clean_Base`
