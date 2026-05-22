# 🛡️ Automated Intrusion Detection and Response System (SIEM/SOAR)

![Wazuh](https://img.shields.io/badge/Wazuh-SIEM-blue)
![Ansible](https://img.shields.io/badge/Ansible-Automation-red)
![Linux](https://img.shields.io/badge/Linux-Environment-yellow)
![Bash](https://img.shields.io/badge/Bash-Scripting-green)

A centralized **SIEM and SOAR** architecture built on Linux, utilizing **Wazuh** for real-time threat detection and **Ansible** for automated incident response. This system mitigates cyber attacks (Global IP Blocking) within 1-3 seconds.

---

## 🎯 Key Features & Engineering Solutions
- **12 Security Use Cases Detected:** Successfully identifies and stops attacks across Network and Application layers (e.g., Nmap Stealth Scan, SQL Injection, XSS, SSH Brute Force, LFI).
- **Automated Mitigation (Global Block):** Uses Ansible playbooks to inject `iptables DROP` rules across multiple servers simultaneously.
- **Anti-Spam Cache Mechanism:** A custom Bash script layer that filters duplicate alerts during high-volume attacks (e.g., Nikto Scans), preventing Race Conditions and reducing Manager CPU load.
- **Network Sensor Tuning:** Overcame standard application log limitations by implementing `iptables LOG` to catch stealthy SYN Scans.
- **Real-Time Notification:** Integrated Telegram Bot API for instant incident reporting (Attack type, Attacker IP, Mitigation status).

---

## 🏗️ System Architecture (3-Layer Pipeline)

*(Insert your Architecture Diagram Image Here)*
`![System Architecture](link_to_your_image.png)`

1. **Detection Layer:** Wazuh Agents collect system logs, `auth.log`, `access.log`, and `iptables` logs.
2. **Analysis Layer:** Wazuh Manager decodes logs, matches them against 3,000+ default rules and Custom XML Rules, and triggers an Active Response for Level 10 alerts.
3. **Response Layer:** The gateway Bash script validates whitelists, checks the Anti-Spam Cache, and executes the Ansible Playbook to block the threat.

---

## 📁 Repository Contents
- `wazuh-ansible.sh`: The gateway script handling whitelist, caching, Ansible execution, and Telegram notifications.
- `block_attacker_global.yml`: Ansible Infrastructure-as-Code (IaC) playbook for iptables manipulation.
- `local_rules.xml`: Custom Wazuh detection rules (e.g., Rule 100021 for Port Scan detection).

---

## 📺 Demonstration (Attack vs Response)

*(Insert a GIF or screenshot showing a side-by-side of an attack and the Telegram notification)*
`![Demo](link_to_your_gif_or_image.gif)`

---

## 📄 Full Project Documentation
For an in-depth analysis of the methodology, test results, and configuration steps, please refer to the full project report:

**[👉 Click Here to Read the Full PDF Report](ใส่ลิงก์_Google_Drive_ของไฟล์_PDF_ที่นี่)**
