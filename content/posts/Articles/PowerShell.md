---
title: PowerShell
date: 2025-05-12T10:00:00+02:00
draft: false
tags:
  - PowerShell
  - Windows
categories:
  - Blog
summary: PowerShell
showToc: true
tocOpen: true
---
# Introduction à PowerShell

PowerShell est à la fois un langage de script puissant et un environnement en ligne de commande pour les systèmes Windows. Il est construit sur le framework .NET, ce qui lui permet de manipuler des objets complexes directement en ligne de commande.

---

# Commandes PowerShell de base

## Utilisation de `Get-Help`

La commande `Get-Help` affiche des informations sur une _cmdlet_ (commande PowerShell). Pour obtenir de l’aide sur une commande spécifique :

```powershell
Get-Help Nom-De-La-Commande
```

Pour voir des exemples d'utilisation concrets :

```powershell
Get-Help Nom-De-La-Commande -Examples
```

---

## Utilisation de `Get-Command`

`Get-Command` permet de lister toutes les cmdlets disponibles sur le système. Elle supporte également les filtres par motif :

```powershell
Get-Command Verb-*
Get-Command *-Noun
```

Par exemple :

```powershell
Get-Command New-*
```

affichera toutes les cmdlets commençant par le verbe `New`.

---

## Le pipeline (`|`)

Le pipeline (`|`) permet de transmettre la sortie d’une commande comme entrée à une autre. Contrairement aux autres shells (comme Bash), PowerShell passe des objets, et non du texte.

Exemple :

```powershell
Get-Command | Get-Member -MemberType Method
```

Cela permet d’examiner les méthodes disponibles sur les objets retournés par `Get-Command`.

---

## Création d’objets avec `Select-Object`

Pour manipuler les résultats d’une commande et ne conserver que certaines propriétés :

```powershell
Get-ChildItem | Select-Object -Property Mode, Name
```

---

## Filtrage des objets avec `Where-Object`

Pour filtrer les objets selon des critères précis :

```powershell
Get-Process | Where-Object -Property CPU -GT 100
```

Ou en notation script block :

```powershell
Get-Process | Where-Object { $_.CPU -gt 100 }
```

### Opérateurs courants :

- `-Contains` : contient exactement la valeur
    
- `-EQ` : égal
    
- `-GT` : supérieur
    

---

## Trier les objets avec `Sort-Object`

Pour trier la sortie d’une commande :

```powershell
Get-Process | Sort-Object CPU
```

---

# Quelques commandes utiles

### Rechercher un fichier dans un dossier

```powershell
Get-ChildItem -Path "C:\Users\Michel\Documents" -Recurse -Filter "rapport.docx"
```

### Obtenir le répertoire de travail actuel

```powershell
Get-Location
```

### Faire une requête HTTP

```powershell
Invoke-WebRequest -Uri "https://example.com"
```

---

# Énumération système

### Nombre d’utilisateurs locaux

```powershell
Get-LocalUser
```

### Informations détaillées sur tous les comptes utilisateur

```powershell
Get-WmiObject -Class Win32_UserAccount
```

### Obtenir l’adresse IP locale

```powershell
Get-NetIPAddress
```

### Voir les ports à l’écoute

```powershell
Get-NetTCPConnection | Where-Object { $_.State -eq "Listen" }
```

### Lister tous les processus en cours

```powershell
Get-Process
```

### Lister toutes les tâches planifiées

```powershell
Get-ScheduledTask
```

### Obtenir la liste de contrôle d’accès (ACL)

```powershell
Get-Acl -Path "C:\chemin\du\dossier-ou-fichier"
```

---

# Pour aller plus loin

- [https://learnxinyminutes.com/powershell/](https://learnxinyminutes.com/powershell/)
    
- [https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/where-object](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/where-object)