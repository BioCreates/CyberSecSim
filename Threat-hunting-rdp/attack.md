# 🔴 Red Team – RDP Brute Force Simulation

## 📜 Overview
This section documents the simulated RDP brute force attack performed against a Windows 10 VM.  
The goal was to generate realistic Event ID 4625 (failed logon) entries for blue team detection and Sigma rule validation.

---

## 🖥️ Lab Environment
**Attacker machine:** WSL (Ubuntu) on Windows host  
**Target machine:** Windows 10 Pro VM  
**Network:** Local VM NAT network (isolated lab traffic)  
**Service Enabled:** Remote Desktop Protocol (RDP)  

---

## 🛠️ Tools & Wordlists
- **Hydra** – High-speed network login cracker  
- **Username list (`users.txt`)** – Custom file containing common admin accounts (e.g., Administrator, Admin, Test)  
- **Password list** – `/usr/share/wordlists/rockyou.txt` (default in Kali/WSL)  

---

## ⚡ Attack Command
Executed from **WSL terminal**:

```bash
hydra -L users.txt -P /usr/share/wordlists/rockyou.txt -t1 rdp://192.168.1.73
````

**Flags Explained:**

* `-L users.txt` → list of usernames to try
* `-P rockyou.txt` → list of passwords to try
* `-t1` → single-threaded to avoid RDP service crashes or account lockouts
* `rdp://192.168.1.73` → target protocol and IP address

---

## 📂 Expected Log Artifacts

**On the target (Windows 10 VM):**

* **Event ID 4625** → Failed logon
* **Logon Type 10** → RemoteInteractive (RDP)
* **Source IP** field → Hydra attacker's IP (seen in Event Viewer)
* Sysmon logs for network connections (if Sysmon installed)

---

## 📸 Screenshots

 Hydra in Action

<img width="1467" height="187" alt="HydraLogSS" src="https://github.com/user-attachments/assets/e038fc9a-2321-45fb-84eb-88187e273dae" />

 Windows Event Viewer

<img width="1053" height="653" alt="WindowsSecurityRDPLog" src="https://github.com/user-attachments/assets/121cbe4a-fb84-43b4-b6d5-7ac47c5df1cd" />

---

## 🧹 Cleanup

* Stopped Hydra with `CTRL+C`
* Verified that no accounts were locked out
* RDP remained enabled for further testing

---

## 🧠 Lessons Learned

* Using `-t1` avoids generating excessive noise that can disrupt RDP entirely during a lab test.
* Event ID 4625 with Logon Type 10 is a clear indicator of remote login attempts — perfect for mapping into a Sigma rule.
* WSL allowed use of Linux-native tools without spinning up a separate Kali VM, but file paths and repo management must be planned in advance.

```
