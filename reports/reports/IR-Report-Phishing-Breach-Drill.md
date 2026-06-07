# Incident Response Report
## Phishing-Initiated Breach Simulation
**Classification:** CONFIDENTIAL — Lab Exercise  
**Analyst:** Tanay Shirsat  
**Date:** June 2026  
**Severity:** HIGH  
**Status:** Resolved  

---

## 1. Executive Summary
A simulated phishing attack was detected and contained within the home SOC lab environment. The attacker gained initial access via a phishing email, established persistence, and attempted lateral movement across two Windows endpoints. Three compromised accounts were identified. Full containment was achieved within 2 hours of detection. Root cause was traced to a user opening a malicious attachment that executed a PowerShell payload.

---

## 2. Incident Timeline

| Time | Event |
|------|-------|
| 09:00 | Phishing email delivered to target inbox |
| 09:14 | User opened malicious attachment (invoice_final.docx) |
| 09:15 | PowerShell process spawned by Word (Event ID 4688) — Splunk alert triggered |
| 09:17 | Outbound C2 connection established to 192.168.56.101:4444 — Snort alert |
| 09:22 | Attacker began credential dumping — Event ID 4672 detected |
| 09:35 | Lateral movement attempt via explicit credentials — Event ID 4648 fired |
| 09:40 | SOC analyst (Tanay) began triage |
| 10:05 | 3 compromised accounts identified: user01, user02, svc_backup |
| 10:20 | Affected endpoints isolated from network |
| 10:45 | Malicious persistence mechanism removed |
| 11:00 | Accounts reset, systems reimaged |
| 11:30 | Incident closed, report filed |

---

## 3. Attack Analysis — MITRE ATT&CK Mapping

| ATT&CK Tactic | Technique | Evidence |
|---------------|-----------|----------|
| Initial Access | T1566.001 - Spearphishing Attachment | Email header analysis, malicious .docx |
| Execution | T1059.001 - PowerShell | Event ID 4688 — powershell.exe spawned by winword.exe |
| Persistence | T1053.005 - Scheduled Task | Scheduled task created under SYSTEM |
| Credential Access | T1003 - OS Credential Dumping | Event ID 4672 — SeDebugPrivilege assigned |
| Lateral Movement | T1550 - Explicit Credentials | Event ID 4648 — svc_backup used across 2 hosts |
| Command & Control | T1071.001 - Web Protocols | Outbound TCP 4444 to C2 server |
| Exfiltration | T1041 - Exfiltration Over C2 | Large outbound transfers via C2 channel |

---

## 4. Indicators of Compromise (IOCs)

| Type | Value | Context |
|------|-------|---------|
| File | invoice_final.docx | Phishing attachment — macro-enabled |
| IP | 192.168.56.101 | C2 server — attacker-controlled |
| Port | 4444 | Metasploit default listener port |
| Process | powershell.exe (parent: winword.exe) | Suspicious parent-child relationship |
| Account | svc_backup | Compromised service account used for lateral movement |
| Event ID | 4688, 4625, 4648, 4672 | Windows Security Log events triggered |

---

## 5. Forensic Evidence

### Memory Forensics (Volatility)
- Running processes showed injected DLL in svchost.exe (PID 1234)
- Open network connections confirmed C2 channel to 192.168.56.101:4444
- Persistence via scheduled task confirmed in memory artefacts

### Network Analysis (Wireshark)
- DNS queries showed abnormally long subdomain strings — possible DNS tunnelling
- TCP stream to 192.168.56.101:4444 confirmed C2 beaconing pattern
- Data exfiltration via HTTP POST requests with encoded payload body

### Active Directory
- svc_backup account authenticated to WORKSTATION-02 using pass-the-hash technique
- user01 and user02 passwords were reset by attacker post-compromise

---

## 6. Containment & Eradication Steps

1. Isolated WORKSTATION-01 and WORKSTATION-02 from network segment
2. Disabled compromised accounts: user01, user02, svc_backup
3. Blocked C2 IP 192.168.56.101 at firewall level
4. Removed malicious scheduled task from both endpoints
5. Killed injected process in svchost.exe
6. Reset passwords for all 3 compromised accounts with MFA enforced
7. Reimaged both affected workstations from clean baseline

---

## 7. Root Cause Analysis
User was not trained to identify macro-enabled documents delivered via phishing. The organisation lacked email attachment sandboxing. PowerShell execution was not restricted via AppLocker or constrained language mode.

---

## 8. Lessons Learned & Recommendations

| Priority | Recommendation |
|----------|---------------|
| CRITICAL | Enable PowerShell Constrained Language Mode via GPO |
| HIGH | Deploy email sandboxing (e.g. Microsoft Defender for Office 365) |
| HIGH | Restrict macro execution in Office documents via GPO |
| MEDIUM | Implement privileged account tiering — separate service accounts |
| MEDIUM | Enable MFA on all accounts including service accounts |
| LOW | Conduct monthly phishing simulation training for all staff |

---

## 9. Chain of Custody
All evidence collected in lab environment. Memory dumps, pcap files, and Windows Event Log exports stored securely. No real user data involved — simulation only.

---

*Report prepared following NIST SP 800-61 Incident Response lifecycle.*  
*Analyst: Tanay Shirsat | tanayshirsar777@gmail.com*
