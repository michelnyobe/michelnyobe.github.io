---
title: "CPTS-HTB"
date: 2025-04-29T20:00:00+02:00
draft: true
tags: ["hackthebox", "writeup"]
categories: ["writeup"]
summary: "Difficulte : esay"
showToc: true
tocOpen: true
---

Liste des tâches et compétences à réviser, organisée par domaine (basée sur les 28 modules du Penetration Tester path).

---

## 1. Méthodologie & Fondamentaux

- [ ] Maîtriser le processus de pentest (pre-engagement, info gathering, reporting)
- [ ] Vulnerability Assessment : utiliser Nessus / OpenVAS et interpréter les résultats
- [ x] File Transfers (Linux/Windows) : SMB, HTTP, FTP, certutil, PowerShell, base64
- [ ] Shells & Payloads : bind/reverse shells, payloads msfvenom, stabilisation TTY
- [x ] Metasploit Framework : modules, payloads, post-exploitation, handlers

---

## 2. Énumération & Reconnaissance

- [ x] Network Enumeration avec Nmap (scripts NSE, scan furtif, fingerprinting)
- [x ] Footprinting des services : FTP, SMB, NFS, DNS, SMTP, IMAP/POP3, SNMP, MySQL, MSSQL, Oracle, IPMI, SSH, RDP, WinRM
- [ x] Information Gathering Web Edition : WHOIS, DNS, sous-domaines, virtual hosts, crawling, fingerprinting

---

## 3. Web Application Pentesting

- [ ] Using Web Proxies (Burp Suite + OWASP ZAP en profondeur)
- [ ] Attacking Web Applications avec ffuf (fuzzing dirs, paramètres, vhosts, sub-domains)
- [ ] Login Brute Forcing (Hydra, Medusa, custom wordlists)
- [ ] SQL Injection : union-based, blind, error-based, second-order
- [ ] SQLMap Essentials : flags avancés, tampers, contournement WAF
- [ ] Cross-Site Scripting (XSS) : reflected, stored, DOM, exploitation
- [ ] File Inclusion (LFI/RFI) : log poisoning, wrappers PHP, RCE
- [ ] File Upload Attacks : bypass de filtres, polyglots, double extensions
- [ ] Command Injection : bypass de blacklists, encodage, chaînage
- [ ] Web Attacks : HTTP verb tampering, IDOR, XXE
- [ ] Attacking Common Applications : WordPress, Joomla, Drupal, Tomcat, Jenkins, Splunk, GitLab, etc.

---

## 4. Password Attacks

- [ x] Cracking offline : Hashcat, John (mode, masks, rules)
- [ x] Attaques Windows / Linux / Active Directory credentials
- [ ] Pass-the-Hash, Pass-the-Ticket, OverPass-the-Hash
- [ ] Credential hunting (fichiers, mémoire, registre, GPP)

---

## 5. Privilege Escalation

- [ ] **Linux PrivEsc** : SUID/SGID, capabilities, cron, sudo, kernel exploits, PATH hijacking
- [ ] **Windows PrivEsc** : tokens, services mal configurés, UAC bypass, registry, AlwaysInstallElevated, unquoted paths
- [ ] Outils : LinPEAS, WinPEAS, PowerUp, BloodHound

---

## 6. Active Directory ⚠️ _CRITIQUE pour l'examen_

- [ ] AD Enumeration : PowerView, BloodHound, ldapsearch, rpcclient, enum4linux-ng
- [ ] Kerberoasting & AS-REP Roasting
- [ ] DCSync, DCShadow
- [ ] Abus d'ACL / ACE (GenericAll, WriteDACL, etc.)
- [ ] Délégations Kerberos (unconstrained, constrained, RBCD)
- [ ] Golden / Silver tickets
- [ ] Attaques inter-domaines et inter-forêts (SID History, trust)
- [ ] Coercition : PetitPotam, PrinterBug, NTLM relay
- [ ] ADCS : ESC1 à ESC8
- [ ] LAPS, gMSA

---

## 7. Attaques sur services courants

- [ ] Exploitation des services exposés (SMB, RDP, WinRM, MSSQL, etc.)
- [ ] Abus de relations de confiance

---

## 8. Pivoting, Tunneling & Port Forwarding

- [ ] SSH tunneling (local, remote, dynamic)
- [ ] Chisel, Ligolo-ng, sshuttle
- [ ] Proxychains, socks proxies
- [ ] Port forwarding Windows (netsh, plink)
- [ ] Pivoting via Metasploit (autoroute, socks_proxy)

---

## 9. Attacking Enterprise Networks 🎯 _Très proche de l'examen_

- [ ] Chaîner web → foothold → AD → DA
- [ ] Méthodologie complète sur réseau réel

---

## 10. Reporting ⚠️ _Déterminant pour la réussite_

- [ ] Lire et appliquer le template HTB à la lettre
- [ ] Executive summary + technical findings
- [ ] Calcul de risque (CVSS), recommandations
- [ ] Captures d'écran, preuves, étapes reproductibles
- [ ] Maîtriser le module "Documentation & Reporting"

---

## Conseils pratiques de révision

- [ ] Refaire les **Skills Assessments** des modules clés sans aide
- [ ] Faire les tracks recommandées : RastaLabs, Dante, Zephyr
- [ ] Pratiquer les machines AD HTB : Forest, Active, Sauna, Resolute, Monteverde, Sizzle, Mantis
- [ ] Construire un **cheatsheet personnel** par catégorie (énumération, exploitation, AD, pivoting, privesc)
- [ ] S'entraîner à la rédaction de rapport sur des machines retired

---

## Modules les plus critiques

Selon les retours d'examen, à maîtriser absolument :

1. **Active Directory Enumeration & Attacks**
2. **Attacking Common Applications**
3. **Attacking Enterprise Networks**