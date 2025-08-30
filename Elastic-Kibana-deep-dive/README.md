# Elastic + Kibana Deep Dive Lab

## Purpose
Explore Elastic as a hunting platform beyond single Sigma detections.  
This lab focused on comparing query languages (KQL, Lucene, EQL), visualizing Sysmon logs, and experimenting with registry persistence detections.

## Environment
- ElasticSearch + Kibana via Docker (`es01`, `kib01`)
- Elastic Agent (Windows VM) sending Sysmon data
- Sigma CLI for rule conversion

## Workflow
1. **Exploration**
   - Queried raw Sysmon logs in Discover.
   - Compared KQL, Lucene, and EQL syntax for `schtasks.exe`.

2. **Visualization**
   - Built dashboards:
     - Bar chart: Sysmon Event ID 1 over time.
     - Pie chart: Top process names.
     - Bar stacked: Event codes over time.

3. **Detection Rules**
   - Added Sigma rule: `registry_set_susp_reg_persist_explorer_run.yml`
   - Converted to Lucene with `sigma-cli`.
   - Saved as `.txt` query in artifacts.

4. **Challenges**
   - EQL queries not supported in Discover (require SIEM/Timeline).
   - Docker performance issues on local host.
   - Sysmon config limited to process creation only — registry ops not captured.

## Takeaways
- Learned Elastic query language differences.
- Built hunting dashboards from Sysmon data.
- Structured artifacts: raw Sigma YAML + converted Lucene query.
- Identified Elastic’s limitations (resource heavy, better on dedicated host).

## Next Steps
- Move persistence detection to Wazuh for more a more permanent deployment, also running Elastic & Kibana from a laptop with only 16GB of RAM was throttling the system too hard.
- Expand Sysmon config for registry keys + network events.
- Write original Sigma rules (not just reuse).

## Structure
- `overview.md` → Narrative walkthrough.
- `attack.md` → Commands and ATT&CK mapping.
- `detection.md` → Sigma rules + converted queries.
- `validation.md` → Evidence that detection worked.
- `artifacts/` → Sigma rules, converted queries.
- `deploy/` → Docker Compose + configs.
- `logs/` → Exported raw logs.
- `screenshots/` → Kibana visualizations.

## Requirements
- Docker Desktop
- Elastic + Kibana stack
- Windows VM with Sysmon + Elastic Agent
- Sigma CLI

## References
- [SigmaHQ rules](https://github.com/SigmaHQ/sigma)
- [Elastic documentation](https://www.elastic.co/guide)