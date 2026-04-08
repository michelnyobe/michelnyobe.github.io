---
title: "Attaque AD"
date: 2026-03-06T10:00:00+02:00
draft: true
tags: ["pentest", "Active Directory", "PowerView", "reconnaissance", "CRTP", "red team"]
categories: ["Red Team", "pentester"]
summary: ""
showToc: true
tocOpen: true
---
je vais lister quelques attaues AD

- [[Enumeration Active Directory]]
- [[Kerberoasting]]
- [[AS-REP roasting]]
- [[Overpass-the-Hash]]
- [[Pass-the-Ticket]]
- [[Golden Ticket]]
- [[Silver Ticket]]
- [[Skeleton Key]]
- [[Kerberos Delegation Abuse]]
    -[[Unconstrained Delegation]]
    - [[Constrained Delegation]]
    - [[Resource-Based Constrained Delegation]] (RBCD)
- [[Pass-The-Hash]]
- NTLM Relay
- SMB Relay
- LDAP Relay
- HTTP Relay
- SMB Signing bypass
- Password spraying
- Brute force (rare en réel mais conceptuellement)
- Credential dumping
- Token impersonation
- SID History abuse
- Service Account abuse
- Local Admin password reuse
- LAPS misconfiguration abuse
- DCSync
- DCShadow
- NTDS.dit extraction
- SYSTEM hive abuse
- SYSVOL abuse
- GPP Passwords
- AD database offline extraction
- GPO abuse
- Modification de scripts de logon
- Scheduled tasks via GPO
- Startup scripts malveillants
- Abuse des permissions GPO
- Forest Trust abuse
- External Trust abuse
- Child → Parent escalation
- SID Filtering bypass
- Trust account compromise
- SMB lateral movement
- WMI lateral movement
- WinRM lateral movement
- PsExec-like techniques
- RDP abuse
- Scheduled Tasks remote
- Service creation abuse
- Golden Ticket persistence
- ACL backdoor
- GPO persistence
- AdminSDHolder abuse
- Shadow Credentials persistence
- Service account persistence
- DCShadow persistence
- Living Off The Land (LOLBins)
- Bypass AMSI
- Bypass PowerShell logging
- Evasion EDR via AD paths
- Time-based attacks
- Token manipulation
- [[BadSuccessor]]
- [[Bypassing WDECK]]
- [[DPAPI abuse]]