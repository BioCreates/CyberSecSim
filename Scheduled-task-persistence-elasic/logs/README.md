# Logs Directory

This folder contains raw log evidence collected during the **Elastic + Sigma Scheduled Task Persistence Lab**.

## Contents
- **sysmon/** → Raw Sysmon event exports (Event ID 1: `schtasks.exe` process creation).
- **security/** → Windows Security log excerpts (if available).
- **elastic/** → JSON/NDJSON exports from Kibana Discover (converted Sigma query results).
- **agent/** → Elastic Agent service logs for troubleshooting connection issues.

## Usage
- Use these logs as **ground truth** when reviewing detections in `detection.md` and `validation.md`.
- Cross-reference timestamps between:
  - `attack.md` (when tasks were executed),
  - Sysmon logs (process creation),
  - Elastic exports (Sigma/Lucene query matches).
- Can be re-ingested into Elastic or analyzed with tools like `jq` / `grep`.

## Notes
- Logs have been minimally sanitized (no sensitive host/user info).
- Keep file sizes manageable — prefer **filtered exports** around the attack window.
- Retain original formats (EVTX → JSON, etc.) to ensure reproducibility.

---
✨ Tip: Treat this folder as your **forensic evidence bag** for the lab. Everything else (attack/detection/validation) is the *story*, this is the raw data.
