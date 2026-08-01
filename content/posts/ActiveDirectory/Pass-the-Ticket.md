---
title: "pass the ticket"
date: 2026-03-06T10:00:00+02:00
draft: true
tags: ["pentest", "Active Directory", "PowerView", "reconnaissance", "CRTP", "red team"]
categories: ["Red Team", "pentester"]
summary: ""
showToc: true
tocOpen: true
---

Dans cette attaque, nous utilisons un ticket Kerberos volé pour nous déplacer latéralement au lieu d'un hash de mot de passe NTLM.

# Attaque Ptt sur Windows 
## Attaque Pass the Ticket (PtT)

Nous avons besoin d'un ticket Kerberos valide pour effectuer une attaque Pass the Ticket (PtT). Il peut s'agir de :

	- Ticket de Service (TGS), pour autoriser l'accès à une ressource particulière.
	- Ticket d'Octroi de Ticket (TGT), que nous utilisons pour demander des tickets de service afin d'accéder à toute ressource pour laquelle l'utilisateur a des privilèges.

## Pass the Key (alias OverPass the Hash)

La technique traditionnelle Pass the Hash (PtH) consiste à réutiliser un hash de mot de passe NTLM qui n'interagit pas avec Kerberos. L'approche Pass the Key (alias OverPass the Hash) convertit un hash/une clé (rc4_hmac, aes256_cts_hmac_sha1, etc.) pour un utilisateur joint au domaine en un Ticket d'Octroi de Ticket (TGT) complet. Cette technique a été développée par Benjamin Delpy et Skip Duckwall dans leur présentation Abusing Microsoft Kerberos - Sorry you guys don't get it. Will Schroeder a également adapté leur projet pour créer l'outil Rubeus.

## Collecte de tickets Kerberos depuis Windows

Sous Windows, les tickets sont traités et stockés par le processus LSASS (Local Security Authority Subsystem Service). Par conséquent, pour obtenir un ticket d'un système Windows, vous devez communiquer avec LSASS et le demander.
En tant qu'utilisateur non-administratif, vous ne pouvez obtenir que vos propres tickets, mais en tant qu'administrateur local, vous pouvez tout collecter.
Nous pouvons collecter tous les tickets d'un système en utilisant le module sekurlsa::tickets /export de Mimikatz. Le résultat est une liste de fichiers avec l'extension .kirbi, qui contiennent les tickets.

```
mimikatz
mimikatz # privilege::debug
mimikatz # sekurlsa::tickets /export
```

> Note : Au moment de la rédaction, en utilisant Mimikatz version 2.2.0 20220919, si nous exécutons ```sekurlsa::ekeys``` , il présente tous les hashs comme des_cbc_md4 sur certaines versions de Windows 10

Nous pouvons également exporter des tickets en utilisant Rubeus et l'option dump. Cette option peut être utilisée pour vider tous les tickets (si elle est exécutée en tant qu'administrateur local). Rubeus dump, au lieu de nous donner un fichier, affichera le ticket encodé au format Base64. Nous ajoutons l'option /nowrap pour faciliter le copier-coller.

```
 Rubeus.exe dump /nowrap
```

> Note : Pour collecter tous les tickets, nous devons exécuter Mimikatz ou Rubeus en tant qu'administrateur.

## Pass the Key (alias OverPass the Hash)

Pour forger nos tickets, nous devons avoir le hash de l'utilisateur ; nous pouvons utiliser Mimikatz pour vider toutes les clés de chiffrement Kerberos des utilisateurs en utilisant le module sekurlsa::ekeys. Ce module énumérera tous les types de clés présents pour le paquet Kerberos.

Maintenant que nous avons accès aux clés AES256_HMAC et RC4_HMAC, nous pouvons effectuer l'attaque OverPass the Hash (alias Pass the Key) en utilisant Mimikatz et Rubeus.

```
mimikatz.exe
mimikatz # privilege::debug
mimikatz # sekurlsa::pth /domain:inlanefreight.htb /user:plaintext /ntlm:3f74aa8f08f712f09cd5177b5c1ce50f
```

Cela créera une nouvelle fenêtre cmd.exe que nous pourrons utiliser pour demander l'accès à n'importe quel service souhaité dans le contexte de l'utilisateur cible.

Pour forger un ticket avec Rubeus, nous pouvons utiliser le module asktgt avec le nom d'utilisateur, le domaine et le hash, qui peut être /rc4, /aes128, /aes256, ou /des. 

```
Rubeus.exe asktgt /domain:inlanefreight.htb /user:plaintext /aes256:b21c99fc068e3ab2ca789bccbef67de43791fd911c6e15ead25641a8fda3fe60 /nowrap
```


## Pass the Ticket (PtT)

Avec Rubeus, nous avons effectué une attaque OverPass the Hash et récupéré le ticket au format Base64. Nous aurions pu utiliser l'option /ptt pour soumettre le ticket (TGT ou TGS) à la session de connexion actuelle.

```
Rubeus.exe asktgt /domain:inlanefreight.htb /user:plaintext /rc4:3f74aa8f08f712f09cd5177b5c1ce50f /ptt
```


```
Rubeus.exe ptt /ticket:[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi
```


# Attaque Ptt sur Linux
## Identifier l'intégration de Linux et Active Directory

Nous pouvons identifier si la machine Linux est jointe au domaine en utilisant realm, un outil utilisé pour gérer l'adhésion du système à un domaine et définir quels utilisateurs ou groupes du domaine sont autorisés à accéder aux ressources du système local.

```
realm list
```

# Abuser des fichiers KeyTab
En tant qu'attaquants, nous pouvons avoir plusieurs usages pour un fichier keytab. La première chose que nous pouvons faire est d'usurper l'identité d'un utilisateur en utilisant kinit. Pour utiliser un fichier keytab, nous devons savoir pour quel utilisateur il a été créé. klist est une autre application utilisée pour interagir avec Kerberos sous Linux. Cette application lit les informations d'un fichier keytab

## Usurper l'identité d'un utilisateur avec un KeyTab

```
klist
kinit carlos@INLANEFREIGHT.HTB -k -t /opt/specialfiles/carlos.keytab
```