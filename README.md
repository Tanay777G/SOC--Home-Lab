# SOC--Home-Lab
Splunk SIEM rules, Snort IDS, MITRE ATT&amp;CK mappings, and incident response playbooks

# SOC Home Lab — Detection Engineering Portfolio

## Overview
Fully operational home SOC environment built to simulate real-world 24x7 security operations monitoring. All detections are mapped to the MITRE ATT&CK framework and Cyber Kill Chain.

## Lab Environment
- **SIEM:** Splunk (log ingestion, correlation searches, custom detection rules)
- **IDS:** Snort (custom rules for port scans, brute force, C2 patterns)
- **Endpoints:** Windows VM + Linux VM (log sources)
- **Frameworks:** MITRE ATT&CK Navigator, NIST SP 800-61, Cyber Kill Chain

## What's In This Repo
| Folder | Contents |
|--------|----------|
| /splunk-rules | Custom SPL detection searches mapped to ATT&CK |
| /snort-rules | Custom Snort IDS rules (.rules files) |
| /reports | Incident response reports and IR drills |
| /screenshots | Lab environment screenshots |

## Key Detections Built
- Failed login brute force (Event ID 4625) → ATT&CK T1110
- Suspicious process creation (Event ID 4688) → ATT&CK T1059
- C2 callback pattern detection via Snort → ATT&CK T1071
- Lateral movement via explicit credential use (Event ID 4648) → ATT&CK T1550

## Certifications
- CompTIA Security+ | CEH | CISSP (Associate) | CompTIA Network+
- Simplilearn Cybersecurity Expert Master's Program — June 2026

## Connect
- LinkedIn: linkedin.com/in/tanayshirsat
- TryHackMe: tryhackme.com/p/tanayshirsat
