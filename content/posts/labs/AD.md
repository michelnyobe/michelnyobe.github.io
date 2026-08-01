# configuration du reseau 
## Créer les VMnets dans VMware

|VMnet|Mode|Usage|
|---|---|---|
|VMnet10|Host-only|VLAN AD/LAN|
|VMnet11|Host-only|VLAN DMZ|
|VMnet12|Host-only|VLAN SOC|
|VMnet13|Host-only|VLAN DevOps|
|VMnet14|Host-only|VLAN Red Team|

![[Pasted image 20260707155924.png]]

# configuration du firewall 
nous avons mis sur une vm un parefeu OPNSense 
# configuration de l'AD DS 
#### Déployer des contrôleurs de domaine AD DS
ip add : 10.10.0.10 
nom : DC01

|VM|VMnet correct|IP attendue|
|---|---|---|
|DC01, CL01|VMnet10|10.10.0.x ✅|
|Serveur Web (DMZ)|VMnet11|10.20.0.x ✅|
|**Kali (Red Team)**|**VMnet12**|**10.50.0.x** ✅|
|**Wazuh (SOC)**|**VMnet13**|**10.30.0.x** ✅|
|**Docker/K8s**|**VMnet14**|**10.40.0.x** ✅|


# Architecture Lab Cybersécurité — nyoma.local

## Informations générales

|Élément|Valeur|
|---|---|
|Domaine AD|lab.local|
|Hyperviseur|VMware Workstation Pro|
|Hôte|Intel i7-11850H · 32 Go RAM · 954 Go|
|Firewall|OPNsense 26.1.6_2 (amd64)|
|Contrôleur de domaine|DC01 — Windows Server 2025|

---

## Topologie réseau

```
Internet (WiFi — Livebox)
        │
        │ NAT VMware (VMnet8 — 192.168.98.0/24)
        │
┌───────▼────────────────────────────────────────────┐
│              OPNsense Firewall                      │
│   WAN : em1 → 192.168.98.133/24 (DHCP NAT)        │
│   OPT1: em0 → 10.10.0.1/24  (AD/LAN)              │
│   OPT2: em2 → 10.20.0.1/24  (DMZ)                 │
│   OPT3: em3 → 10.30.0.1/24  (SOC)                 │
│   OPT4: em4 → 10.40.0.1/24  (DevOps)              │
│   OPT5: em5 → 10.50.0.1/24  (Red Team)            │
└────┬──────────┬──────────┬──────────┬──────────────┘
     │          │          │          │
  VMnet10    VMnet11    VMnet13    VMnet14
  AD/LAN      DMZ        SOC      Red Team
  10.10.0     10.20.0   10.30.0   10.50.0
```

---

## Plan d'adressage VLAN

|VLAN|Zone|Subnet|Gateway|VMnet|Plage DHCP|
|---|---|---|---|---|---|
|OPT1|AD / LAN|10.10.0.0/24|10.10.0.1|VMnet10|10.10.0.100–200|
|OPT2|DMZ|10.20.0.0/24|10.20.0.1|VMnet11|10.20.0.100–200|
|OPT3|SOC / SIEM|10.30.0.0/24|10.30.0.1|VMnet13|10.30.0.100–200|
|OPT4|DevOps|10.40.0.0/24|10.40.0.1|VMnet14|10.40.0.100–200|
|OPT5|Red Team|10.50.0.0/24|10.50.0.1|VMnet12|10.50.0.100–200|

> **Note mapping VMnet réel** : VMnet12→OPT5 (Red Team), VMnet13→OPT3 (SOC), VMnet14→OPT4 (DevOps)

---

## Inventaire des machines virtuelles

### ✅ Déployées

|VM|OS|IP|VMnet|Zone|RAM|Statut|
|---|---|---|---|---|---|---|
|OPNsense|OPNsense 26.1.6|10.10.0.1 (LAN)|NAT+tous|Firewall|2 Go|✅ Opérationnel|
|DC01|Windows Server 2025|10.10.0.10|VMnet10|AD/LAN|4 Go|✅ Opérationnel|
|CL01|Windows 11 Pro|10.10.0.101 (DHCP)|VMnet10|AD/LAN|4 Go|✅ Jointé au domaine|
|Wazuh-SOC|Ubuntu 22.04 LTS|10.30.0.10|VMnet13|SOC|8 Go|🔧 En cours|
|Kali Linux|Kali 2026.2|10.50.0.x|VMnet12|Red Team|4 Go|⚠️ Internet KO|

### ⏳ À déployer

| VM          | OS                  | IP prévue  | VMnet   | Zone   | RAM  |
| ----------- | ------------------- | ---------- | ------- | ------ | ---- |
| DC02        | Windows Server 2025 | 10.10.0.11 | VMnet10 | AD/LAN | 2 Go |
| Serveur Web | Ubuntu 22.04 LTS    | 10.20.0.10 | VMnet11 | DMZ    | 2 Go |
| Docker/K8s  | Ubuntu 22.04 LTS    | 10.40.0.10 | VMnet14 | DevOps | 6 Go |

---

## Active Directory

### Forêt et domaine

|Paramètre|Valeur|
|---|---|
|Forest Root|lab.local|
|NetBIOS|LAB|
|DC Principal|DC01.lab.local (10.10.0.10)|
|DC Secondaire|DC02.child.lab.local (à déployer)|
|OS DC|Windows Server 2025 Datacenter Evaluation|
|Rôles FSMO|SchemaMaster, DomainNamingMaster, PDCEmulator, RIDMaster|

### Structure OU

```
DC=lab,DC=local
└── OU=Corp
    ├── OU=Users
    │   ├── OU=Direction    → Groupe: GG_Direction
    │   ├── OU=IT           → Groupes: GG_IT_Admin, GG_IT_Support
    │   ├── OU=Finance      → Groupe: GG_Finance
    │   ├── OU=RH           → Groupe: GG_RH
    │   ├── OU=Marketing    → Groupe: GG_Marketing
    │   └── OU=Support      → Groupes: GG_Sales, GG_Production
    ├── OU=Servers
    ├── OU=Admins
    └── OU=ServiceAccounts  → Groupe: GG_Backup_Operators
```

### Utilisateurs créés (25)

|Nom complet|Username|Groupe|Département|
|---|---|---|---|
|Michel Nyobe|mnyobe|GG_IT_Admin|IT|
|Sarah Laurent|slaurent|GG_Direction|Direction|
|Thomas Dubois|tdubois|GG_Direction|Direction|
|Julien Moreau|jmoreau|GG_IT_Support|IT|
|Clara Petit|cpetit|GG_Finance|Finance|
|Antoine Martin|amartin|GG_Marketing|Marketing|
|Laura Bernard|lbernard|GG_Finance|Finance|
|Kevin Robert|krobert|GG_Production|Support|
|Sophie Leroy|sleroy|GG_RH|RH|
|Hugo Fontaine|hfontaine|GG_IT_Admin|IT|
|Emma Rousseau|erousseau|GG_Marketing|Marketing|
|Lucas Garnier|lgarnier|GG_IT_Support|IT|
|Inès Chevalier|ichevalier|GG_Marketing|Marketing|
|Nathan Girard|ngirard|GG_IT_Support|IT|
|Camille Bonnet|cbonnet|GG_Finance|Finance|
|Maxime Marchand|mmarchand|GG_Server_Admins|Finance|
|Léa Dupont|ldupont|GG_Backup_Operators|RH|
|Enzo Mercier|emergier|GG_IT_Support|Support|
|Chloé Faure|cfaure|GG_Marketing|Marketing|
|Adam Noel|anoel|GG_Sales|Support|

### Comptes spéciaux (Red Team)

|Username|Mot de passe|Groupe|Objectif|
|---|---|---|---|
|jmoreau|Welcome1|GG_IT_Support|Password Spray|
|svc-backup|Backup123|GG_Backup_Operators|Kerberoasting|
|adm-tech|Admin@Tech2024!|Domain Admins|Privilege escalation|

---

## Services configurés

### OPNsense

|Service|Statut|Config|
|---|---|---|
|Kea DHCPv4|✅ Actif|5 subnets (10.x0.0.100-200)|
|NAT Outbound|✅ Actif|Hybrid mode — toutes interfaces|
|Firewall Rules|✅ Actif|Pass sur OPT1–OPT5|
|DNS Unbound|⏳ À configurer|—|
|NTP|⏳ À configurer|pool.ntp.org|
|SSH|✅ Actif|Port 22|

### Active Directory (DC01)

|Service|Statut|
|---|---|
|AD DS|✅ Opérationnel|
|DNS Server|✅ Opérationnel|
|DHCP (Windows)|Non utilisé (OPNsense gère)|
|CL01 jointé|✅ LAB\cpetit connecté|

---

## Phases du projet

|Phase|Description|Statut|Progression|
|---|---|---|---|
|Phase 1|Infrastructure réseau (OPNsense + VLANs)|🔧 En cours|90%|
|Phase 2|Active Directory (DC01 + users)|✅ Terminée|85%|
|Phase 3|SOC / Blue Team (Wazuh + ELK)|🔧 En cours|5%|
|Phase 4|DMZ & Applications web|⏳ À faire|0%|
|Phase 5|DevOps (Docker + K8s)|⏳ À faire|0%|
|Phase 6|Red Team (Kali + C2)|⏳ À faire|10%|

---

# Machines Virtuelles — Lab Cybersécurité

## VMs déployées

|VM|OS|IP|VMnet|RAM|CPU|Disque|Statut|
|---|---|---|---|---|---|---|---|
|OPNsense|OPNsense 26.1.6|10.10.0.1|NAT + tous|2 Go|2 vCPU|20 Go|✅ Opérationnel|
|DC01|Windows Server 2025|10.10.0.10|VMnet10|4 Go|2 vCPU|60 Go|✅ Opérationnel|
|CL01|Windows 11 Pro|10.10.0.101|VMnet10|4 Go|4 vCPU|80 Go|✅ Jointé au domaine|
|Kali Linux|Kali 2026.2|10.50.0.x|VMnet12|8 Go|4 vCPU|80 Go|⚠️ Internet KO|
|Wazuh-SOC|Ubuntu 22.04 LTS|10.30.0.10|VMnet13|8 Go|2 vCPU|100 Go|🔧 En cours|

## VMs à déployer

|VM|OS|IP prévue|VMnet|RAM|CPU|Disque|Zone|
|---|---|---|---|---|---|---|---|
|DC02|Windows Server 2025|10.10.0.11|VMnet10|2 Go|2 vCPU|60 Go|AD/LAN|
|Serveur Web|Ubuntu 22.04 LTS|10.20.0.10|VMnet11|2 Go|2 vCPU|40 Go|DMZ|
|Zabbix|Ubuntu 22.04 LTS|10.30.0.20|VMnet13|4 Go|2 vCPU|50 Go|SOC|
|Docker/K8s|Ubuntu 22.04 LTS|10.40.0.10|VMnet14|6 Go|2 vCPU|100 Go|DevOps|

## Budget RAM total

| VM          | RAM       | CPU         | Statut           |
| ----------- | --------- | ----------- | ---------------- |
| OPNsense    | 2 Go      | 2 vCPU      | ✅ Active         |
| DC01        | 4 Go      | 2 vCPU      | ✅ Active         |
| DC02        | 2 Go      | 2 vCPU      | ⏳ À déployer     |
| CL01        | 4 Go      | 2 vCPU      | ✅ Active         |
| Wazuh       | 4 Go      | 2 vCPU      | ✅ Active         |
| Zabbix      | 3 Go      | 2 vCPU      | ✅ Active         |
| Kali Linux  | 8 Go      | 4 vCPU      | ✅ Active         |
| Serveur Web | 1 Go      | 2 vCPU      | ✅ Active         |
| Docker/K8s  | 6 Go      | 2 vCPU      | ⏳ À déployer     |
| **TOTAL**   | **40 Go** | **22 vCPU** | ⚠️ Dépasse 32 Go |

> ⚠️ **Attention** : Le total prévu (40 Go) dépasse la RAM de l'hôte (32 Go). Ne jamais démarrer toutes les VMs en même temps. Démarrer uniquement les VMs nécessaires à la phase en cours.

## Recommandation démarrage par phase

|Phase|VMs à démarrer|RAM utilisée|
|---|---|---|
|Phase 1-2 (AD)|OPNsense + DC01 + CL01|10 Go|
|Phase 3 (SOC)|+ Wazuh + Zabbix|22 Go|
|Phase 4 (DMZ)|+ Serveur Web|24 Go|
|Phase 5 (DevOps)|+ Docker/K8s|30 Go|
|Phase 6 (Red Team)|+ Kali (arrêter Docker)|28 Go|
---

## Règles firewall inter-VLAN

|Source|Destination|Action|Règle|
|---|---|---|---|
|OPT1 (LAN)|Any|✅ Pass|Allow LAN to any|
|OPT2 (DMZ)|Any|✅ Pass|Allow DMZ to any|
|OPT3 (SOC)|Any|✅ Pass|Allow SOC to any|
|OPT4 (DevOps)|Any|✅ Pass|Allow DevOps to any|
|OPT5 (RedTeam)|Any|✅ Pass|Allow RedTeam to any|
|WAN|—|NAT|Hybrid outbound NAT|

---

# Roadmap Lab Cybersécurité — Réaliste & Avancé

# Objectifs : CPTS (HTB) · CWES · CRTL (ZeroPoint) · Red Team · Exploit Dev · SOC Avancé

---

## Architecture finale visée

```
Internet
    │
OPNsense (Firewall/Router)
    │
    ├── VLAN 10 (AD/LAN)     → DC01, DC02, CL01, Matrix (Jump)
    ├── VLAN 20 (DMZ)        → Serveur Web vulnérable
    ├── VLAN 30 (SOC)        → Wazuh, Zabbix
    ├── VLAN 40 (DevOps)     → Docker/K8s (plus tard)
    └── VLAN 50 (Red Team)   → Kali, C2
```

---

## ÉTAT ACTUEL DES VMs

| VM         | Statut           | IP          | Rôle             |
| ---------- | ---------------- | ----------- | ---------------- |
| OPNsense   | ✅ Opérationnel   | 10.10.0.1   | Firewall/Router  |
| DC01       | ✅ Opérationnel   | 10.10.0.10  | Forest lab.local |
| CL01       | ✅ Jointé domaine | 10.10.0.101 | Poste victime    |
| Kali Linux | ✅ Opérationnel   | 10.50.0.x   | Attaquant        |
| Wazuh      | ✅ Opérationnel   | 10.30.0.100 | SIEM/EDR         |
| Zabbix     | ✅ Opérationnel   | 10.30.0.101 | Monitoring       |
| Matrix     | ✅ Opérationnel   | 10.20.0.101 | Application web  |
| DC02       | ⏳ À installer    | 10.10.0.11  | Child domain     |

---

## PHASE 1 — Socle réseau et sécurité (FAIT À 90%)

### Objectif : infrastructure stable et sécurisée

- [x] OPNsense installé et configuré
- [x] 5 VLANs opérationnels (10/20/30/40/50)
- [x] Kea DHCP fonctionnel sur tous les VLANs
- [x] Règles firewall inter-VLAN
- [x] NAT Outbound configuré
- [ ] DNS Unbound activé
- [ ] NTP configuré (pool.ntp.org)
- [ ] IDS/IPS Suricata activé sur OPNsense
- [ ] Réparer internet Kali (VMnet12)

---

## PHASE 2 — Active Directory réaliste (FAIT À 85%)

### Objectif : environnement AD d'entreprise avec vulnérabilités intentionnelles

- [x] DC01 Windows Server 2025 — Forest lab.local
- [x] 25 utilisateurs + groupes (script PowerShell)
- [x] Structure OU réaliste (IT/Finance/RH/Direction...)
- [x] CL01 jointé au domaine
- [x] Comptes faibles : jmoreau (Welcome1) · svc-backup (Backup123)
- [x] Domain Admin : adm-tech
- [ ] DC02 — Child domain child.lab.local
- [ ] GPO de sécurité de base
- [ ] Auditing activé (logon, object access, privilege use)
- [ ] Matrix — Jump server configuré
- [ ] Kerberoasting : SPN sur svc-backup
- [ ] AS-REP Roasting : compte sans pré-auth Kerberos
- [ ] ACL abusable : WriteDACL, GenericAll sur certains objets
- [ ] Délégation Kerberos non contrainte (unconstrained)
- [ ] AdminSDHolder mal configuré
- [ ] AD CS vulnérable (ESC1, ESC8) sur DC01

---

## PHASE 3 — SOC & Monitoring (EN COURS)

### Objectif : détection en temps réel de toutes les attaques

#### 3.1 Wazuh (SIEM/EDR)

- [x] Wazuh Manager installé (Ubuntu 22.04 — 10.30.0.10)
- [ ] Internet fonctionnel sur Wazuh
- [x] Dashboard Wazuh accessible
- [ ] Agent Wazuh sur DC01
- [ ] Agent Wazuh sur CL01
- [ ] Agent Wazuh sur DC02 (après install)
- [ ] Règles de détection AD :
    - [ ] Kerberoasting (Event 4769)
    - [ ] Pass-the-Hash (Event 4624 type 3)
    - [ ] DCSync (Event 4662)
    - [ ] Golden Ticket (Event 4768)
    - [ ] BloodHound (LDAP massif)
    - [ ] Mimikatz (lsass access)
- [ ] Alertes email/Slack configurées

#### 3.2 Zabbix (Monitoring infrastructure)

- [ ] Zabbix Server installé (Ubuntu 22.04 — 10.30.0.20)
- [ ] Monitoring DC01 (CPU, RAM, services AD)
- [ ] Monitoring CL01
- [ ] Monitoring OPNsense (SNMP)
- [ ] Monitoring Wazuh lui-même
- [ ] Dashboard personnalisé lab
- [ ] Alertes sur services critiques (AD DS, DNS, Kerberos)

#### 3.3 ELK Stack (optionnel — avancé)

- [ ] Elasticsearch + Kibana sur Wazuh ou VM dédiée
- [ ] Intégration logs OPNsense → ELK
- [ ] Intégration logs DC01 → ELK
- [ ] Dashboards de visualisation

---

## PHASE 4 — Jump Server & Matrix (Pivoting)

### Objectif : simuler un accès réaliste au SI via un point d'entrée

#### Matrix (Jump server — 10.10.0.50)

- [ ] VM Ubuntu ou Windows Server installée
- [ ] SSH/RDP accessible depuis Kali
- [ ] Connecté au VLAN AD/LAN (VMnet10)
- [ ] Agent Wazuh installé
- [ ] Pivot depuis Kali → Matrix → DC01

#### Scénarios de pivoting

- [ ] Port forwarding SSH (ssh -L, -R, -D)
- [ ] Chisel/Ligolo-ng pour tunneling
- [ ] Pivoting via SOCKS5
- [ ] Double pivot : Kali → Matrix → CL01 → DC01

---

## PHASE 5 — Red Team & C2 (Kali + C2)

### Objectif : attaques réalistes, techniques CPTS/CWES/CRTL

#### 5.1 C2 Framework

- [x] Réparer internet Kali (VMnet12 → 10.50.0.x)
- [ ] Havoc C2 installé sur Kali
    - [ ] Team server configuré
    - [ ] Listener HTTPS sur port 443
    - [ ] Payload généré pour CL01
- [ ] Sliver C2 (backup/alternative)
    - [ ] mTLS listener
    - [ ] Implant généré

#### 5.2 Attaques AD (CPTS/CRTL)

- [ ] Reconnaissance
    - [ ] BloodHound + SharpHound (graphe AD)
    - [ ] ldapdomaindump
    - [ ] enum4linux-ng
    - [ ] CrackMapExec / NetExec
- [ ] Initial Access
    - [ ] Password Spray (jmoreau → Welcome1)
    - [ ] AS-REP Roasting
    - [ ] Phishing simulé (GoPhish)
- [ ] Privilege Escalation
    - [ ] Kerberoasting (svc-backup → Backup123)
    - [ ] ACL abuse (WriteDACL)
    - [ ] Token impersonation
    - [ ] Unconstrained delegation
- [ ] Lateral Movement
    - [ ] Pass-the-Hash
    - [ ] Pass-the-Ticket
    - [ ] OverPass-the-Hash
    - [ ] WMI / PSExec / SMBExec
- [ ] Domain Dominance
    - [ ] DCSync (secretsdump)
    - [ ] Golden Ticket
    - [ ] Silver Ticket
    - [ ] Diamond Ticket
    - [ ] AD CS ESC1/ESC8
    - [ ] Cross-domain attack (lab.local → child.lab.local)
- [ ] Persistence
    - [ ] Skeleton Key
    - [ ] AdminSDHolder backdoor
    - [ ] GPO abuse
    - [ ] DCShadow

#### 5.3 Développement d'exploit (CWES)

- [ ] VM dédiée exploit dev (Windows 10 + outils)
- [ ] Buffer overflow (x86/x64)
- [ ] SEH exploitation
- [ ] ROP chains
- [ ] Bypass DEP/ASLR
- [ ] Shellcode custom

#### 5.4 Evasion EDR (CRTL)

- [ ] Bypass Wazuh/AV
    - [ ] Process hollowing
    - [ ] DLL sideloading
    - [ ] AMSI bypass
    - [ ] ETW patching
    - [ ] Unhooking NTDLL
- [ ] Obfuscation payloads
    - [ ] Encodage shellcode
    - [ ] Payload chiffré AES
    - [ ] In-memory execution
- [ ] Living off the Land (LOLBins)
    - [ ] certutil, regsvr32, mshta
    - [ ] PowerShell constrained mode bypass

---

## PHASE 6 — DMZ & Applications Web (après certifs)

### Objectif : pentest web réaliste

- [ ] Serveur Ubuntu en DMZ (VMnet11 — 10.20.0.10)
- [ ] Apache/Nginx installé
- [ ] DVWA (Damn Vulnerable Web App)
- [ ] WebGoat
- [ ] Juice Shop (OWASP)
- [ ] Application custom avec vulnérabilités :
    - [ ] SQLi, XSS, SSRF, IDOR, LFI/RFI
- [ ] WAF OPNsense en frontal

---

## PHASE 7 — Docker & Kubernetes (PLUS TARD)

- [ ] VM Ubuntu DevOps (VMnet14 — 10.40.0.10)
- [ ] Docker installé
- [ ] Kubernetes K3s cluster
- [ ] Jenkins CI/CD
- [ ] Pipeline sécurisé avec scan SAST/DAST

---

## PLANNING RECOMMANDÉ

### Semaine 1-2 (maintenant)

```
1. Finir Phase 1 → DNS Unbound + NTP + Suricata
2. Réparer internet Kali
3. Installer Wazuh complètement + agents
4. Configurer Zabbix monitoring
```

### Semaine 3-4

```
5. DC02 child domain
6. Vulnérabilités AD intentionnelles
7. Jump server Matrix
8. BloodHound reconnaissance
```

### Semaine 5-6

```
9. C2 Havoc opérationnel
10. Premières attaques AD avec détection Wazuh
11. Password Spray + Kerberoasting
12. Exercice Red vs Blue
```

### Semaine 7-8

```
13. Lateral movement + Domain dominance
14. Evasion EDR
15. Développement exploit (CWES)
16. Préparation certifs CPTS/CRTL
```

---

## CERTIFICATIONS VISÉES

|Certif|Organisme|Compétences|Lien avec le lab|
|---|---|---|---|
|CPTS|HackTheBox|Pentest complet, AD, Web|Phases 2+5+6|
|CWES|HackTheBox|Exploit Web avancé|Phase 6|
|CRTL|ZeroPoint|Red Team, C2, Evasion|Phase 5|

---

## RESSOURCES RAM — DÉMARRAGE PAR SCÉNARIO

|Scénario|VMs actives|RAM|
|---|---|---|
|AD lab seul|OPNsense+DC01+CL01|10 Go|
|SOC actif|+Wazuh+Zabbix|18 Go|
|Red Team|+Kali+C2|26 Go|
|Avec DC02+Matrix|+DC02+Matrix|30 Go|
|Tout (max)|Toutes|~40 Go ⚠️|

> Ne jamais dépasser 28-30 Go actifs simultanément sur 32 Go RAM hôte.