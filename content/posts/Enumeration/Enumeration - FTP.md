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
Port 21  
 Quelques Commande FTP
 get  , Connect , put , quit , status , verbose 
  ft $IP
Telecharger tous les fichiers disponibles 
```
wget -m --no-passive ftp://anonymous:anonymous@10.129.14.13
```

Trouver tous les scripts ftp Nmap 

 ```
find / -type f -name ftp* 2>/dev/null | grep scripts
```

