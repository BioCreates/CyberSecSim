# Overview

- **Spin up Elastic** via Docker (`artifacts/docker/docker-compose.yml`).
- **Harden creds:** set `ELASTIC_PASSWORD` via `.env`; set `kibana_system` password via the `_security` API.
- **Install Elastic Agent (standalone)** on Windows from ZIP; fix the **Program Files** YAML host.
- **Simulate persistence** with `schtasks` (every minute → writes `whoami.txt` to `C:\Windows\Temp`).
- **Collect telemetry:** Sysmon Event ID 1.
- **Write Sigma rule** and convert to **Lucene** with `sigma-cli`.
- **Hunt in Kibana Discover** (switch language to Lucene).
- **Capture screenshots** and log raw events.

**MITRE ATT&CK Mapping**: T1053.005 – Scheduled Task/Job: Scheduled Task

Cross-refs:
- Deploy scripts → `deploy/`
- Configs & Sigma → `artifacts/`
- Detection write-up → `detection.md`
- Validation steps → `validation.md`
