# Attack / Simulation

## Technique
- **MITRE ATT&CK**: T1053.005 – Scheduled Task/Job: Scheduled Task  
- **Tooling**: Native Windows binaries (`schtasks.exe`) and PowerShell ScheduledTask cmdlets  

## Execution

### Using schtasks.exe
```cmd
schtasks /create /sc minute /mo 1 /tn "UpdaterSvc" ^
  /tr "cmd.exe /c whoami > C:\Windows\Temp\whoami.txt" ^
  /ru SYSTEM /f
```

This creates a recurring task named **UpdaterSvc** that runs every minute and writes the current user context into `C:\Windows\Temp\whoami.txt`.

---

### Using PowerShell
```powershell
$A = New-ScheduledTaskAction -Execute "cmd.exe" -Argument "/c whoami > C:\Windows\Temp\whoami_ps.txt"
$T = New-ScheduledTaskTrigger -Once -At (Get-Date).AddMinutes(1)
Register-ScheduledTask -TaskName "UpdaterSvcPS" -Action $A -Trigger $T -User "SYSTEM" -RunLevel Highest
```

This creates a one-time task named **UpdaterSvcPS** using PowerShell’s ScheduledTask cmdlets.  
It executes once (at the scheduled minute) and writes the output to `whoami_ps.txt`.

---

## Cleanup
Always remove simulation artifacts to restore the host to a clean state:

```cmd
:: Remove schtasks persistence
schtasks /delete /tn "UpdaterSvc" /f

:: Remove PowerShell persistence
powershell -NoP -C "Unregister-ScheduledTask -TaskName 'UpdaterSvcPS' -Confirm:$false"

:: Delete dropped artifacts
del C:\Windows\Temp\whoami.txt 2>nul
del C:\Windows\Temp\whoami_ps.txt 2>nul
```

---

## Notes
- Both approaches mimic how adversaries create persistence with scheduled tasks.  
- Using both `schtasks.exe` and PowerShell provides broader coverage for detection testing.  
- Sysmon Event ID 1 captured these process creations for analysis in Elastic.  