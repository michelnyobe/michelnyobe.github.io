---
title: "Whoami /All"
date: 2025-05-04
---
<div style="font-size: 13px;">

**Michel NYOBE** | Pentester & Cloud Security Enthusiast  , developpeur 
📍 Paris, France | 📧 michelnyobe87@gmail.com | 🔐 

---

## Profil

Passionné par la **cybersécurité offensive**, le je me spécialise dans les attaques **Active Directory**, le pentesting d’environnements cloud (Azure & AWS), et la sécurité des identités. En formation continue et à la recherche d’un poste/apprentissage en Red Team ou pentest.

---

## Labs et Environnements

- Active Directory Lab (6 VM) automatisé sur Azure  
- Plateformes : 
    - TryHackMe  
    - [HackTheBox](/tags/hackthebox/)

    - Root-Me  
- Défis réguliers : 
    - Pro Labs
        - Dante HTB
        - Zephyr HTB 

---

![profil_htb](/images/20250506164239.png)


![rootme](/images/20250506164745.png)
https://www.root-me.org/vladimirlarass


![tryhackme](/images/20250506165505.png)
<iframe src="https://tryhackme.com/api/v2/badges/public-profile?userPublicId=1717193" style='border:none;'></iframe>
---
## Formations & Certifications

- **CRTP (A jour)** — Certified Red Team Professional  
- **CRTE (A jour)** — Certified Red Team Expert 
- **CARTP (En cours)** — Certified Azure Red Team Professional 
- **CPTS - HTB (en cours)** — Certified Penetration Testing Specialist  
- ** OSCP ( En cours) 
- **AZ-900 (A jour)** — Azure Fundamentals  
- **AZ-500 (A jour)** — Azure Security Engineer  
- **SC-900 (A jour)** — Security, Compliance, and Identity Fundamentals
- **CNSP ( A jour )** — Certified Network Security Practitioner
- **Security+ (CompTIA)** : Concepts fondamentaux de la sécurité SI, gestion des incidents, sécurité réseau et organisationnelle  
- **HTB ProLabs** : DANTE, Zephyr
---

## Expériences

#### 🛡️ Ingénieur Sécurité – **ATOS** (04/2026 – maintenant)
- 

#### 🛡️ Développeur Logiciels de Sécurité – **ENIX** (07/2023 – 01/2024)
- Développement de scripts (Next.js, GraphQL)
- Administration d’Active Directory, Wazuh, [**PowerShell**](https://nyobemichel.me/Articles/Powershell/)

#### 🔐 DevSecOps – **TDR Consulting** (05/2022 – 07/2023)
- Sécurisation des chaînes CI/CD : Docker, Kubernetes, Jenkins
- Projets : [Uptiimum.tech](https://uptiimum.tech), [IFPCE Africa](https://e-learning.ifpce-africa.tech)

#### 🧠 Ingénieur Sécurité (Stage) – **Backbone Corp** (08/2021 – 10/2021)
- Configuration de serveurs, sauvegarde (Zabbix, Bacula)
- Python, documentation technique

#### 💻 Technicien Support Informatique (Stage) – **ENEO** (07/2019 – 10/2019)
- Administration réseau (Cisco), Active Directory
- Virtualisation : VMware, Proxmox

---
## 🛡️ Compétences Techniques en Cybersécurité

---

#### 🔍 Tests d’Intrusion Active Directory

Les tests d’intrusion AD simulent des attaques ciblant des environnements Windows d’entreprise.

##### 🏁 Reconnaissance Active Directory
- - [**Enumération Active Directory**](https://nyobemichel.me/posts/enumeration-active-directory/)
- Enumération LDAP : `ldapsearch`, `ADFind`, `SharpHound`
- Cartographie des relations d’accès avec **BloodHound** (Neo4j)
- Enumération DNS interne : `nslookup`, `dig`, `dnscmd`
- Enumération des partages et services : `smbclient`, `smbmap`, `rpcclient`, `CrackMapExec`

##### 🔐 Techniques d’authentification
- **AS-REP Roasting** : utilisateurs avec `DONT_REQ_PREAUTH`
- [**Kerberoasting** : extraction de tickets TGS](https://nyobemichel.me/posts/kerberoasting/)
- **Pass-the-Hash** : utilisation de hash NTLM pour l’authentification
- **Pass-the-Ticket** : réutilisation de tickets Kerberos
- **Overpass-the-Hash** / **Pass-the-Key** : création de TGT
- **NTLM Relay** : `ntlmrelayx`, `mitm6`

##### 🧬 Exploitation des délégations
- **Délégation non contrainte** : extraction TGT depuis les services
- **Délégation contrainte (S4U2Self / S4U2Proxy)** : abus via `Rubeus`
- **RBCD (Resource-Based Constrained Delegation)** : `Powermad`, `Rubeus`

##### 🛠️ Vulnérabilités spécifiques
- **Zerologon (CVE-2020-1472)** : élévation via Netlogon
- **PetitPotam** : coercion NTLM via MS-EFSRPC
- **PrintNightmare (CVE-2021-34527)** : RCE via spouleur
- **Shadow Credentials** : ajout de clé dans AD
- **LAPS** et **GPP Passwords** : extraction dans SYSVOL

##### 🧑‍💻 Mouvement latéral & persistance
- **DCSync**, **DCShadow**
- **Golden Ticket**, **Silver Ticket**
- **Skeleton Key**, **SIDHistory abuse** , - [**SID** : Identificateurs de sécurité](https://nyobemichel.me/posts/sid--identificateurs-de-s%C3%A9curit%C3%A9-/)
- Credential Dumping : `Mimikatz`, `LaZagne`, `Rubeus`, `SharpDPAPI`

### 💥 Exploitation de Vulnérabilités

##### 🔎 Scanning & Enumeration
- **Nmap**, **Nessus**, **OpenVAS**, **Nikto**, **LinPEAS**, **WinPEAS**
- Recherche de failles : `searchsploit`, `exploit-db`, `CVE.sh`, VulnHub
##### ⚙️ Techniques
- Buffer Overflow, SEH, DEP/ASLR bypass
- Injections : SQLi, Commande, XXE, XSS, LFI/RFI
- **Escalade locale** : kernel exploits, SUID abuse, ACL
- **Reverse Engineering** : `Ghidra`, `strings`, `radare2`
##### 📡 Outils
- `Metasploit`, `Impacket`, `Responder`, `CrackMapExec`, `Evil-WinRM`
- Post-exploitation : `Chisel`, `Ligolo`, `Pupy`, `PowerView`, `Empire`


### 🌐 Attaques Web

##### 🔍 Reconnaissance
- Fuzzing & découverte : `FFUF`, `Gobuster`, `Dirsearch`
- Interception & Analyse : `Burp Suite`, `ZAP`, `Postman`
- Analyse JS, robots.txt, cookies, headers HTTP
##### 💉 Vulnérabilités Web
- **SQLi**, **XSS (Reflected, Stored, DOM)**
- **Command Injection**, **SSRF**, **XXE**
- **File Inclusion** (LFI/RFI)
- **File Upload** bypass (extension, MIME)
- **CSRF**, **Broken Access Control (IDOR, path traversal)**
- **Auth bypass**, **Brute force**, **JWT cracking**
#### 🧪 Post-exploitation
- **Reverse shells** en PHP, Python, Bash
- **Webshells** : C99, WSO, China Chopper
- **SSRF vers metadata AWS**, **Pivoting via intranet**
##### 🔧 Outils Web
- `Burp Suite`, `SQLmap`, `WFuzz`, `XSStrike`, `Commix`, `JWT Tool`


### 🔐 Cloud Security

##### ☁️ Microsoft Azure & Azure AD
- **IAM & Sécurité** :
  - RBAC, PIM, Conditional Access, Identity Protection
- **Microsoft Defender (Cloud, Endpoint)** :
  - Posture de sécurité, alertes, intégration Sentinel
- **Microsoft Sentinel (SIEM)** :
  - KQL, alertes, playbooks, détection MITRE
- **Outils** : `Az`, `AzureHound`, `Stormspotter`, `ROADtools`, `AADInternals`
##### ☁️ AWS Security
- IAM (Roles, Policies, MFA, SCP)
- Logging & audit : CloudTrail, Config, CloudWatch
- Détection & alertes : GuardDuty, Security Hub, Macie
- Outils Red Team : `Pacu`, `enumerate-iam`, `cloudsplaining`, `ScoutSuite`


### 📊 SIEM

- **Splunk** : SPL, détection de comportements, dashboards
- **QRadar** : corrélation, offenses, log sources
- **Wazuh** : HIDS, ELK, intégration Sysmon

#### 📌 Cas pratiques
- Détection brute force, exfiltration DNS
- Analyse de logs AD, GPO, accès privilégiés
- Threat Hunting MITRE ATT&CK

#### ⚙️ SOAR

- **Cortex XSOAR** :
  - Playbooks automatisés : enrichissement IOC, blocage IP
  - Intégration avec EDR, SIEM, antivirus, email
  - Cas d’usage : phishing, quarantaine automatique, sandbox analyse

#### 🧪 Analyse de Logs & Réponse à Incident

- Journaux : Syslog, Event Viewer, Apache/Nginx, Firewall, AV/EDR
- Techniques de **Threat Hunting** (MITRE, hypothesis-based)
- Outils : YARA, Sigma, Sysmon, KAPE, Velociraptor

#### 📈 Vulnerability Management

- **Outils** : Nessus, OpenVAS, CVE.sh
- **Processus** :
  - Scans récurrents, CVSS scoring
  - Patch management, hardening, benchmark CIS/STIG
- **Reporting** : priorisation, KPI, plans de remédiation

#### 🧰 Outils Techniques

##### Offensive
- **Burp Suite**, **Nmap**, **Metasploit**, **Wireshark**
- **Impacket**, **CrackMapExec**, **BloodHound**
- **Responder**, **Rubeus**, **PowerView**, **Mimikatz**
- **Tools Web** : `SQLmap`, `FFUF`, `Dirsearch`, `ZAP`

##### Reverse & Post-exploitation
- **Ghidra**, `strings`, `radare2`, `Powershell Empire`, `Covenant`

##### 🤖 Automatisation & Scripting

- **Bash** : durcissement Linux, audits, automatisation
- **PowerShell** : scripts d’audit AD, post-exploitation Windows
- **Python** : scrapers, outils offensifs, parsing, APIs
- Frameworks : `Requests`, `Paramiko`, `Nmap`, `Shodan`, `Flask`, `PyInstaller`

##### 🚀 CI/CD & DevSecOps

- **GitHub Actions**, **Azure DevOps** : CI/CD, linters, scan SAST
- **Docker** : images sécurisées, vulnérabilité scan (`Trivy`, `Snyk`)
- **Kubernetes (base)** : RBAC, secrets, pods, monitoring
- Intégration des outils de sécurité dans les pipelines (DevSecOps)

#### 💻 Langages de Programmation

- **Python** : outils de pentest, détection de vulnérabilités
- **PHP** : développement Web (Laravel), injections
- **C / C++** : exploitation mémoire, reverse engineering
- **JavaScript** :  développement web


