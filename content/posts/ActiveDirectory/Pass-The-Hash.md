---
title: "pass the hash"
date: 2026-03-06T10:00:00+02:00
draft: true
tags: ["pentest", "Active Directory", "PowerView", "reconnaissance", "CRTP", "red team"]
categories: ["Red Team", "pentester"]
summary: ""
showToc: true
tocOpen: true
---
## Qu'est-ce qu'une attaque Pass the Hash ?

L'attaque « Pass the Hash » est répandue dans les environnements Windows. Elle exploite le mécanisme d'authentification du système d'exploitation Windows.

Pour éviter de stocker les mots de passe en clair, Windows conserve les hachages NTLM des mots de passe des utilisateurs. Lors du processus d'authentification, Windows compare ces hachages plutôt que les mots de passe en clair. L'attaque « Pass the Hash » exploite ce mécanisme.

Au lieu de voler et d'utiliser des mots de passe en clair, les pirates se concentrent sur l'obtention et l'utilisation des hachages pour s'authentifier sur diverses ressources réseau. L'attaque « Pass the Hash » est furtive et très efficace.


Lors d'une attaque par « pass the Hash », un attaquant commence par accéder au hachage d'un utilisateur, souvent en vidant le contenu de la base de données SAM du système ou de la mémoire, à l'aide d'outils comme Mimikatz.

## Pass the Hash avec Mimikatz (Windows)

Mimikatz possède un module nommé sekurlsa::pth qui nous permet d'effectuer une attaque Pass the Hash en démarrant un processus à l'aide du hash du mot de passe de l'utilisateur. Pour utiliser ce module, nous aurons besoin des éléments suivants :

	- /user - Le nom de l'utilisateur dont nous voulons usurper l'identité.
	- /rc4 ou /NTLM - Le hash NTLM du mot de passe de l'utilisateur.
	- /domain - Le domaine auquel appartient l'utilisateur à usurper. Dans le cas d'un compte d'utilisateur local, nous pouvons utiliser le nom de l'ordinateur, localhost, ou un point (.).
	- /run - Le programme que nous voulons exécuter dans le contexte de l'utilisateur (s'il n'est pas spécifié, il lancera cmd.exe).

## Pass the Hash avec PowerShell Invoke-TheHash (Windows)

Un autre outil que nous pouvons utiliser pour effectuer des attaques Pass the Hash sur Windows est Invoke-TheHash. Cet outil est une collection de fonctions PowerShell pour effectuer des attaques Pass the Hash avec WMI et SMB. Les connexions WMI et SMB sont accessibles via le .NET TCPClient. L'authentification est réalisée en passant un hash NTLM dans le protocole d'authentification NTLMv2.

Lors de l'utilisation de Invoke-TheHash, nous avons deux options : l'exécution de commandes SMB ou WMI. Pour utiliser cet outil, nous devons spécifier les paramètres suivants pour exécuter des commandes sur l'ordinateur cible :

	- Target - Nom d'hôte ou adresse IP de la cible.
	- Username - Nom d'utilisateur à utiliser pour l'authentification.
	- Domain - Domaine à utiliser pour l'authentification. Ce paramètre n'est pas nécessaire avec les comptes locaux ou lors de l'utilisation de @domaine après le nom d'utilisateur.
	- Hash - Hash NTLM du mot de passe pour l'authentification. Cette fonction acceptera le format LM:NTLM ou NTLM.
	- Command - Commande à exécuter sur la cible. Si aucune commande n'est spécifiée, la fonction vérifiera si le nom d'utilisateur et le hash ont accès à WMI sur la cible.

nous pouvons exécuter Invoke-TheHash pour exécuter notre script de shell inversé PowerShell sur l'ordinateur cible. 


## Pass the Hash avec Impacket (Linux)

Impacket dispose de plusieurs outils que nous pouvons utiliser pour différentes opérations telles que l'Exécution de Commandes et le Vidage d'Identifiants, l'Énumération, etc. Pour cet exemple, nous allons exécuter une commande sur la machine cible en utilisant PsExec.

```
impacket-psexec administrator@10.129.201.126 -hashes :30B3783CE2ABF1AF70F77D0660CF3453
```

## Pass the Hash avec NetExec (Linux)


## Pass the Hash avec evil-winrm (Linux)

Evil-WinRM est un autre outil que nous pouvons utiliser pour nous authentifier en utilisant l'attaque Pass the Hash avec l'accès à distance PowerShell (PowerShell Remoting). Si SMB est bloqué ou si nous n'avons pas de droits administratifs, nous pouvons utiliser ce protocole alternatif pour nous connecter à la machine cible.

## Pass the Hash avec RDP (Linux)

Il y a quelques mises en garde à cette attaque :

Le Mode d'Administration Restreinte (Restricted Admin Mode), qui est désactivé par défaut, doit être activé sur l'hôte cible ; sinon, l'erreur suivante vous sera présentée :

Cela peut être activé en ajoutant une nouvelle clé de registre DisableRestrictedAdmin (REG_DWORD) sous HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Lsa avec la valeur 0. Cela peut être fait en utilisant la commande suivante :

Enable Restricted Admin Mode to allow PtH


```
reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f
```


# Pass the Ticket (PtT) depuis Windows
Dans cette attaque, nous utilisons un ticket Kerberos volé pour nous déplacer latéralement au lieu d'un hash de mot de passe NTLM. Nous aborderons plusieurs façons de réaliser une attaque PtT depuis Windows et Linux. Dans cette section, nous nous concentrerons sur les attaques depuis Windows, et dans la section suivant

## Attaque Pass the Ticket (PtT)

Nous avons besoin d'un ticket Kerberos valide pour effectuer une attaque Pass the Ticket (PtT). Il peut s'agir de :

Ticket de Service (TGS), pour autoriser l'accès à une ressource particulière.
Ticket d'Octroi de Ticket (TGT), que nous utilisons pour demander des tickets de service afin d'accéder à toute ressource pour laquelle l'utilisateur a des privilèges.
