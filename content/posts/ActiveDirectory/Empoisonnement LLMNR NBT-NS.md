---
title: "Empoisonnement LLMNR NBT-NS"
date: 2026-07-30
draft: false
tags: ["pentest", "Active Directory", "Responder", "CPTS"]
categories: ["CPTS", "pentester","AD"]
summary: "LLMNR et NBT-NS n'authentifient pas leurs réponses : cette faille de conception permet à un attaquant du réseau local d'usurper des hôtes légitimes, de capturer des hachages NTLMv2 avec Responder ou Inveigh, puis de les craquer hors ligne avec Hashcat."
showToc: true
tocOpen: true
---

## Introduction à LLMNR & NBT-NS

La Résolution de noms multidiffusion de lien local (LLMNR) et le Service de noms NetBIOS (NBT-NS) sont des composants de Microsoft Windows qui servent de méthodes alternatives d'identification d'hôte pouvant être utilisées en cas d'échec du DNS.

Cependant, LLMNR et NBT-NS présentent tous deux une faille de conception critique : ils n’authentifient pas les réponses . De ce fait, n’importe quel périphérique du réseau local peut répondre aux requêtes de résolution de noms. Cela permet aux attaquants d’usurper l’identité de systèmes légitimes et d’intercepter le trafic réseau.

En exploitant des outils , un attaquant peut répondre à ces requêtes de diffusion et inciter les machines victimes à initier des tentatives d'authentification. Ce processus lui permet de capturer des informations sensibles, notamment les hachages NTLMv2 , les noms d'utilisateur et les détails de domaine . Ces identifiants peuvent ensuite être déchiffrés hors ligne ou relayés à d'autres services pour obtenir un accès non autorisé.

## Fonctionnement de l'attaque 

Lorsqu'un système Windows ne parvient pas à résoudre un nom d'hôte via DNS, il utilise automatiquement LLMNR ou NBT-NS . Ces protocoles diffusent les requêtes sur le réseau local, acceptant toute réponse reçue sans vérification.

Comme aucune authentification n'est requise, n'importe quel appareil du sous-réseau peut répondre à ces requêtes. Un attaquant surveillant le réseau peut intercepter ces requêtes et y répondre par une réponse malveillante, redirigeant ainsi le trafic de la victime.

Lors d'une attaque par empoisonnement LLMNR , l'attaquant intercepte les requêtes de résolution de noms diffusées et y répond en fournissant sa propre adresse IP. La victime est alors amenée à établir une connexion avec le système de l'attaquant. Durant ce processus, elle transmet automatiquement des données d'authentification, telles que des hachages NTLMv2 .


![llmnr](/images/LLMNR.svg)

Plusieurs outils peuvent être utilisés pour tenter l'empoisonnement LLMNR & NBT-NS  :

Responder : Responder est un outil spécialement conçu pour empoisonner LLMNR, NBT-NS et MDNS, avec de nombreuses fonctions différentes.

Inveigh :  Inveigh est une plateforme MITM multiplateforme qui peut être utilisée pour des attaques d'usurpation et d'empoisonnement.

Metasploit : Metasploit dispose de plusieurs scanners et modules d'usurpation intégrés conçus pour gérer les attaques d'empoisonnement.

## Responder 

Responder est un outil relativement simple, mais il est extrêmement puissant et possède de nombreuses fonctions différentes.

Avec la configuration montrée ci-dessus, Responder écoutera et répondra à toutes les requêtes qu'il verra sur le réseau. Si vous réussissez et parvenez à capturer un hachage, Responder l'affichera à l'écran et l'écrira dans un fichier de log par hôte situé dans le répertoire /usr/share/responder/logs. Les hachages sont enregistrés au format (NOM_MODULE)-(TYPE_HACHAGE)-(IP_CLIENT).txt, et un hachage est affiché dans la console et stocké dans son fichier de log associé, sauf si le mode -v est activé. Par exemple, un fichier de log peut ressembler à SMB-NTLMv2-SSP-172.16.5.25. Les hachages sont également stockés dans une base de données SQLite qui peut être configurée dans le fichier de configuration Responder.conf, généralement situé dans /usr/share/responder, à moins que nous ne clonions le dépôt Responder directement depuis GitHub.
Nous devons exécuter l'outil avec les privilèges sudo ou en tant que root 

Démarrer Responder avec les paramètres par défaut

```
sudo responder -I <Interdace d'ecoute>

```


![responder](/images/responder.png)


Une fois que nous en avons assez, nous devons mettre ces hachages dans un format utilisable pour nous dès maintenant. Les hachages NetNTLMv2 sont très utiles une fois craqués, mais ne peuvent pas être utilisés pour des techniques telles que le pass-the-hash

```
hashcat -m 5600 hash.txt <liste de mot de passes >
```


![hashcat](/images/hashcat.png)

## Inveigh

Si nous nous retrouvons avec un hôte Windows comme machine d'attaque, si notre client nous fournit une machine Windows pour effectuer nos tests, ou si nous atterrissons sur un hôte Windows en tant qu'administrateur local via une autre méthode d'attaque et que nous souhaitons étendre notre accès, l'outil Inveigh fonctionne de manière similaire à Responder, mais il est écrit en PowerShell et en C#.

```
Import-Module .\Inveigh.ps1
(Get-Command Invoke-Inveigh).Parameters
```

Lançons Inveigh avec l'usurpation (spoofing) LLMNR et NBNS,

```
Invoke-Inveigh Y -NBNS Y -ConsoleOutput Y -FileOutput Y
```


![inveigh](/images/inveigh.png)

 
