---
title: "OSCP"
date: 2025-04-29T20:00:00+02:00
draft: true
tags: ["hackthebox", "writeup"]
categories: ["writeup"]
summary: "Difficulte : esay"
showToc: true
tocOpen: true
---

Liste des tâches et compétences à réviser, basée sur le **syllabus officiel PEN-200** (24 modules).

> ℹ️ Examen pratique de **23h45** + 24h pour le rapport. 100 points (70 requis pour passer). Set Active Directory : **40 points** (full chain obligatoire). Set autonomes : 3 × 20 points (10 initial access + 10 privesc). ⚠️ La partie AD est éliminatoire : il faut compromettre TOUT le set AD pour avoir les 40 points.

---

## 1. Introduction & Méthodologie (modules 1-3)

- [ ] Setup VM Kali, connexion VPN
- [ ] Comprendre la méthodologie OffSec ("Try Harder")
- [ ] Stratégies d'apprentissage (retrieval practice, spaced repetition, Feynman)
- [ ] Cycle de vie d'un pentest (recon → exploit → post-exploit → report)
- [ ] CIA Triad, threats, threat actors

---

## 2. Report Writing for Penetration Testers ⚠️ _Obligatoire pour passer_

- [ ] Outils de prise de notes (CherryTree, Obsidian, Notion, Joplin)
- [ ] Importance de la portabilité des notes
- [ ] Captures d'écran systématiques (flameshot, Greenshot)
- [ ] Structure d'un rapport pro : Executive Summary, Technical Findings, Recommendations
- [ ] Adapter le contenu à l'audience (technique vs management)
- [ ] Template OffSec officiel
- [ ] Annexes, références, ressources

---

## 3. Information Gathering

- [ ] **Passive recon** : OSINT, WHOIS, DNS passif, Shodan, Censys
- [ ] Web server passive recon (headers, robots.txt, sitemap, archive.org)
- [ ] **Active recon** :
    - [ ] Netcat scanning
    - [ ] **Nmap** : TCP/UDP scans, version detection, NSE scripts, timing
    - [ ] Énumération DNS (dig, host, dnsenum, dnsrecon)
    - [ ] Énumération SMB (smbclient, smbmap, enum4linux-ng, nmap NSE)
    - [ ] Énumération SMTP (VRFY, EXPN, RCPT TO)
    - [ ] Énumération SNMP (snmpwalk, snmp-check)
- [ ] Living off the Land (LOLBins, GTFOBins)

---

## 4. Vulnerability Scanning

- [ ] Théorie : authenticated vs unauthenticated, agent vs network-based
- [ ] **Nessus** : install, scan policies, plugins, credentialed scans
- [ ] **Nmap NSE** : scripts vuln, custom NSE
- [ ] Interpréter et trier les résultats (faux positifs)

---

## 5. Introduction to Web Applications

- [ ] Méthodologie OWASP, Top 10
- [ ] Énumération web : headers, cookies, source code, JS files
- [ ] **Burp Suite** (Community suffit) : Proxy, Repeater, Intruder, Decoder
- [ ] Debug source code côté client
- [ ] API testing basique
- [ ] **XSS** : reflected, stored, DOM
- [ ] Privilege escalation via XSS (vol de cookies admin)

---

## 6. Common Web Application Attacks ⚠️ _Très important_

- [ ] **Directory Traversal** : `../`, encodage, double encoding, null byte
- [ ] **LFI** : lecture de fichiers sensibles (`/etc/passwd`, log files)
- [ ] PHP wrappers : `php://filter`, `php://input`, `data://`, `expect://`
- [ ] **LFI → RCE** : log poisoning (Apache, SSH, mail), session files
- [ ] **RFI** (rare mais possible)
- [ ] **File Upload** : bypass extension, MIME, magic bytes, .htaccess
- [ ] Webshells PHP/ASPX/JSP
- [ ] **Command Injection** : opérateurs (`;`, `|`, `&&`, `` ` ``, `$()`), bypass de filtres

---

## 7. SQL Injection Attacks

- [ ] Théorie SQL, types de DB (MySQL, MSSQL, PostgreSQL, SQLite, Oracle)
- [ ] Détection manuelle (single quote, comments)
- [ ] **UNION-based** : nombre de colonnes, types, extraction de données
- [ ] **Error-based**
- [ ] **Blind** : boolean-based, time-based
- [ ] **MSSQL `xp_cmdshell`** → RCE
- [ ] Lecture/écriture de fichiers (`LOAD_FILE`, `INTO OUTFILE`)
- [ ] **SQLMap** : flags essentiels, `--os-shell`, tampers

---

## 8. Client-Side Attacks

- [ ] Reconnaissance de la cible (browser, OS, plugins)
- [ ] Client fingerprinting
- [ ] **Microsoft Office macros** (Word, Excel)
- [ ] Création de payloads malveillants
- [ ] **Windows library files** (.library-ms)
- [ ] Windows shortcuts (.lnk) pour RCE
- [ ] Phishing avec payloads custom

---

## 9. Locating Public Exploits ⚠️ _Critique pour l'examen_

- [ ] **Risques d'exploits non vérifiés** (toujours lire le code avant)
- [ ] Sources online : Exploit-DB, GitHub, Packet Storm, NVD
- [ ] **SearchSploit** (offline, syntaxe avancée)
- [ ] Google dorking pour exploits
- [ ] Workflow : recon → identification CVE → recherche exploit → adaptation
- [ ] Frameworks : Metasploit, Exploit Frameworks

---

## 10. Fixing Exploits ⚠️ _Spécifique OSCP_

- [ ] Lire et comprendre du code C / Python d'exploit
- [ ] Théorie buffer overflow (high-level)
- [ ] **Cross-compilation** (mingw pour Windows depuis Linux)
- [ ] Modifier shellcode, addresses, offsets, badchars
- [ ] Fix d'exploits web cassés (URLs, paramètres, payloads)
- [ ] Troubleshooting d'exploits qui ne marchent pas

---

## 11. Antivirus Evasion

- [ ] Composants AV : signatures, heuristiques, behavioral
- [ ] **Évasion manuelle** : refactoring, encodage, obfuscation
- [ ] **Outils** : Shellter, Veil, msfvenom encoders, donut
- [ ] Tester sur **AntiScan.me** ou en local (jamais VirusTotal)
- [ ] PowerShell obfuscation (Invoke-Obfuscation)
- [ ] AMSI bypass basique

---

## 12. Password Attacks

- [ ] **Hydra** : SSH, RDP, HTTP POST forms, services
- [ ] Cracking de wordlists, mutations (rules)
- [ ] **Hashcat** : modes courants, masks, rules (best64, OneRuleToRuleThemAll)
- [ ] **John the Ripper** : `*2john` scripts (ssh2john, zip2john, keepass2john)
- [ ] Attaque SSH private keys (passphrase cracking)
- [ ] Password manager files
- [ ] **NTLM hashes** : obtenir, cracker, **Pass-the-Hash**
- [ ] **Net-NTLMv2** : capture (Responder), crack, **NTLM relay** (ntlmrelayx)

---

## 13. Windows Privilege Escalation ⚠️ _Très important_

- [ ] Énumération manuelle : `whoami /priv`, `whoami /groups`, `systeminfo`
- [ ] Outils auto : **WinPEAS**, PowerUp, Seatbelt, Sherlock
- [ ] PowerShell history, files sensibles
- [ ] **Service binaries hijacking** (BinPath weak permissions)
- [ ] **Service DLL hijacking**
- [ ] **Unquoted service paths**
- [ ] **Scheduled tasks** abuse
- [ ] **Privilèges abusifs** : SeImpersonate (Potato attacks), SeBackup, SeRestore, SeTakeOwnership
- [ ] **Kernel exploits** (rare mais possible)
- [ ] **AlwaysInstallElevated**, registry autoruns

---

## 14. Linux Privilege Escalation ⚠️ _Très important_

- [ ] Énumération manuelle : `id`, `sudo -l`, `cat /etc/passwd`, `crontab -l`
- [ ] Outils auto : **LinPEAS**, linenum, lse
- [ ] Bash history, fichiers `.config`, credentials harvesting
- [ ] **Cron jobs insecure** (writable scripts, PATH injection, wildcards)
- [ ] **Insecure file permissions** (writable /etc/passwd, /etc/shadow)
- [ ] **SUID / SGID** binaries (GTFOBins)
- [ ] **Capabilities** Linux
- [ ] **sudo misconfigurations** (sudo -l, special permissions)
- [ ] **Kernel exploits** (DirtyCow, DirtyPipe, etc.)
- [ ] LD_PRELOAD, LD_LIBRARY_PATH abuse

---

## 15. Port Redirection & SSH Tunneling ⚠️ _Critique pour AD set_

- [ ] **Socat** : port forwarding TCP/UDP
- [ ] **SSH** :
    - [ ] Local port forwarding (`-L`)
    - [ ] Remote port forwarding (`-R`)
    - [ ] Dynamic port forwarding (`-D`) + SOCKS proxy
    - [ ] Reverse dynamic forwarding
- [ ] **Plink** (Windows)
- [ ] **netsh** (Windows port forwarding)
- [ ] **proxychains** configuration

---

## 16. Advanced Tunneling

- [ ] **HTTP tunneling** avec Chisel
- [ ] **DNS tunneling** avec dnscat2
- [ ] Bypass de DPI (Deep Packet Inspection)
- [ ] Pivoting multi-hop

---

## 17. The Metasploit Framework

- [ ] Setup, navigation (`workspace`, `db_nmap`)
- [ ] **Auxiliary modules** (scanners, brute force)
- [ ] **Exploit modules** : configuration, options
- [ ] **Payloads** : staged vs non-staged, Meterpreter
- [ ] Création d'exécutables (`msfvenom`)
- [ ] **Post-exploitation Meterpreter** : hashdump, getsystem, migrate, kiwi
- [ ] **Post modules** (gather, recon, manage)
- [ ] **Pivoting Metasploit** : autoroute, socks_proxy, portfwd
- [ ] Resource scripts (.rc) pour automatiser

> ⚠️ **Important** : Metasploit ne peut être utilisé que sur **UNE** machine sur l'examen (hors AD set). À utiliser stratégiquement.

---

## 18. Active Directory — Introduction & Enumeration ⚠️ _40 points à l'examen_

- [ ] **Énumération manuelle legacy** : `net user /domain`, `net group`, `nltest`
- [ ] **PowerShell / .NET** : ADSI, PowerView
- [ ] Énumération via **SPN** (`setspn`, GetUserSPNs)
- [ ] Object permissions (DACL/ACL)
- [ ] **Shares du domaine** (smbmap, PowerView Get-DomainShare)
- [ ] **SharpHound** (collectors : All, Default, Session, etc.)
- [ ] **BloodHound** : analyse des chemins d'attaque, custom queries

---

## 19. Attacking Active Directory Authentication ⚠️ _Critique_

- [ ] **NTLM Authentication** : fonctionnement, faiblesses
- [ ] **Kerberos Authentication** : AS-REQ, TGT, TGS, schéma complet
- [ ] Cached credentials
- [ ] **Password attacks** :
    - [ ] Password spraying (kerbrute, crackmapexec)
    - [ ] Brute force ciblé
- [ ] **AS-REP Roasting** (utilisateurs `DONT_REQ_PREAUTH`)
- [ ] **Kerberoasting** (extraction de TGS, crack hors ligne)
- [ ] **Silver tickets** (forge de TGS)
- [ ] **Golden tickets** (impersonate via krbtgt hash)
- [ ] DCSync (impersonate DC pour récupérer hashes)

---

## 20. Lateral Movement in Active Directory

- [ ] **WMI / WinRS / WinRM**
- [ ] **PsExec** (Impacket : psexec.py, smbexec.py, wmiexec.py)
- [ ] **Pass-the-Hash** (avec pth-winexe, evil-winrm, impacket)
- [ ] **Overpass-the-Hash** (Rubeus, Mimikatz)
- [ ] **DCOM** lateral movement
- [ ] **Persistence** : golden tickets, shadow copies, scheduled tasks

---

## 21. Assembling the Pieces (capstone) ⚠️ _Très représentatif de l'examen_

- [ ] Énumération réseau publique
- [ ] Exploitation web (WordPress plugins, etc.)
- [ ] Cracking SSH private keys
- [ ] Privesc Linux (sudo)
- [ ] Phishing / pivoting vers réseau interne
- [ ] Énumération interne (sessions, services, hosts)
- [ ] **Kerberoasting** chained
- [ ] **NTLM relay** sur fonctions plugin WordPress
- [ ] Compromise du DC

> 💡 Ce module simule un mini-pentest complet. À refaire **plusieurs fois** sans aide.

---

## 22. Challenge Labs (Trying Harder)

- [ ] **OSCP A, B, C** : 3 labs qui simulent l'environnement de l'examen
- [ ] **Medtech** & **Relia** : standalone scenarios
- [ ] **Skylark** : grand lab AD
- [ ] **Beyond Root** : preuves de skill avancé
- [ ] Concept de **machine "decoy"** (à ignorer)
- [ ] Pas d'ordre logique dans les IPs
- [ ] Gestion des credentials (à tester partout)
- [ ] Routeurs / NAT / pivot

---

## Compétences transverses indispensables

- [ ] **Bash scripting** : automatisation simple
- [ ] **Python** : modifier des exploits, écrire des PoC
- [ ] **PowerShell** : énumération AD, exécution sans téléchargement
- [ ] **Cross-compilation** : `i686-w64-mingw32-gcc`
- [ ] Lecture de code en C, Python, PHP
- [ ] **Reverse / bind shells** : générer, stabiliser TTY (`stty raw -echo`, `python -c 'pty.spawn'`)
- [ ] **File transfers** Windows : certutil, PowerShell IWR, SMB, base64
- [ ] **File transfers** Linux : wget, curl, python http.server, nc

---

## OSCP+ — Bonus 10 points

- [ ] Compléter **80%** des Exercises de chaque module
- [ ] Compléter **30 machines** des Challenge Labs (sur 9 labs/100+ machines)
- [ ] Soumettre dans le portal AVANT l'examen pour avoir les bonus

---

## Conseils pratiques de révision

- [ ] Compléter **TOUT le matériel PEN-200** (modules + exercises + extra mile)
- [ ] Faire les **Challenge Labs OSCP A, B, C** en priorité
- [ ] Travailler **HackTheBox** : machines TJ_Null OSCP-like list (~75 machines)
- [ ] Travailler **Proving Grounds Practice** (PG Play + PG Practice)
- [ ] Boxes recommandées HTB : Lame, Brainfuck, Cronos, Bashed, Optimum, Granny, Devel, Active, Forest, Sauna, Resolute, Cascade, Monteverde, Mantis (AD)
- [ ] **Ippsec videos** sur YouTube (TJNull list)
- [ ] Construire un **cheatsheet personnel** par catégorie
- [ ] **Méthodologie écrite** à dérouler sur chaque box (checklist)
- [ ] S'entraîner à la **rédaction de rapport** sur chaque box pratique

---

## Conseils spécifiques à l'examen

- ⏱️ **23h45 d'examen** + 24h pour le rapport
- 🎯 **70 points minimum** pour passer
- 🔒 Set AD = **40 points** (tout-ou-rien : initial → user → DA)
- 🖥️ 3 machines standalone : **20 points chacune** (10 user / 10 root)
- 🛠️ Metasploit autorisé sur **1 seule machine standalone** (pas le set AD)
- 📸 **Captures avec proof.txt visible** + ipconfig/ifconfig OBLIGATOIRES
- 📝 Rapport rigoureusement formaté (template OffSec)
- 🚫 **Pas de scripts auto** (autoEnum, autoRecon utilisables, mais pas de scripts qui exploitent)
- ☕ Faire des **pauses** régulières, dormir, manger
- 🐰 Éviter les **rabbit holes** : chronomètre par machine

---

## Modules les plus critiques pour l'examen

1. **Active Directory (3 modules)** — 40 points en jeu, à maîtriser à 100%
2. **Linux & Windows Privilege Escalation** — sur chaque machine
3. **Port Redirection & Tunneling** — indispensable pour le set AD
4. **Common Web Application Attacks** — souvent point d'entrée standalone
5. **Locating & Fixing Public Exploits** — savoir adapter un exploit cassé
6. **Password Attacks** — hashes croisés entre machines fréquents
7. **Assembling the Pieces** — le module le plus proche de l'examen

---

## Ressources externes recommandées

- **TJ_Null OSCP Prep List** (HackTheBox + PG)
- **0xdf walkthroughs** (HTB)
- **IppSec YouTube** (videos très détaillées)
- **PayloadsAllTheThings** (GitHub) — cheatsheets
- **HackTricks** (book.hacktricks.xyz) — référence ultime
- **GTFOBins** & **LOLBAS** — privesc rapide
- **Tib3rius PrivEsc courses** (Udemy) — Windows & Linux PrivEsc dédiés