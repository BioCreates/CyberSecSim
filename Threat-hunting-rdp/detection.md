# 🔵 Blue Team – RDP Brute Force Detection

 🎯 Objective
Detect signs of an RDP brute-force attack using Windows Security Logs, Sysmon, and Sigma rules.

---

 📖 Relevant Event IDs

| Event ID | Source   | Description                   |
|----------|----------|-------------------------------|
| **4625** | Security | Failed logon attempt          |
| **4776** | Security | NTLM authentication failure   |
| **Sysmon 3** | Sysmon | Network connection initiated |
| **Sysmon 10**| Sysmon | Process access attempt       |

---

 🪪 Indicators of Attack

- High frequency of Event ID 4625 from the same source IP.
- Targeted accounts include "Administrator" or common usernames.
- Multiple failures in a short time window.

---

 🛠️ Manual Detection Process

1. Open **Event Viewer** on the Windows host.
2. Navigate to `Windows Logs` → `Security`.
3. Filter for `Event ID 4625`.
4. Look for:
   - Same IP address repeating.
   - Multiple login attempts in minutes.

---

 🖥️ PowerShell Detection Script

```powershell
$since = (Get-Date).AddMinutes(-10)
$events = Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4625; StartTime=$since}

$parsed = $events | ForEach-Object {
  $x = [xml]$_.ToXml()
  [pscustomobject]@{
    TimeCreated = $_.TimeCreated
    IpAddress   = ($x.Event.EventData.Data | Where-Object {$_.Name -eq 'IpAddress'}).'#text'
    LogonType   = ($x.Event.EventData.Data | Where-Object {$_.Name -eq 'LogonType'}).'#text'
    Account     = ($x.Event.EventData.Data | Where-Object {$_.Name -eq 'TargetUserName'}).'#text'
  }
}

$parsed |
  Where-Object { $_.LogonType -eq '10' } |
  Group-Object IpAddress |
  Where-Object { $_.Name -and $_.Count -ge 5 } |
  Select-Object Name, Count
