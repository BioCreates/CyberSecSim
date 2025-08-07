# 🔵 Blue Team – RDP Brute Force Detection

## 🎯 Objective
Analyze Windows logs and Sysmon data to detect signs of an RDP brute-force attack and document the correlation strategy.

---

## 📖 Relevant Event IDs

| Event ID | Source | Description |
|----------|--------|-------------|
| **4625** | Security | Failed logon attempt |
| **4776** | Security | NTLM authentication failed |
| **Sysmon 3** | Sysmon | Network connection initiated |
| **Sysmon 10** | Sysmon | Process access attempt |
| *(Optional)* | Sysmon 1 | Process creation (Hydra, ncrack) |

---

## 🪪 Indicators of Attack

- High frequency of Event ID 4625 from a single source IP
- Account name "Administrator" targeted repeatedly
- Attempts using common usernames (from `users.txt`)
- Source IP logged in Sysmon network connections

---

## 🛠️ Detection Process (Manual)

1. Open **Event Viewer** on Windows target
2. Navigate to:
3. Filter for Event ID `4625`
4. Look for:
- Same IP repeated
- Multiple login attempts in short time window

---

## 🖥️ Detection Process (Splunk Example)

If logs are ingested in Splunk:

```spl
index=winlogs EventCode=4625
| stats count by Account_Name, IpAddress
| where count > 5


🧠 MITRE ATT&CK Mapping
T1110 – Brute Force

T1021.001 – Remote Services: RDP

📁 Artifacts
If applicable:

- [rdp-brute.yml](artifacts/rdp-brute.yml): Sigma rule to detect RDP brute force attempts
- Screenshots of Event Viewer logs
- Optional log exports in `/logs/`

Export screenshots from Event Viewer

Include log sample in /logs/
