# RDP Brute Force Detection Lab

 🎯 Objective
Simulate an RDP brute-force attack against a Windows 10 system and develop detection logic using Sysmon, Event Viewer, and a Sigma rule for SIEM integration.

---

 💻 Environment
- **Windows 10 VM** (RDP enabled, Sysmon installed with SwiftOnSecurity config)
- **WSL (Ubuntu)** running Hydra for brute-force simulation
- **Event Viewer** for manual log analysis
- **Optional**: SIEM platform (Splunk, Elastic, Sentinel) for Sigma rule deployment

---

 🔍 MITRE ATT&CK Mapping
- **T1110**: Brute Force  
- **T1021.001**: Remote Services (RDP)  

---

 🛠️ Outcome
- Full attack execution documented (Hydra → RDP target)
- Detection methods explained:
  - Manual detection in Event Viewer
  - PowerShell-based correlation script
  - Sigma rule creation for SIEM conversion
- WSL-specific workflow considerations noted
- Ready for future integration into Wazuh or other SIEM for automated detection
