---
title: "XSS"
date: 2026-03-06T10:00:00+02:00
draft: true
tags: ["pentest", "Active Directory", "PowerView", "reconnaissance", "CRTP", "red team"]
categories: ["Red Team", "pentester"]
summary: ""
showToc: true
tocOpen: true
---
Cross-site scripting

Qu'est-ce que le cross-site scripting (XSS) ?
Lorsqu'une application web vulnérable ne filtre pas correctement les données saisies par l'utilisateur, un utilisateur malveillant peut injecter du code JavaScript supplémentaire dans un champ de saisie (par exemple, un champ de commentaire ou de réponse). Ainsi, lorsqu'un autre utilisateur consulte la même page, il exécute, à son insu, ce code JavaScript malveillant.
## Type de XSS
Il existe trois principaux types de vulnérabilités XSS 
- Stored ( Persistent ) XSS
- Reflected ( Non persistent) XSS
- DOM-based XSS


### XSS Réfléchi

Les vulnérabilités Reflected XSS se produisent lorsque notre entrée atteint le serveur back-end et nous est retournée sans être filtrée ou assainie. 



### XSS basé sur le DOM


### outils 

https://github.com/s0md3v/XSStrike
https://github.com/epsylon/xsser

https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XSS%20Injection/README.md

https://github.com/payload-box/xss-payload-list


#### Défaçage

Quatre éléments HTML sont généralement utilisés pour modifier l'apparence principale d'une page web :

- Couleur d'arrière-plan document.body.style.background
- Arrière-plan document.body.background
- Titre de la page document.title
- Texte de la page DOM.innerHTML

creation d'un serveur web php -S 127.0.0.1:8080
