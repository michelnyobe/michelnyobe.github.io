---
title: "Protocoles de gestion à distance"
date: 2025-06-30
draft: false
tags: ["Linux", "CPTS HTB", "linux"]
categories: ["CPTS HTB"]
summary: "Protocoles de gestion à distance Linux"
showToc: true
tocOpen: true
---
Dans le monde des distributions Linux et windows, il existe de nombreuses façons de gérer les serveurs à distance. 

# SSH
Secure Shell( `SSH`) permet à deux ordinateurs d'établir une connexion chiffrée et directe au sein d'un réseau potentiellement non sécurisé sur le port standard `TCP 22`.
Il existe donc deux protocoles concurrents : `SSH-1`et `SSH-2`.
`SSH-2`, également connu sous le nom de SSH version 2, est un protocole plus avancé que SSH version 1 en termes de chiffrement, de vitesse, de stabilité et de sécurité. Par exemple, `SSH-1` il est vulnérable aux `MITM`attaques, contrairement à SSH-2.

#  Rsync
Rsync est un outil rapide et efficace pour copier des fichiers localement et à distance. Il permet de copier des fichiers localement sur une machine donnée et vers/depuis des hôtes distants.

# R-Services
Les R-Services sont une suite de services hébergés pour permettre l'accès à distance ou l'exécution de commandes entre hôtes Unix via TCP/IP.
`R-services`S'étendant sur les ports `512`, `513`, et `514`et , ils ne sont accessibles que via une suite de programmes appelée `r-commands`. Ils sont le plus souvent utilisés par les systèmes d'exploitation commerciaux tels que Solaris, HP-UX et AIX.
La suite R-commands se compose des programmes suivants :
- rcp ( `remote copy`)
- rexec ( `remote execution`)
- connexion r ( `remote login`)
- rsh ( `remote shell`)
- rstat
- temps de rupture
- rwho ( `remote who`)
- 
# RDP
Le protocole RDP (Remote Desktop Protocol `RDP`est un protocole développé par Microsoft pour l'accès à distance à un ordinateur exécutant Windows. Ce protocole permet la transmission chiffrée de commandes d'affichage et de contrôle via l'interface utilisateur graphique (GUI) sur les réseaux IP.

# WinRM

La gestion à distance Windows ( `WinRM`) est un protocole simple de gestion à distance intégré à Windows, basé sur la ligne de commande. WinRM utilise le protocole Simple Object Access Protocol ( `SOAP`) pour établir des connexions aux hôtes distants et à leurs applications. Par conséquent, WinRM doit être explicitement activé et configuré à partir de Windows 10. WinRM s'appuie sur `TCP`les ports `5985`et `5986`pour communiquer.

# WMI

Windows Management Instrumentation ( `WMI`) est l'implémentation de Microsoft et une extension du modèle commun d'information ( `CIM`), fonctionnalité essentielle de la gestion d'entreprise Web standardisée ( `WBEM`) pour la plateforme Windows.



https://www.golinuxcloud.com/openssh-authentication-methods-sshd-config/
https://book.hacktricks.wiki/en/network-services-pentesting/873-pentesting-rsync.html