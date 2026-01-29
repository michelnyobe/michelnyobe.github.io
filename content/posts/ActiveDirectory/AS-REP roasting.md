---
title: "AS-REP roasting"
date: 2026-28-01
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


# mettre en place une vulnerabilité AS-REP Roasting
# test de l'attaque 
# Detection avec elastic 
# comment se prevenir 







Lorsque l’authentification se produit dans Kerberos, la première chose qui se produit est une demande d’authentification au contrôleur de domaine afin que l’identité qui tente de s’authentifier puisse être vérifiée.
Cette demande est connue sous le nom de demande de serveur d’authentification (AS-REQ).
Une fois l'authentification du client validée, le contrôleur de domaine envoie une réponse du serveur d'authentification (AS-REP) au client, contenant une clé de session et un ticket d'octroi de ticket (TGT).
AS-REP Roasting vide les hachages krbasrep5 des comptes utilisateurs dont la pré-authentification Kerberos est désactivée. Contrairement à Kerberoasting, ces utilisateurs ne doivent pas nécessairement être des comptes de service. La seule condition pour pouvoir effectuer AS-REP Roasting sur un utilisateur est que la pré-authentification soit désactivée.


```
rubeus.exe asreproast
```

Déchiffrez ces hachages avec hashcat - 

1.) Transférez le hachage de la machine cible vers votre machine attaquante et placez le hachage dans un fichier txt

2.) Insérez 23$ après $krb5asrep$ afin que la première ligne soit $krb5asrep$23$User.....

Utilisez la même liste de mots que celle que vous avez téléchargée dans la tâche 4

3.)  `hashcat -m 18200 hash.txt Pass.txt` - Décryptez ces hachages ! La torréfaction Rubeus AS-REP utilise le mode hashcat 18200.

## Enumeration - Identification des comptes vulnérables

Au cours de cette phase, vous identifiez les comptes utilisateurs Active Directory pour lesquels la pré-authentification Kerberos est désactivée. Les comptes sans pré-authentification permettent à tout utilisateur du réseau de demander un ticket Kerberos (notamment un AS-REP) sans avoir à prouver son identité.
**[Rubeus](https://github.com/GhostPack/Rubeus)**
**[GetNPUsers.py d'Impacket](https://github.com/fortra/impacket)**

```
GetNPUsers.py tryhackme.loc/ -dc-ip 10.211.12.10 -usersfile users.txt -format hashcat -outputfile hashes.txt -no-pass
```





https://github.com/fortra/impacket/blob/master/examples/GetNPUsers.py
https://www.hackthebox.com/blog/as-rep-roasting-detection
https://netwrix.com/en/cybersecurity-glossary/cyber-security-attacks/as-rep-roasting/