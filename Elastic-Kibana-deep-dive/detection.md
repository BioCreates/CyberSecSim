###**detection.md**
```markdown
# Detection

## Sigma Rules
- **Scheduled Task Creation**
  - `artifacts/sigma/scheduled_task_creation_schtasks.yml`
- **Registry Run Key Persistence**
  - `artifacts/sigma/registry_set_susp_reg_persist_explorer_run.yml`

## Conversion
```powershell
sigma convert -t lucene -p ecs_windows .\artifacts\sigma\*.yml -o .\artifacts\converted_queries\
```
## Example Queries
### Lucene
```
winlog.channel:"Microsoft-Windows-Sysmon/Operational" AND event.code:1 AND process.executable:*\\schtasks.exe
```
```
event.code:13 AND registry.path:*\\Software\\Microsoft\\Windows\\CurrentVersion\\Run\\*
```