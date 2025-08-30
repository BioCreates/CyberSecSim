# Overview

This lab demonstrates detection of Windows persistence using scheduled tasks and registry run keys, analyzed in Elastic.

## Flow
1. **Setup**: Elastic + Kibana stack (Docker Compose) and Elastic Agent on Windows VM.
2. **Attack Simulation**:
   - Created a scheduled task (`schtasks.exe`) and PowerShell variant.
   - Explored persistence via registry Run key (Explorer.exe).
3. **Detection**:
   - Sigma rules for `schtasks.exe` and registry Run persistence.
   - Converted to Lucene queries with `sigma-cli`.
   - Ran queries in Kibana → confirmed process executions.
4. **Validation**:
   - Elastic showed Sysmon Event ID 1 with `schtasks.exe`.
   - Screenshots and raw log evidence collected.

## References
- `attack.md` → full commands used
- `detection.md` → detection rules + queries
- `validation.md` → screenshots/logs
