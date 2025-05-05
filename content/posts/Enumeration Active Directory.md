---
title: Reconnaissance Active Directory avec PowerView
date: 2025-05-05
tags:
  - Active
  - Directory
  - CRTP
  - PowerView
  - Reconnaissance
  - Pentest
categories:
  - Cybersecurity
  - Red Team
---


## 📁 1. Domaine et Forêts

### 🔍 Obtenir le domaine actuel
```powershell
Get-Domain                # PowerView
Get-ADDomain              # Module ActiveDirectory
```

### 🌐 Interroger un autre domaine
```powershell
Get-Domain -Domain example.local
Get-ADDomain -Identity example.local
```

### 🆔 Récupérer le SID du domaine
```powershell
Get-DomainSID
(Get-ADDomain).DomainSID
```

### 🖥️ Récupérer les contrôleurs de domaine
```powershell
Get-DomainController
Get-ADDomainController
```

### 🤝 Obtenir les relations d’approbation (trusts)
```powershell
Get-DomainTrust
Get-ADTrust
```

### 🌲 Informations sur la forêt
```powershell
Get-Forest
Get-ADForest
```

### 📜 Lister tous les domaines d’une forêt
```powershell
Get-ForestDomain
(Get-ADForest).Domains
```

### 🗺️ Cartographier les fiducies d'une forêt
```powershell
Get-ForestTrust
Get-ADTrust -Filter 'ms-DS-TrustForestTrustInfo -ne "$null"'
```

---

## 👤 2. Utilisateurs

### 👥 Liste des utilisateurs
```powershell
Get-DomainUser
Get-ADUser -Filter * -Properties *
```

### 🔍 Détails d’un utilisateur spécifique
```powershell
Get-DomainUser -Identity user1 -Properties *
Get-ADUser -Identity user1 -Properties *
```

### 🔄 Propriétés utiles
```powershell
Get-ADUser -Filter * -Properties * | select -First 1 | Get-Member -MemberType *Property | select Name
```

### 👑 Membres du groupe « Enterprise Admins »
```powershell
Get-DomainGroupMember -Identity "Enterprise Admins" -Recurse
```

### 🖥️ Groupes locaux d'une machine (privilèges requis)
```powershell
Get-NetLocalGroup -ComputerName dcorp-dc
Get-NetLocalGroup -ComputerName dcorp-dc -GroupName Administrators
```

### 🔎 Recherche d’accès administrateur local
```powershell
Find-LocalAdminAccess -Verbose
```

### 🔐 Comptes dont le mot de passe n’expire jamais
```powershell
Get-ADUser -Filter {PasswordNeverExpires -eq $true}
```

### 🎭 Comptes avec délégation Kerberos activée
```powershell
Get-ADUser -Filter {TrustedForDelegation -eq $true -or TrustedToAuthForDelegation -eq $true}
```

### 🧾 Droits de l'utilisateur actuel
```powershell
whoami /priv
```

---

## 👪 3. Groupes

### 📋 Lister les groupes
```powershell
Get-DomainGroup
Get-ADGroup -Filter * -Properties *
```

### 🔍 Rechercher des groupes contenant "admin"
```powershell
Get-DomainGroup *admin*
Get-ADGroup -Filter 'Name -Like "*admin*"'
```

### 👑 Membres « Domain Admins »
```powershell
Get-DomainGroupMember -Identity "Domain Admins" -Recurse
```

---

## 🖥️ 4. Machines & Serveurs

### 📜 Liste des ordinateurs
```powershell
Get-DomainComputer
Get-ADComputer -Filter * -Properties *
```

### 🔍 Recherche spécifique (ex. Server 2022)
```powershell
Get-DomainComputer -OperatingSystem "*Server 2022*"
```

### 📡 Test de connectivité
```powershell
Get-ADComputer -Filter * -Properties DNSHostName | % { Test-Connection -Count 1 -ComputerName $_.DNSHostName }
```

### 👁️ Localisation des sessions d’un utilisateur
```powershell
Find-DomainUserLocation -UserGroupIdentity "RDPUsers"
```

---

## 🔐 5. ACL et Permissions

### 🔍 Obtenir les ACL d’un objet
```powershell
Get-DomainObjectAcl -SamAccountName user1 -ResolveGUIDs
```

### 🧠 Rechercher des ACE intéressants
```powershell
Find-InterestingDomainAcl -ResolveGUIDs
```

### 📂 ACL sur un chemin spécifique
```powershell
Get-PathAcl -Path "\\dcorp-dc.dollarcorp.moneycorp.local\sysvol"
```

---

## 📂 6. Autres Techniques

### 📂 Découverte de serveurs de fichiers
```powershell
Get-NetFileServer
```

---

## 📚 Ressources utiles

- [PowerView Documentation (PowerSploit)](https://powersploit.readthedocs.io/en/latest/Recon/)
- [Cmdlets Active Directory Microsoft](https://learn.microsoft.com/en-us/powershell/module/activedirectory/)
