---
title: "AS-REP roasting"
date: 2026-01-28
draft: false
tags: ["AD", "kerberos", "autoapprentissage" ,"brute force"]
categories: ["Attaque AD"]
summary: "AS-REP Roasting"
showToc: true
tocOpen: true
---

# Introduction 

Lorsqu'une authentification a lieu au sein de Kerberos, la première chose qui se produit est une requête d'authentification adressée au contrôleur de domaine afin que l'identité de la personne tentant de s'authentifier puisse être vérifiée. 
Cette requête est connue sous le nom de requête au serveur d'authentification (AS-REQ.). 
Ce processus est communément appelé préauthentification Kerberos.

# Pre-authentification Kerberos

![preauthentification](/images/preauthentificationkerberos.png)

## definition 

La pré-authentification Kerberos est une étape obligatoire de la requête d'authentification Kerberos initiale. Le client chiffre un horodatage avec le hachage de son mot de passe afin de prouver son identité au serveur d'authentification (AS).Ce processus a lieu avant que le KDC n'émette un ticket d'autorisation (TGT) au client demandeur.

## Fonctionnement 

Le processus de pré-authentification suit une séquence spécifique qui valide l'identité du client avant de diffuser tout matériel chiffré susceptible d'être utilisé pour des attaques hors ligne.

### Phase de demande du client

Le client initie l'authentification en envoyant un message AS_REQ au serveur d'authentification. Cette requête inclut un horodatage chiffré avec le hachage du mot de passe du client, prouvant ainsi que ce dernier connaît le mot de passe correct sans le transmettre en clair.

### Processus de vérification du serveur

Le serveur d'authentification reçoit la requête AS_REQ et tente de déchiffrer l'horodatage à l'aide du hachage du mot de passe de l'utilisateur stocké dans sa base de données. Le serveur effectue deux validations critiques au cours de ce processus.

Premièrement, il vérifie que le déchiffrement a réussi, confirmant ainsi que le client possède le hachage de mot de passe correct. Deuxièmement, il vérifie que l'horodatage se situe dans les paramètres de décalage horaire acceptables, généralement cinq minutes par défaut.

Une fois l'authentification préalable réussie, le serveur d'authentification (AS) émet un ticket de service chiffré (TGT) et une clé de session au client. Ce TGT permet au client de demander des tickets de service pour des ressources spécifiques sans avoir à ressaisir ses identifiants.

# c'est quoi AS-REP Roasting

L'attaque **AS-REP Roasting**  est une technique d'exploitation du protocole Kerberos qui cible les utilisateurs n'ayant pas l'option « L'authentification préalable Kerberos n'est pas requise » (Do not require Kerberos preauthentication) activée. Normalement, un utilisateur doit chiffrer un horodatage avec son mot de passe pour prouver son identité avant que le contrôleur de domaine (KDC) ne lui réponde. Si cette sécurité est désactivée, un attaquant peut envoyer une simple requête de ticket (**AS-REQ**) au nom de la victime, et le KDC renverra immédiatement un message **AS-REP** contenant une partie chiffrée avec le hash du mot de passe de l'utilisateur. L'attaquant peut alors récupérer ce message hors ligne et tenter de "cracker" le mot de passe par force brute ou dictionnaire sans jamais interagir à nouveau avec le réseau, ce qui rend l'attaque très discrète.

# mettre en place une vulnerabilité AS-REP Roasting

## creation d'un utilisateur 

Dans AD DS, vous devez fournir à tous les utilisateurs qui ont besoin d’accéder aux ressources réseau un compte utilisateur. Avec ce compte d’utilisateur, les utilisateurs peuvent s’authentifier auprès du domaine AD DS et accéder aux ressources réseau.
Dans Windows Server, un compte d’utilisateur est un objet qui contient toutes les informations qui définissent un utilisateur. 

- Le nom d’utilisateur.
- Un mot de passe utilisateur.
- Les appartenances aux groupes.


![usercreate](/images/usercreate.png)
 j'ai coché la case 

# test de l'attaque 

pour notre lab nous allons utiliser un script pour generer des utilisateursuffer/vulnerable-AD
## Linux 
### Enumeration
### enumeration du domaine 
outil : Nmap 



nous avons  le domaine 

### enumeration des utilisateurs 


Pour mettre en œuvre la technique AS-REP Roasting, il est nécessaire d'énumérer les comptes vulnérables. ADSearch est un outil capable d'effectuer des requêtes LDAP afin d'énumérer les objets Active Directory.
 outils :  Impacket GetNPUsers ( python)
 
```
crackmapexec smb IP -u UserNme -p 'Mot de passe ' --users | grep -oP 'Domain\\\K[^ ]+' > users.txt
```

![usercreate](/images/enuusers.png)

 maintenant les utilisateur qui ont 

```
Python GetNPUsers.py nyoma.local/ -usersfile users.txt
```


![usercreate](/images/aesr.png)


### brute force 

```
john --wordlist=rockyou.txt asrep_hashes.txt 
```


![usercreate](/images/brtuf.png)


# Detection avec elastic 


# comment se prevenir 








https://github.com/fortra/impacket/blob/master/examples/GetNPUsers.py
https://www.hackthebox.com/blog/as-rep-roasting-detection
https://netwrix.com/en/cybersecurity-glossary/cyber-security-attacks/as-rep-roasting/