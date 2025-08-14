# 📂 Logs Directory

This folder is reserved for **raw or exported log data** collected during the RDP Brute Force Detection Lab.

## Purpose
Centralize all log evidence used for detection, validation, and documentation.  
These logs support the analysis shown in:
- `detection.md`
- `validation.md`
- `attack.md`

## Current Status
At present, this folder contains **no raw `.evtx` or `.txt` log exports**.  
Detection validation was performed using:
- **Event Viewer screenshots** (stored in `/screenshots/`)
- **PowerShell aggregation script output**

## Future Use
If running this lab again or in a production-like environment:
1. Export relevant **Windows Event Logs** (Security, Sysmon) as `.evtx` or `.csv`.
2. Save them in this directory with a clear naming format, e.g.:
   - `securitylog_failedlogons.evtx`
   - `sysmon_networkconnections.csv`
3. Reference these files in `validation.md` for reproducibility.

---
*Note: Logs may be excluded from public repos if they contain sensitive or real-world data.*
