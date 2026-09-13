# คู่มือ PJZB Server Manager

คู่มือนี้ใช้กับ **PJZB Server Manager v1.6.0 ขึ้นไป** สำหรับ Project Zomboid Dedicated Server Build 42 บน Windows 10/11 64-bit

## สารบัญ

1. [ดาวน์โหลดและเริ่มใช้งาน](#ดาวน์โหลดและเริ่มใช้งาน)
2. [First-Time Setup](#first-time-setup)
3. [นำเซิร์ฟเวอร์เดิมมาใช้](#นำเซิร์ฟเวอร์เดิมมาใช้)
4. [เลือกวิธีเชื่อมต่อ](#เลือกวิธีเชื่อมต่อ)
5. [เปิดและปิดเซิร์ฟเวอร์](#เปิดและปิดเซิร์ฟเวอร์)
6. [Configuration และ Sandbox](#configuration-และ-sandbox)
7. [Mods และ Workshop](#mods-และ-workshop)
8. [Players, Backups และ Updates](#players-backups-และ-updates)
9. [Launcher Settings](#launcher-settings)
10. [แก้ปัญหาที่พบบ่อย](#แก้ปัญหาที่พบบ่อย)

## ดาวน์โหลดและเริ่มใช้งาน

1. เปิดหน้า [Releases](https://github.com/natthawutGitZ/PJZB-Server-Manager/releases/latest)
2. ดาวน์โหลด `PJZB.Server.Manager.exe`
3. วางไฟล์ในโฟลเดอร์ที่ต้องการ เช่น `C:\PJZB`
4. เปิดโปรแกรม ไม่จำเป็นต้องติดตั้ง .NET เพิ่ม

หาก Windows SmartScreen เตือน ให้ตรวจสอบว่าดาวน์โหลดจาก Repository นี้ แล้วเลือก **More info → Run anyway**

> อย่าใช้รากไดรฟ์ เช่น `C:\` เป็น Server root ให้ใช้โฟลเดอร์เฉพาะ เช่น `C:\PJZB` หรือ `D:\Servers\PJZB`

## First-Time Setup

เมื่อ Launcher ยังไม่พบเซิร์ฟเวอร์ จะเปิดหน้า First-Time Setup แทน Dashboard

1. เลือก **Server installation path**
2. กำหนด Java RAM หรือใช้ค่าที่ระบบแนะนำ
3. เลือกตัวเลือก Auto Start ตามต้องการ
4. กด **FIRST-TIME SETUP**
5. เลือก Dedicated Server, Forward Port หรือ playit.gg

Launcher จะดาวน์โหลด SteamCMD, ติดตั้ง Dedicated Server, สร้าง Configuration, RCON/Admin password และโฟลเดอร์ `server`, `data`, `backups`, `logs`, `tools` ให้อัตโนมัติ

SteamCMD อาจใช้เวลาหลายนาที หากเครือข่ายสะดุด Launcher จะลองใหม่สูงสุด 4 ครั้งและเก็บไฟล์ที่ดาวน์โหลดแล้วไว้

## นำเซิร์ฟเวอร์เดิมมาใช้

กด **USE EXISTING SERVER** เมื่อมี Server root จาก PJZB อยู่แล้วและไม่ต้องการติดตั้งใหม่

```text
PJZB\
├─ server\
│  └─ jre64-pjzb\bin\java.exe  (หรือ jre64\bin\java.exe)
└─ data\
   └─ Server\PJZB.ini
```

Launcher จะตรวจ runtime และ Configuration แล้วใช้ world, saves, mods, database และข้อมูลผู้เล่นจากตำแหน่งเดิม ไม่คัดลอกหรือรีเซ็ตข้อมูล

> หากใช้ชื่ออื่น เช่น `servertest.ini` ต้องแปลงเป็นโครงสร้าง PJZB ก่อน ระบบไม่เปลี่ยนชื่อ world อัตโนมัติเพื่อป้องกันการเปิดเป็นโลกใหม่

## เลือกวิธีเชื่อมต่อ

### Dedicated Server

เหมาะกับ VPS, เครื่องเช่า หรือเครื่องแยกที่มี Public IP และเปิดตลอดเวลา

- อนุญาต UDP `16261–16262` ใน Firewall
- รองรับ Public Server Browser
- หากเครื่องยังอยู่หลังเราเตอร์บ้าน อาจต้อง Forward Port
- ใช้ priority และ Auto Update ที่เหมาะกับเครื่องเซิร์ฟเวอร์

### Forward Port

เหมาะกับเครื่องบ้าน/Gaming PC และให้ latency ต่ำที่สุด

- Forward UDP `16261–16262` จากเราเตอร์มายังเครื่องเซิร์ฟเวอร์
- อนุญาตพอร์ตเดียวกันใน Windows Firewall
- ผู้เล่นเชื่อมด้วย `Public IP:16261`
- เปิด Public Browser ได้ด้วย `Public=true`
- ใช้ไม่ได้หาก ISP เป็น CGNAT จนกว่าจะขอ Public IPv4

### playit.gg — No Port Forward

เหมาะเมื่อเปิดพอร์ตไม่ได้หรือติด CGNAT ผู้เล่นไม่ต้องลงโปรแกรมเพิ่ม

Launcher จะดาวน์โหลด/เปิด playit agent และเปิด Claim URL ให้ เจ้าของต้องทำครั้งแรก:

1. เข้าสู่ระบบหรือสมัคร [playit.gg](https://playit.gg/)
2. อนุมัติและ Claim agent
3. สร้าง Tunnel ชนิด **Project Zomboid**
4. นำ numeric IP, พอร์ตหลัก และ Server Address มากรอก
5. กด **SAVE & CONTINUE**

Launcher จะตั้ง `DefaultPort`, `UDPPort`, `Public=false`, `UPnP=false` ให้ ผู้เล่นเพิ่ม Address ในหน้า Join และปิด **Use Steam Relay**

- [วิดีโอสอน playit.gg กับ Project Zomboid](https://www.youtube.com/watch?v=ClPNlTIkYb0)
- [คู่มือภาพจาก playit.gg](https://playit.gg/support/project-zomboid-steam-dedicated-server/)

## เปิดและปิดเซิร์ฟเวอร์

- **START** — เปิดเซิร์ฟเวอร์
- **SAVE & STOP** — บันทึกโลกก่อนปิด ควรใช้แทนการ Kill process
- **RESTART** — บันทึก ปิด และเปิดใหม่
- **CHECK UPDATES** — ตรวจ Workshop/Server updates

Dashboard แสดงสถานะ จำนวนผู้เล่น CPU, Java RAM, IP/Port และ Live Log โดยแยกสี Error, Warning, Success, Network, Lua และ Info

## Configuration และ Sandbox

### Configuration

- **Quick Setup** แก้ชื่อเซิร์ฟเวอร์ จำนวนผู้เล่น ข้อความต้อนรับ PVP, PauseEmpty, Public Browser และ Open Account
- **All Server Settings** ค้นหา แบ่งหมวด และแก้ค่าทั้งหมดใน `PJZB.ini`
- กด **APPLY VALUE** แล้ว **SAVE ALL CHANGES**

### Sandbox

- ตัวกรอง **ALL**, **BASE GAME**, **MODS** แยกค่าตัวเกมและ Mod
- แก้ค่าใน `PJZB_SandboxVars.lua`
- กด **SAVE SANDBOX** หลังแก้ไข

ค่าหลายรายการเริ่มใช้หลัง Restart ควร Backup ก่อนเปลี่ยน Mods, Maps หรือ Sandbox จำนวนมาก

## Mods และ Workshop

หน้า **Mods** ใช้เปิด/ปิด Mod, จัดลำดับ, เปิด dependency, เพิ่ม Mod/Workshop ID และดาวน์โหลดอัปเดต

หน้า **Workshop** เปิด Steam Workshop ภายใน Launcher และเพิ่ม/ลบ Workshop item ปัจจุบันจากรายการเซิร์ฟเวอร์ได้

> หยุดเซิร์ฟเวอร์ก่อนดาวน์โหลดหรืออัปเดต Mod และตรวจ dependency ก่อนปิด Mod หลัก

## Players, Backups และ Updates

### Players

เมื่อ RCON ready สามารถดูผู้เล่นออนไลน์, เปลี่ยน Access Level, Kick/Ban/Unban และ Broadcast หากรายชื่อไม่ขึ้น ให้ตรวจสถานะ RCON แล้วกด Refresh

### Backups

- **CREATE BACKUP NOW** — สร้าง Backup
- **RESTORE SELECTED** — คืนข้อมูลจาก Backup (ต้องหยุดเซิร์ฟเวอร์)
- **DELETE** — ย้าย Backup ไป Recycle Bin

ก่อน Restore ระบบจะสร้าง Safety Backup ของข้อมูลปัจจุบัน

### Updates

ตั้ง Auto Update interval, initial delay, player warning และ retry cooldown ได้ การอัปเดตถูกบล็อกเมื่อมีผู้เล่นออนไลน์ ใช้ **UPDATE WHILE STOPPED** หลัง Save & Stop

## Launcher Settings

กดไอคอนเฟืองมุมขวาบนเพื่อ:

- เปลี่ยน Server root path
- เลือก Shared/Gaming PC หรือ Dedicated Server
- กำหนด Java RAM และ process priority
- ตั้ง Auto Start Server / Start with Windows
- ตรวจ Launcher Update
- คัดลอก Admin password

การอัปเดต Launcher จะไม่ปิด Java server ที่กำลังทำงาน

## แก้ปัญหาที่พบบ่อย

### SteamCMD ติด 0% หรือ Setup Failed

- ตรวจอินเทอร์เน็ตและพื้นที่ว่าง
- อนุญาต `steamcmd.exe` ผ่าน Antivirus/Firewall
- กด First-Time Setup ซ้ำที่ path เดิม ระบบจะดาวน์โหลดต่อ
- ใช้ **COPY ERROR** เพื่อเก็บรายละเอียด

### Online แต่ผู้เล่นเข้าไม่ได้

- Forward Port: ตรวจ UDP `16261–16262`, Firewall และ CGNAT
- playit.gg: ตรวจ Agent/Tunnel, ให้พอร์ตตรงกับ Configuration และปิด Use Steam Relay
- ตรวจว่าเกม/เซิร์ฟเวอร์ใช้ Build และ Mod version เดียวกัน

### Public Browser ไม่แสดง

- ตั้ง `Public=true` และ `PublicName`
- Public Browser เหมาะกับ Dedicated/Forward Port มากกว่า playit.gg
- การแสดงชื่อไม่รับประกันว่าพอร์ตเข้าถึงได้

### Launcher เปิดอยู่แต่ไม่เห็นหน้าต่าง

ปิด process `PJZB Server Manager.exe` ที่ซ้ำใน Task Manager แล้วเปิดใหม่ และอัปเดตเป็นรุ่นล่าสุด

## ความปลอดภัย

- อย่าเผยแพร่ Admin password, RCON password หรือ `manager-settings.json`
- ใช้ Password/Whitelist สำหรับ Private Server
- Backup ก่อนเปลี่ยน Mods, Maps หรือ Sandbox จำนวนมาก
- ใช้ **SAVE & STOP** ก่อนปิด Windows หรือ Restore

## ลิงก์สำคัญ

- [ดาวน์โหลดรุ่นล่าสุด](https://github.com/natthawutGitZ/PJZB-Server-Manager/releases/latest)
- [Project Zomboid Workshop](https://steamcommunity.com/app/108600/workshop/)
- [แจ้งปัญหา](https://github.com/natthawutGitZ/PJZB-Server-Manager/issues)

