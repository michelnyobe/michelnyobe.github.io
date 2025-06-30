---
title: "Enumeration - FTP"
date: 2025-05-07T10:00:00+02:00
draft: false
tags: ["Enumeration", "FTP"]
categories: ["Enumeration"]
summary: "FTP"
showToc: true
tocOpen: true
---

Le protocole FTP `File Transfer Protocol`( `FTP`) est l'un des plus anciens protocoles d'Internet. Il s'exécute au sein de la couche application de la pile de protocoles TCP/IP.
Port 21  
 Quelques Commande FTP
 
 get  , Connect , put , quit , status , verbose 
 
```
  ft $IP
```
Telecharger tous les fichiers disponibles 

```
wget -m --no-passive ftp://anonymous:anonymous@10.129.14.13
```

Trouver tous les scripts ftp Nmap 

 ```
find / -type f -name ftp* 2>/dev/null | grep scripts
```


## TFTP

`Trivial File Transfer Protocol`( `TFTP`) est plus simple que FTP et effectue des transferts de fichiers entre les processus client et serveur.

https://web.archive.org/web/20230326204635/https://www.smartfile.com/blog/the-ultimate-ftp-commands-list/