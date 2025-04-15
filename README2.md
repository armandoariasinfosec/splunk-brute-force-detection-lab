# 🛡️ Brute Force Detection Lab (Windows | Splunk | Kali)

## Overview

This project simulates a brute force attack in a home lab using Splunk Enterprise to detect and visualize failed RDP login attempts on a Windows 10 virtual machine, with the attacker originating from Kali Linux.

> ⚠️ This is a controlled environment project designed for educational purposes only.

---

## 📌 Lab Setup Summary

| Component       | Purpose                               |
|----------------|----------------------------------------|
| Windows 10 VM   | Target system                          |
| Kali Linux VM   | Attack simulation using `rdesktop`     |
| Splunk SIEM     | Log ingestion, analysis, and alerting  |
| Splunk UF       | Installed on Windows to forward logs   |

---

## 📥 Deployment Instructions

### Step 1: Enable RDP on the Windows 10 Target
- Go to `System > Remote Desktop`
- Toggle “Enable Remote Desktop” to **ON**
- Note the internal IP using `ipconfig`

### Step 2: Configure Splunk Forwarder on Windows
- Download and install the **Universal Forwarder**
- Forward security logs to your Splunk SIEM (port `9997`)
- Ensure Splunk Enterprise shows incoming logs via:
  ```spl
  index=* sourcetype=WinEventLog:Security
  ```

### Step 3: Simulate Brute Force from Kali
Use `rdesktop` with a looped script:
```bash
rdesktop -u fakeuser -p wrongpass 192.168.x.x
```
Repeat this command 5–10 times with incorrect credentials.

### Step 4: Detection Query in Splunk
```spl
index=* sourcetype=WinEventLog:Security EventCode=4625
| timechart count
```
This populates your dashboard with failed login attempts.

---

## 📊 Dashboard Panels

### 1. Brute Force Timechart
```spl
index=* sourcetype=WinEventLog:Security EventCode=4625
| timechart count
```

### 2. Top Targeted Usernames
```spl
index=* sourcetype=WinEventLog:Security EventCode=4625
| stats count by Account_Name
| sort -count
```

### 3. Top Source Hosts
```spl
index=* sourcetype=WinEventLog:Security EventCode=4625
| stats count by host
| sort -count
```

---

## ⚠️ Alert Configuration

> Trigger an alert if **5 or more** failed attempts occur within **1 minute**

- **Trigger Conditions**: Number of Results > 0
- **Schedule**: Real-time
- **Throttle**: 10 minutes
- **Severity**: High

---

---

## 🧠 Skills Demonstrated

- SIEM configuration with Splunk
- Real-time log forwarding and ingestion
- Detection engineering for EventCode 4625
- Visualization of attacker patterns
- Alert creation for brute force activity

---

## 🧰 Tools Used

- 🖥️ Windows 10 (Victim)
- 🐉 Kali Linux (Attacker)
- 🕵️ Splunk Enterprise
- 📡 Splunk Universal Forwarder
- 🔍 EventCode 4625 (Failed Logon)

---

## 🔗 Portfolio Ready

This project is portfolio-optimized with:

- Rich screenshots
- Time-series + stacked bar charts
- Reusable detection SPL
- GitHub-friendly layout

---

## ✉️ Contact

**Armando Arias**  
📫 Email: [armando@ariasinfosecurity.tech](mailto:armando@ariasinfosecurity.tech)  
🔗 GitHub: [github.com/armandoariasinfosec](https://github.com/armandoariasinfosec)
