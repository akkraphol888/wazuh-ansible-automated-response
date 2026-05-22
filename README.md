# 🛡️ Automated Intrusion Detection and Response System (SIEM/SOAR)

![Wazuh](https://img.shields.io/badge/Wazuh-SIEM-blue)
![Ansible](https://img.shields.io/badge/Ansible-Automation-red)
![Linux](https://img.shields.io/badge/Linux-Environment-yellow)
![Bash](https://img.shields.io/badge/Bash-Scripting-green)

ระบบตรวจจับและตอบสนองต่อภัยคุกคามอัตโนมัติ (SIEM/SOAR) บนระบบปฏิบัติการ Linux โดยใช้ **Wazuh** ในการวิเคราะห์ Log และตรวจจับความผิดปกติร่วมกับ **Ansible** ในการควบคุมโครงสร้างพื้นฐานเพื่อระงับเหตุ (Active Response) ด้วยการบล็อก IP ผู้โจมตีผ่าน iptables ภายใน 1-3 วินาที

---

## 🎯 คุณสมบัติหลักของระบบ (Key Features)
- **Multi-Vector Threat Detection:** รองรับการตรวจจับการโจมตีทั้งในระดับ Network Layer และ Application Layer
- **Automated Mitigation (Global Block):** สั่งการผ่าน Ansible Playbook เพื่อแทรกกฎ `iptables DROP` บล็อก IP ผู้โจมตีทันทีเมื่อความรุนแรงถึงเกณฑ์ที่กำหนด
- **Anti-Spam Cache Mechanism:** พัฒนาสคริปต์ด่านหน้าคอยกรอง Request ที่ซ้ำซ้อนในระดับมิลลิวินาที เพื่อป้องกันเสถียรภาพของเซิร์ฟเวอร์ ไม่ให้เกิดสภาวะ Race Condition ตอนโดนสแกนหนักๆ
- **Network Sensor Tuning:** ประยุกต์ใช้กฎ `iptables LOG` ดักจับเฉพาะแพ็กเก็ตที่เป็น SYN Flag เพื่อส่งต่อให้ Wazuh วิเคราะห์ ทำให้สามารถตรวจจับ Stealth Port Scan ได้สำเร็จ

---

## 🔒 ผลการทดสอบการตรวจจับ (Tested Use Cases)
ระบบผ่านการทดสอบและสามารถตอบสนองต่อภัยคุกคามหลักได้สำเร็จ 100% ดังนี้:
1. **SSH Brute Force Attack:** ตรวจจับการเดารหัสผ่านผิดซ้ำๆ ผ่าน `auth.log` และบล็อก IP อัตโนมัติ
2. **Stealth Port Scanning (Nmap -sS):** ตรวจจับการสแกนพอร์ตแบบซ่อนตัวผ่าน Custom Rule ระดับ Network
3. **SQL Injection (SQLi):** ตรวจจับ Payload อันตราย (เช่น `UNION SELECT`) ผ่าน Apache Access Log แม้เซิร์ฟเวอร์จะตอบกลับเป็น HTTP 200 OK
4. **Cross-Site Scripting (XSS):** ตรวจจับการส่งแท็กสคริปต์อันตรายผ่าน URL Parameter
5. **Directory Traversal:** ตรวจจับพฤติกรรมการพยายามเข้าถึงไฟล์ระบบ (`../../../../etc/passwd`)
6. **Web Vulnerability Scanning (Nikto):** ตรวจจับการยิงสแกนช่องโหว่เว็บปริมาณมหาศาล ยกระดับ Alert เป็น Level 10 ด้วย Correlation Rules

---

## 🏗️ สถาปัตยกรรมของระบบ (3-Layer Pipeline)

*(ใสรูปรูปแผนภาพ Topology/Architecture ตรงนี้)*
`![System Architecture](ชื่อไฟล์รูปภาพของคุณ.png)`

1. **Detection Layer:** เครื่อง Agent รวบรวมและส่ง Log (`auth.log`, `access.log`, `syslog`) ไปยัง Manager ผ่านพอร์ต 1514 (AES-256)
2. **Analysis Layer:** Wazuh Manager ถอดรหัส Log นำไปเทียบกับ Rule และสั่งการ Active Response เมื่อพบภัยคุกคามระดับสูง
3. **Response Layer:** สคริปต์ Bash ทำหน้าที่ตรวจสอบ Whitelist, เช็ค Cache และเรียกใช้งาน Ansible Playbook เพื่อวิ่งไปบล็อก IP บนเซิร์ฟเวอร์เป้าหมาย

---

## 📁 โครงสร้างไฟล์ในคลังข้อมูล (Repository Contents)
- `wazuh-ansible.sh`: สคริปต์ Bash ด่านหน้าสำหรับควบคุม Whitelist, Anti-Spam Cache และเรียกใช้ Ansible
- `block_attacker_global.yml`: ไฟล์ Ansible Playbook (Infrastructure as Code) สำหรับจัดการตาราง iptables
- `local_rules.xml`: Custom Rules ภาษา XML ที่เขียนเพิ่มบน Wazuh Manager เพื่อดักจับ Port Scan

---

## 📄 เอกสารโครงงานฉบับเต็ม (Project Documentation)
สามารถกดอ่านรายละเอียดการตั้งค่าเชิงลึก สคริปต์ทั้งหมด และผลการทดลองอย่างละเอียดได้ที่ลิงก์ด้านล่างนี้:

**[👉 คลิกที่นี่เพื่ออ่านเล่มรายงานฉบับเต็ม (PDF) ]([ใส่ลิงก์_Google_Drive_ของไฟล์_PDF_ตรงนี้](https://drive.google.com/file/d/1tl0GeAQdkV-ZeD1jXlgXxEYWOjIA3o0G/view?usp=sharing))**
