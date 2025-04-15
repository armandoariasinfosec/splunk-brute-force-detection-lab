# 🔐 Home Cybersecurity Lab – Blue Team vs Red Team Simulation

## Overview

This project was born out of a personal goal to go beyond theoretical knowledge and get hands-on experience with cybersecurity tools, tactics, and workflows. I built a full lab environment using VirtualBox to simulate both offensive and defensive scenarios. On one side, I had Kali Linux performing brute-force login attempts. On the other, I had a Windows 10 system acting as the target, feeding real-time logs into Splunk via Universal Forwarder. Sitting in the middle was a Splunk SIEM on Ubuntu Server m...

This lab helped me explore how attackers behave and how defenders detect — and gave me a space to build, break, fix, and learn.

---

## Lab Architecture

```
Kali Linux (Attacker)
        ↓
  Brute-force RDP
        ↓
Windows 10 (Target w/ Universal Forwarder)
        ↓
 Security Log Forwarding (TCP 9997)
        ↓
Ubuntu Server (Splunk SIEM)
```

---

## 💻 Tech Stack

| Role            | Tool / OS                         |
|-----------------|-----------------------------------|
| Hypervisor      | VirtualBox                        |
| Attacker        | Kali Linux                        |
| Target          | Windows 10 (w/ RDP enabled)       |
| SIEM            | Ubuntu Server + Splunk Enterprise |
| Logging Agent   | Splunk Universal Forwarder        |
| Protocol        | RDP (port 3389)                   |

---

## 🚀 Deployment Instructions

### 1. Set Up Virtual Machines
- Install [VirtualBox](https://www.virtualbox.org/)
- Create 3 VMs:
  - **Kali Linux**
  - **Windows 10**
  - **Ubuntu Server**

### 2. Configure Network
- Use **NAT** + **Host-Only Adapter** for each VM
- NAT provides internet; Host-Only enables VM-to-VM communication

### 3. Configure Ubuntu Server (Splunk SIEM)
- Download Splunk Enterprise (free trial) from [Splunk Downloads](https://www.splunk.com/en_us/download/splunk-enterprise.html)
- Install using `.deb` package and configure to listen on TCP port `9997`
- Start Splunk and create a user login

### 4. Configure Windows 10
- Enable **Remote Desktop**
- Disable **NLA (Network Level Authentication)**
- Install **Splunk Universal Forwarder**
- Forward Windows **Security logs** to the Splunk SIEM

### 5. Configure Kali Linux
- Install `rdesktop`:
  ```bash
  sudo apt update && sudo apt install rdesktop -y
  ```
- Run brute-force attempts:
  ```bash
  rdesktop -u fakeuser -p wrongpass 192.168.56.104
  ```

### 6. Verify Logs in Splunk
- Log into Splunk Web (typically at `http://192.168.56.101:8000`)
- Use the search:
  ```spl
  index=* sourcetype=WinEventLog:Security EventCode=4625
  ```

---

## 🔍 Splunk Search Example

```spl
index=* sourcetype=WinEventLog:Security EventCode=4625
```

---

## 🧠 Key Learnings

- Built and configured an end-to-end detection lab from scratch
- Troubleshot VM networking issues using NAT and Host-Only interfaces
- Learned how to forward Windows Event Logs using Splunk UF
- Practiced searching and filtering data in Splunk
- Simulated real-world brute-force behavior and monitored detection in real-time

---

## 📈 Future Improvements

- Create a Splunk dashboard for login attempt trends
- Build alerts to notify on brute-force behavior
- Expand attack surface to include Linux log forwarding and web application attacks
- Document log pipelines and attack detection use cases

---

## ⚡️ Why I Built This

Certifications and labs are great, but I wanted something real — something I could build, attack, and monitor myself. This project helped me reinforce blue team fundamentals, gave me insight into red team tactics, and gave me material to showcase in interviews and my portfolio.

---

## ✉️ Contact

**Armando Arias**  
📫 Email: [armando@ariasinfosecurity.tech](mailto:armando@ariasinfosecurity.tech)  
🔗 GitHub: [github.com/armandoariasinfosec](https://github.com/armandoariasinfosec)
