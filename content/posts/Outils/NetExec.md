---
title: "NetExec"
date: 2025-05-07T15:00:00+02:00
draft: false
tags: ["Outils", "Pentester"]
categories: ["Outils"]
summary: "NetExec"
showToc: true
tocOpen: true
---

Énumérer les utilisateurs en recherchant par force brute le RID sur la cible distante

```
nxc smb 192.168.1.0/24 -u UserNAme -p 'PASSWORDHERE' --rid-brute
```


Renvoie une liste d'hôtes en direct

```
nxc smb IP/24
```

https://www.netexec.wiki/smb-protocol/enumeration/enumerate-null-sessions