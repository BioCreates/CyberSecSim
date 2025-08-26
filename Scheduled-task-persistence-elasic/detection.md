# Detection

## Sigma Rule (Sysmon Event ID 1, schtasks.exe)
Path: `artifacts/sigma/scheduled_task_creation_schtasks.yml`

```yaml
title: Scheduled Task Creation via Schtasks
id: 4c5b777a-e2b0-49a1-9c2f-71b4b8a9b7f0
status: experimental
description: Detects creation of scheduled tasks via schtasks.exe
author: RoninSec
date: 2025/08/24
logsource:
  product: windows
  service: sysmon
detection:
  selection:
    EventID: 1
    Image|endswith: '\schtasks.exe'
  condition: selection
fields:
  - CommandLine
level: medium
```

---

## Converting with Sigma CLI (Lucene)

First, install the required backends and pipelines:

```powershell
sigma plugin install elasticsearch
sigma plugin install sysmon
sigma plugin install windows
```

Convert the Sigma rule using the **ecs_windows** pipeline:

```powershell
sigma convert -t lucene -p ecs_windows `
  .\artifacts\sigma\scheduled_task_creation_schtasks.yml `
  -o .\artifacts\scheduled_task_creation_schtasks_lucene.txt
```

---

## Resulting Lucene Query
This is what was used in Kibana Discover (after switching the query language to Lucene):

```lucene
winlog.channel:"Microsoft-Windows-Sysmon/Operational" AND event.code:1 AND process.executable:*\\schtasks.exe
```

---

## Tips
- Switch Kibana **Discover** filter language from **KQL → Lucene** for converted Sigma queries.
- Keep rules modular in `artifacts/sigma/` for easy conversion and reuse.

---

## Gotchas (Detection)

- **UUID required** → every Sigma rule `id:` must be a valid UUIDv4.  
- **YAML indentation** → invalid spacing breaks `sigma convert`.  
- **Language mismatch** → Elastic Discover defaults to KQL; converted rules here target **Lucene**.  
