---
title: "Infrastructure d'entreprise"
date: 2025-08-21
draft: true
tags: ["Active Directory", "blog", "SID", "attaques", "windows"]
categories: ["Active Directory"]
summary: "Un identificateur de sécurité (SID) est essentiel dans la gestion des accès et de la sécurité dans les systèmes Windows. Découvrez comment il fonctionne, son rôle dans l'authentification et sa structure binaire."
showToc: true
tocOpen: true
---

### Réseau

- - **Forêt 1 : `corp.local`**
    
    - Domaine racine : `corp.local`
        
    - Domaine enfant : `reseau.corp.local` (ex. département IT)
        
- **Forêt 2 : `filiale.local`**
    
    - Domaine racine : `filiale.local`
        
    - Pas forcément besoin de domaine enfant (mais possible si tu veux un `hr.filiale.local`)
        
- **Sous-réseaux VLAN**
    
    - VLAN 10 → Admin (contrôleurs de domaine, PKI, SIEM)
        
    - VLAN 20 → Utilisateurs (Windows 10/11 clients)
        
    - VLAN 30 → Serveurs applicatifs (BD, messagerie, web interne)
        
    - VLAN 40 → Sécurité (pare-feu, IDS/IPS, SOC tools)
        
    - VLAN 50 → Red Team (Kali, C2, vulnérable server)
        

---

### 💻 Machines (minimum 12–15)

1. **Contrôleurs de domaine (DC)**
    
    - `DC1` (Forest Root, DNS, DHCP)
        
    - `DC2` (backup GC, LDAP, réplication)
        
2. **Serveur de fichiers et partage**
    
    - `FS1` avec DFS + quotas
        
3. **Serveur de messagerie**
    
    - `EXCH1` avec **Exchange Server** (ou Postfix/Dovecot en alternative open source)
        
4. **Serveur de base de données**
    
    - `DB1` : Microsoft SQL Server (pour AD apps, ERP)
        
    - Option : MySQL/PostgreSQL pour une appli web interne
        
5. **Serveur de certificats** (PKI)
    
    - `CA1` : Active Directory Certificate Services (ACCS) pour les certificats internes (serveurs, utilisateurs, VPN).
        
6. **Serveur SIEM & SOC**
    
    - `SIEM1` : Splunk / ELK (Elasticsearch + Logstash + Kibana) ou Wazuh
        
    - Collecte logs Windows, Sysmon, firewall, IDS, messagerie
        
7. **Serveur de supervision & monitoring**
    
    - `MON1` : Zabbix / Nagios
        
8. **Pare-feux & IDS/IPS**
    
    - `FW1` : pfSense/OPNsense (pare-feu, NAT, VPN)
        
    - `IDS1` : Suricata ou Snort
        
9. **Machines utilisateurs (clients)**
    
    - `PC1` → Employé Marketing (Win10/11)
        
    - `PC2` → Employé RH (Win10/11)
        
    - `PC3` → Employé Finance (Win10/11)
        
    - `PC4` → Développeur (Win11 + VSCode, accès BD)
        
10. **Serveur d’applications métier**
    
    - `APP1` : ERP (Odoo ou Dynamics) ou appli web interne
        
11. **Machine Attaque (Red Team)**
    
    - `Kali` Linux avec outils (C2, BloodHound, mimikatz, etc.)
        

---

### 🔒 Services et intégrations

- **Active Directory** : gestion IAM, GPO, Kerberos, LDAP
    
- **PKI** : certificats pour serveurs + utilisateurs (authentification)
    
- **Messagerie** : Exchange / Postfix
    
- **SIEM** : collecte Sysmon, firewall logs, audit AD
    
- **Base de données** : SQL Server, PostgreSQL
    
- **Pare-feux** : pfSense entre VLANs
    
- **IDS/IPS** : Suricata pour détection réseau
    
- **Red Team** : Kali dans le VLAN attaquant, accès limité via FW
    
- **Monitoring** : Zabbix (alertes perf + sécurité)


## 🏰 1. Attaques Active Directory (AD)

Ton lab avec 2 forêts + trust est parfait pour ça.  
Tu pourras tester :

- **Reconnaissance & Enumeration**
    
    - LDAP / SMB enumeration, BloodHound, SharpHound
        
    - Découverte d’objets sensibles (users, groups, GPO, trusts)
        
- **Credential Attacks**
    
    - Pass-the-Hash (NTLM)
        
    - Pass-the-Ticket (Kerberos TGT/TGS)
        
    - Kerberoasting (service tickets crackés offline)
        
    - AS-REP Roasting (users sans pré-auth Kerberos)
        
- **Escalation & Persistence**
    
    - Abuse des délégations Kerberos (Unconstrained/Constrained/RBCD)
        
    - DCsync / DCshadow
        
    - Golden Ticket / Silver Ticket
        
    - Shadow Credentials (ADCS abuse si tu as installé une PKI)
        
- **Trust Exploitation (inter-forêt)**
    
    - Exploitation des relations d’approbation (`corp.local` ↔ `filiale.local`)
        
    - Mouvements latéraux entre forêts
        

➡️ Avec ce lab : **20+ attaques AD réalistes**

---

## 🎣 2. Phishing & Initial Access

Comme tu as un **serveur de messagerie (Exchange ou Postfix)** + clients Windows :

- Phishing avec macros malveillantes (Word, Excel)
    
- Pièces jointes piégées (HTA, VBS, EXE)
    
- Malicious links (rediriger vers un site clone OWA/SharePoint)
    
- Credential harvesting (phishing login page interne/externe)
    

➡️ Tu peux tester **10+ scénarios de phishing** réalistes

---

## ☁️ 3. Attaques Azure / Cloud

Si tu relies ton lab à **Azure AD (Entra ID)** :

- Password spraying & brute force sur comptes O365
    
- Consent phishing (application malveillante OAuth)
    
- Token replay (vol d’access/refresh token)
    
- Abus de **permissions mal configurées** (Role Assignments, Service Principals)
    
- Persistence via **App Registrations** ou **Conditional Access bypass**
    

➡️ Environ **8–12 attaques cloud** exploitables

---

## 🛡 4. Évasion AV / EDR

Comme tu installes un EDR ou un AV (Defender, Wazuh, Velociraptor, etc.), tu peux t’entraîner à :

- Obfuscation PowerShell (Invoke-Obfuscation)
    
- LOLBins (utiliser `certutil`, `regsvr32`, `mshta` pour bypass AV)
    
- Payloads chiffrés (Cobalt Strike beacon, Sliver implant)
    
- In-Memory execution (Reflective DLL injection, SharpShooter)
    
- Process hollowing / injection
    
- Bypass AMSI & ScriptBlock Logging
    

➡️ Tu as facilement **15–20 techniques d’évasion** testables

---

## 🔢 Bilan global

Avec ce lab, tu peux pratiquer :

- **AD** → ~20 attaques
    
- **Phishing** → ~10 attaques
    
- **Azure/Cloud** → ~10 attaques
    
- **Evasion AV/EDR** → ~15 attaques
    

👉 Total : **55+ scénarios réalistes**, couvrant **Red Team, Blue Team et Purple Team**.

## **Semaine 1 – AD Basics & Recon**

🎯 Objectif : comprendre ton AD et lancer les attaques fondamentales

- **Jour 1** :
    
    - Mise en place du lab (2 forêts, DCs, 4 clients, 1 serveur Exchange, 1 serveur DB, SIEM, pfSense, Kali)
        
    - Vérification DNS, LDAP, trusts
        
- **Jour 2** :
    
    - Enumeration LDAP/SMB (`ldapsearch`, `rpcclient`, `enum4linux-ng`)
        
    - BloodHound + SharpHound → cartographie du domaine `corp.local`
        
- **Jour 3** :
    
    - Attaque Kerberos : AS-REP Roasting
        
    - Cracking avec Hashcat
        
- **Jour 4** :
    
    - Kerberoasting + exploitation SPN
        
- **Jour 5** :
    
    - Pass-the-Hash (psexec, wmiexec)
        
- **Jour 6** :
    
    - Abus des groupes locaux (Admin locaux, RDP)
        
- **Jour 7** :
    
    - Revue Blue Team (Sysmon + Wazuh/Splunk → détection bruteforce & Kerberoasting)
        

---

## 🔹 **Semaine 2 – Escalade & Trusts**

🎯 Objectif : attaquer les relations inter-domaines & forêts

- **Jour 8** :
    
    - Pass-the-Ticket (TGT/TGS replay)
        
- **Jour 9** :
    
    - Golden Ticket + Silver Ticket
        
- **Jour 10** :
    
    - DCsync & DCshadow
        
- **Jour 11** :
    
    - Abus ADCS (Shadow Credentials, ESC1-ESC8 scénarios)
        
- **Jour 12** :
    
    - Exploitation Constrained Delegation (RBCD)
        
- **Jour 13** :
    
    - Pivot vers forêt `filiale.local` via trust inter-forest
        
- **Jour 14** :
    
    - Détection dans SIEM : DCsync, Golden Ticket
        

---

## 🔹 **Semaine 3 – Phishing & Azure**

🎯 Objectif : simuler un accès initial via phishing, puis exploitation cloud

- **Jour 15** :
    
    - Setup serveur Exchange + clients Outlook
        
    - Envoi d’un phishing avec pièce jointe Word + macro
        
- **Jour 16** :
    
    - Malicious link phishing (OWA fake login page)
        
- **Jour 17** :
    
    - Credential harvesting (phishing creds) + réutilisation AD
        
- **Jour 18** :
    
    - Password spraying sur O365/Azure AD
        
- **Jour 19** :
    
    - Consent phishing (malicious OAuth App)
        
- **Jour 20** :
    
    - Exploitation d’un service principal Azure mal configuré (App registration)
        
- **Jour 21** :
    
    - Revue Blue Team : logs Exchange + Azure AD sign-ins
        

---

## 🔹 **Semaine 4 – Evasion AV/EDR + SOC**

🎯 Objectif : bypass AV/EDR et tester détection

- **Jour 22** :
    
    - AMSI bypass (PowerShell obfuscation)
        
- **Jour 23** :
    
    - LOLBins (certutil, mshta, regsvr32)
        
- **Jour 24** :
    
    - Payload chiffré + exécution en mémoire (Cobalt Strike/Sliver beacon)
        
- **Jour 25** :
    
    - Process injection (hollowing)
        
- **Jour 26** :
    
    - Persistence via Scheduled Tasks / Registry Run keys
        
- **Jour 27** :
    
    - Evasion AV signature avec custom payload
        
- **Jour 28** :
    
    - Revue SOC :
        
        - Logs Defender / Sysmon / Wazuh
            
        - Corrélation Splunk
            
- **Jour 29** :
    
    - Simulation attaque complète :
        
        - Phishing → foothold → lateral movement → DCsync
            
- **Jour 30** :
    
    - Purple Teaming :
        
        - Attaques vs détection
            
        - Rapport final (quelles attaques détectées, lesquelles sont passées)