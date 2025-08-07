# 🔴 Red Team – RDP Brute Force Attack Simulation

## 🎯 Objective
Simulate a brute-force attack against RDP on a Windows 10 machine using Kali Linux tools to emulate real-world attacker behavior.

---

## 🛠️ Tools Used

| Tool | Purpose |
|------|---------|
| `hydra` | Brute-force login attempts |
| `ncrack` *(optional)* | RDP-specific brute-force tool |
| `Kali Linux` | Attacker VM environment |
| `Windows 10` | Target VM with RDP enabled |

---

## 🧪 Steps Taken

### 🔹 1. Enable RDP on Target
- Enabled RDP on the Windows 10 VM via:
  - `System Properties → Remote → Allow remote connections`
- Verified port `3389` is open using:


---

### 🔹 2. Gather Target Info
- Checked for open RDP with:
```bash
nmap -sS -p 3389 -Pn <target-ip>
' 
---

### 🔹 2. Launch Brute force attack

hydra -L users.txt -P rockyou.txt rdp://<target-ip>
🛑 Caution: Do NOT run this against unauthorized systems.


🧠 Notes
Multiple failed logins should trigger Event ID 4625 on Windows

Hydra may not always show successful attempts — monitor results on both ends

If you want to mimic brute-force without causing actual stress, you can script a series of failed RDP login attempts manually

See detection.md to analyze Sysmon and Windows Event Logs for alerts tied to this activity.



