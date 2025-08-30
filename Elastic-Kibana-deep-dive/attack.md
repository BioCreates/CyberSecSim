# Attack Simulation

## Technique
- **MITRE ATT&CK**: 
  - T1053.005 – Scheduled Task: Scheduled Task/Job
  - T1547.001 – Registry Run Keys / Startup Folder
- **Tooling**: Native Windows (`schtasks.exe`, PowerShell), Registry Editor

## Execution
### Scheduled Task
```cmd
schtasks /create /sc minute /mo 1 /tn "UpdaterSvc" ^
  /tr "cmd.exe /c whoami > C:\Windows\Temp\whoami.txt" ^
  /ru SYSTEM /f
```
## Registry Run Key
```cmd
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v ExplorerRun /t REG_SZ /d "C:\Windows\System32\cmd.exe /c notepad.exe"
```

## Cleanup
```cmd
schtasks /delete /tn "UpdaterSvc" /f
reg delete "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v ExplorerRun /f
del C:\Windows\Temp\whoami.txt 2>nul
```