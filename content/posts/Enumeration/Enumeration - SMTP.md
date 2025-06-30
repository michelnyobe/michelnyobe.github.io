---
title: "Enumeration - SMTP"
date: 2025-05-07T10:00:00+02:00
draft: false
tags: ["Enumeration", "SMTP"]
categories: ["Enumeration"]
summary: "SMTP"
showToc: true
tocOpen: true
---
Le protocole `Simple Mail Transfer Protocol`( `SMTP`) permet d'envoyer des e-mails sur un réseau IP. Il peut être utilisé entre un client de messagerie et un serveur de courrier sortant, ou entre deux serveurs SMTP
Par défaut, les serveurs SMTP acceptent les demandes de connexion sur le port `25`
Pour interagir avec le serveur SMTP, nous pouvons utiliser l' `telnet`outil pour initialiser une connexion TCP. L'initialisation de la session s'effectue avec la commande mentionnée ci-dessus, `HELO`ou `EHLO`.\

```bash
telnet IP_ADRESS 25
```

```bash
sudo nmap 10.129.14.128 -p25 --script smtp-open-relay -v
```

```
smtp-user-enum
```


```bash
smtp-user-enum -M VRFY -U /usr/share/seclists/Discovery/SNMP/snmp.txt -t 10.129.1.62 
```




https://serversmtp.com/smtp-error/