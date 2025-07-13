---
title: "Gobuster"
date: 2025-05-07T15:00:00+02:00
draft: false
tags: ["Outils", "Pentester"]
categories: ["Outils"]
summary: "Gobuster"
showToc: true
tocOpen: true
---
Gobuster est un outil polyvalent couramment utilisé pour le forçage de répertoires et de fichiers, mais il excelle également dans la découverte d'hôtes virtuels. Il envoie systématiquement des requêtes HTTP avec différents `Host`en-têtes à une adresse IP cible, puis analyse les réponses pour identifier les hôtes virtuels valides.

Il y a quelques éléments que vous devez préparer pour forcer `Host`les en-têtes :

1. `Target Identification`Tout d'abord, identifiez l'adresse IP du serveur web cible. Cela peut être réalisé par des recherches DNS ou d'autres techniques de reconnaissance.
2. `Wordlist Preparation`Préparez une liste de mots contenant des noms d'hôtes virtuels potentiels. Vous pouvez utiliser une liste de mots précompilée, telle que SecLists, ou en créer une personnalisée en fonction du secteur d'activité de votre cible, des conventions de nommage ou d'autres informations pertinentes.

```bash
gobuster dir -u http://10.10.11.72  -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 100
```


```bash
gobuster vhost -u http://<target_IP_address> -w <wordlist_file> --append-domain
```