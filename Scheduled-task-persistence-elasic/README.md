# Scheduled Task Persistence – Elastic + Sigma (Lab)

**Goal:** Detect Windows scheduled-task persistence created with `schtasks.exe` using Elastic (Docker), Elastic Agent (standalone), Sysmon, and a Sigma rule converted to Lucene.

## Stack
- Elasticsearch 8.12.2 (Docker)
- Kibana 8.12.2 (Docker)
- Elastic Agent 8.12.2 (Windows, standalone)
- Sysmon v13+ (Event ID 1)
- Sigma CLI (sigma-cli)

## Quick Start
1) Bring up Elastic in Docker:
   ```powershell
   docker compose up -d
Install Elastic Agent on Windows (standalone) – see deploy/agent-install.ps1.

Edit the right config: C:\Program Files\Elastic\Agent\elastic-agent.yml:

yaml
Copy code
outputs:
  default:
    type: elasticsearch
    hosts: ['http://<YOUR_HOST_IP>:9200']
    username: 'elastic'
    password: '<ELASTIC_PASSWORD>'
Then:

powershell
Copy code
Restart-Service "Elastic Agent"
Generate persistence:

powershell
Copy code
schtasks /create /sc minute /mo 1 /tn "UpdaterSvc" /tr "cmd.exe /c whoami > C:\Windows\Temp\whoami.txt" /ru SYSTEM /f
Convert & hunt with Sigma – see artifacts/sigma/ and detection.md.

Notes
Kibana uses the kibana_system service account. Do not log in Kibana as elastic.

Discover defaults to KQL. Switch to Lucene for the converted query.