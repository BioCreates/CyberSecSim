# ✅ Detection Validation – RDP Brute Force

## 📜 Objective
Confirm that the simulated RDP brute force activity (Part 2 – `attack.md`) can be detected without a SIEM by using native Windows tools, then link the results back to the Sigma rule for future SIEM integration.

---

## 1️⃣ Manual Event Viewer Check
- Opened **Event Viewer** → Windows Logs → Security.
- Filtered by:
  - **Event ID 4625** (failed logon)
  - **Logon Type = 10** (RDP)
- Observed **multiple failed attempts from the same IP address** within a short time window.
- This confirmed that the attack generated the expected telemetry.

---

## 2️⃣ PowerShell Aggregation Script
Executed the following PowerShell on the **Windows 10 VM** to replicate Sigma’s detection logic locally:

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
````

**Result:**
Output displayed the attacking IP with a **count ≥ 5**, confirming that the brute-force pattern was detected.

---

## 3️⃣ Sigma Rule Cross-Check

The enhanced Sigma rule (`artifacts/rdp-bruteforce-agg-roninsec-v1.yml`) contains the same logic:

```yaml
selection:
  EventID: 4625
  LogonType: 10
timeframe: 10m
condition: selection | count(IpAddress) by 1 >= 5
```

* The PowerShell script is essentially the *manual implementation* of this rule.
* This proves that once loaded into a SIEM, the rule will detect the same pattern automatically.

---

## 4️⃣ WSL + Sigma CLI Notes

* Sigma rule tested with **Uncoder.io** for SPL (Splunk), KQL (Sentinel), and Elastic conversions.
* WSL used for Sigma CLI testing due to Python virtual environment setup.
* Workflow confirmed the Sigma → SIEM conversion process is functional, even without a live SIEM.

---

## 📂 Artifacts

* **`rdp-bruteforce-agg-roninsec-v1.yml`** – Sigma rule file
* **`powershell-validation.ps1`** – PowerShell detection script
* **Screenshots** – Event Viewer and PowerShell output showing detection

```
