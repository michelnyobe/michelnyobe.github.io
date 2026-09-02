---
title: "Methodologie"
date: 2026-08-01
draft: true
tags: ["pentest", "Active Directory", "PowerView", "reconnaissance", "CPTS", "red team"]
categories: ["Red Team", "pentester"]
summary: ""
showToc: true
tocOpen: true
---



## Enumeration 
# Enumeration reseaux 
# enumeration AD 

kerbrute 

Utilisation de CrackMapExec avec des identifiants valides

```
kerbrute userenum -d inlanefreight.local --dc 172.16.5.5 /opt/jsmith.txt 
```

```
sudo crackmapexec smb 172.16.5.5 -u htb-student -p Academy_student_AD! --users
```
