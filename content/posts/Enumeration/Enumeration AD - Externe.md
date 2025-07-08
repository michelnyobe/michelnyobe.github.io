---
title: "Enumeration - SMTP"
date: 2025-05-07T10:00:00+02:00
draft: false
tags: ["Enumeration", "Active Directory"]
categories: ["Enumeration"]
summary: "SMTP"
showToc: true
tocOpen: true
---

Outils 
- Crackmapexec
- netexec
- smbclient
- rpcclient
- ldapsearch


```bash
ldapsearch -x -H ldap://10.10.11.75 -D 'name' -w 'password' -b "dc=rustykey,dc=htb" "(objectClass=user)" userPrincipalName
```