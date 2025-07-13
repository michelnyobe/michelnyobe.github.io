---
title: "AS-REP roasting"
date: 2025-07-11T20:00:00+02:00
draft: false
tags: ["AD", "kerberos", "autoapprentissage"]
categories: ["AD"]
summary: "Entra ID"
showToc: true
tocOpen: true
---

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