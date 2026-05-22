# 🛡️ Automated Intrusion Detection and Response System (SIEM/SOAR)

![Wazuh](https://img.shields.io/badge/Wazuh-SIEM-blue)
![Ansible](https://img.shields.io/badge/Ansible-Automation-red)
![Linux](https://img.shields.io/badge/Linux-Environment-yellow)
![Bash](https://img.shields.io/badge/Bash-Scripting-green)

ระบบตรวจจับและตอบสนองต่อภัยคุกคามทางไซเบอร์แบบอัตโนมัติ (Automated Active Response) บนสภาพแวดล้อม Linux โดยการผสานการทำงานของ **Wazuh** (ทำหน้าที่วิเคราะห์และตรวจจับ) และ **Ansible** (ทำหน้าที่ควบคุมและสั่งการ) เพื่อระงับเหตุภัยคุกคามด้วยการบล็อก IP ผู้โจมตีภายใน 1-3 วินาที

---

## 🎯 คุณสมบัติเด่น (Key Engineering Features)

- **Automated Mitigation (Global Block):** ควบคุมโครงสร้างพื้นฐานผ่าน Ansible Playbook (IaC) เพื่อแทรกกฎ `iptables DROP` ระงับการโจมตีได้ทันที
- **Anti-Spam Cache Mechanism:** พัฒนา Bash Script เป็นด่านหน้าเพื่อคัดกรอง Request ที่ซ้ำซ้อนระดับมิลลิวินาที ป้องกันเซิร์ฟเวอร์เกิดสภาวะ Race Condition จากการถูกสแกนปริมาณมหาศาล
- **Network Sensor Tuning:** ประยุกต์ใช้กฎ `iptables LOG` ร่วมกับ Custom Rules เพื่ออุดช่องโหว่การตรวจจับระดับ Application ทำให้ระบบสามารถดักจับ Stealth Port Scan ได้

---

## 🔒 ผลการทดสอบภัยคุกคาม (Tested Security Use Cases)

ระบบสามารถตรวจจับและตอบสนองต่อการโจมตีครอบคลุมทั้ง Network Layer และ Application Layer ได้สำเร็จ 100% ดังนี้:
1. **SSH Brute Force Attack:** ตรวจจับการโจมตีรหัสผ่านซ้ำๆ 
2. **Stealth Port Scanning (Nmap -sS):** ตรวจจับการสแกนพอร์ตแบบซ่อนตัว (SYN Scan)
3. **SQL Injection (SQLi):** ตรวจจับ Payload อันตราย (เช่น `UNION SELECT`) แม้เซิร์ฟเวอร์ตอบกลับ 200 OK
4. **Cross-Site Scripting (XSS):** ตรวจจับการฝังโค้ดอันตรายผ่าน URL Parameter
5. **Directory Traversal:** ตรวจจับพฤติกรรมการพยายามเข้าถึงไฟล์ระบบ (`../../../../etc/passwd`)
6. **Web Vulnerability Scanning (Nikto):** ยกระดับ Alert ผ่าน Correlation Rules เมื่อมีการสแกนหาช่องโหว่ปริมาณมาก

---

## 🏗️ สถาปัตยกรรมและตรรกะการทำงาน (Architecture & Methodology)

### 1. Network Topology (ภาพรวมโครงสร้างเครือข่าย)
แผนผังเครือข่ายจำลอง แสดงตำแหน่งของ Attacker, Wazuh Agent (เป้าหมาย) และ Wazuh Manager (ศูนย์กลาง) พร้อมช่องทางการสื่อสารและการสั่งการ
<p align="center">
  <img src="image_2.jpg" alt="Network Topology and Attack Flow Diagram" width="90%" />
</p>

### 2. Active Response Logic Flow (ตรรกะการตอบสนองอัตโนมัติ)
ผังงานแสดงกระบวนการตัดสินใจ (Decision Making) ของระบบ ตั้งแต่การวิเคราะห์ Rule ID, การตรวจสอบผ่านระบบ Safeguard (Whitelist & Cache) ไปจนถึงการเรียกใช้งาน Ansible Playbook
<p align="center">
  <img src="image_1.jpg" alt="Active Response Logic Flowchart" width="70%" />
</p>

---

## 📁 โครงสร้างไฟล์สำคัญ (Repository Contents)

- `wazuh-ansible.sh`: สคริปต์ Bash ด่านหน้าสำหรับควบคุม Whitelist, Anti-Spam Cache และส่งตัวแปรให้ Ansible
- `block_attacker_global.yml`: ไฟล์ Ansible Playbook สำหรับจัดการตาราง iptables แบบอัตโนมัติ
- `local_rules.xml`: Custom Rules ภาษา XML ที่เขียนเพิ่มเพื่อดักจับ Network-based attacks (เช่น Port Scan)

---

## 📄 เอกสารโครงงานฉบับเต็ม (Full Project Documentation)

สำหรับรายละเอียดการตั้งค่าเชิงลึก (Configuration), โค้ดฉบับเต็ม และผลการทดลองอย่างละเอียด สามารถอ่านได้จากเล่มรายงานฉบับสมบูรณ์ด้านล่างนี้:

**[👉 คลิกที่นี่เพื่อเปิดอ่านเล่มรายงาน (PDF) ](https://drive.google.com/file/d/1tl0GeAQdkV-ZeD1jXlgXxEYWOjIA3o0G/view?usp=drive_link)**
