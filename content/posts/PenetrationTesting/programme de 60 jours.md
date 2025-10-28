# Modules 

Penetration Testing Process
Getting Started
Network Enumeration with Nmap
Footprinting
Information Gathering - Web Edition
Vulnerability Assessment
File Transfers
Shells & Payloads
Using the Metasploit Framework
Password Attacks
Attacking Common Services
Pivoting, Tunneling, and Port Forwarding
Active Directory Enumeration & Attacks
Using Web Proxies
Attacking Web Applications with Ffuf
Login Brute Forcing
SQL Injection Fundamentals
SQLMap Essentials
Cross-Site Scripting (XSS)
File Inclusion
File Upload Attacks
Command Injections
Web Attacks
Attacking Common Applications
Linux Privilege Escalation
Windows Privilege Escalation
Documentation & Reporting
Attacking Enterprise Networks
# Programme 

## Semaine 1 — Fondations & reconnaissance

1. Jour 1 — Introduction HTB & préparation environnement (VPN, Kali/WSL, proxy Burp). Crée template de rapport.
    
2. Jour 2 — Outils reconnaissance réseau : **nmap** (scans TCP/UDP, NSE), lecture rapide options avancées. Hands-on : 3 scans différents sur une VM.
    
3. Jour 3 — Enumeration service : **nmap scripts**, **rpc**, **smb/ftp/http** ; pratique sur une machine Linux simple.
    
4. Jour 4 — Recon web basique : **curl**, **wget**, **whatweb**, **robots.txt**, **dirb/gobuster**. Lab : force directories sur un site vuln.
    
5. Jour 5 — Burp Suite : interception, repeater, intruder basics. Exercices de prise en main.
    
6. Jour 6 — OWASP Top 10 — introduction + A01 Broken Access Control / A03 Injection. Étudie les exemples. [OWASP Foundation](https://owasp.org/Top10/?utm_source=chatgpt.com)
    
7. Jour 7 — Mini-CTF récapitulatif : réalise 2 machines faciles (HTB / TryHackMe) et rédige report sommaire pour chaque accès.
    

## Semaine 2 — Web avancé & exploitation

8. Jour 8 — Injection SQL : théorie, payloads, time-based / error-based. Lab sur room OWASP/SQLi.
    
9. Jour 9 — XSS & autres injections client : stored/reflected, exploitation via Burp.
    
10. Jour 10 — Authentification / Broken Access Control avancé (IDOR, misconfig), test horizontal/vertical.
    
11. Jour 11 — Files upload / RCE via webapps (upload filtering, content-type). Pratique sur VM vulnérable.
    
12. Jour 12 — CVE / exploit public : chercher/exploiter un exploit public (Metasploit / manuel).
    
13. Jour 13 — Web chaining : combiner vulnérabilités pour obtenir shell. Exercice complet.
    
14. Jour 14 — Capture-the-flag web : 1 box HTB/THM de difficulté moyenne, rapport complet.
    

## Semaine 3 — Systèmes, services et exploitation réseau

15. Jour 15 — Exploitation SMB & services Windows (smbclient, enum4linux).
    
16. Jour 16 — Exploit local & transfer de fichiers (nc, python -c, smbclient).
    
17. Jour 17 — Buffer overflow (concepts) : stack vs heap, fuzzing basique. Exercice d’intro (simple vulnerable binary).
    
18. Jour 18 — Binary exploitation pratique (ret2libc / simple bo).
    
19. Jour 19 — Post-exploitation : maintien d’accès, pivoting (sshuttle, proxychains), tunneling.
    
20. Jour 20 — Exercice complet : obtention de shell → maintien → pivots vers autre machine.
    
21. Jour 21 — Revue mi-parcours : corriger templates de rapport, checklist d’examen HTB (sauvegarder preuves).
    

## Semaine 4 — Escalade de privilèges (Linux & Windows)

22. Jour 22 — Linux priv-esc : enumeration (linpeas, sudoers, SUID, kernel exploits). Pratique hands-on. [Delinea](https://delinea.com/blog/linux-privilege-escalation?utm_source=chatgpt.com)
    
23. Jour 23 — Linux priv-esc avancé : file capabilities, cron, PATH hijacking.
    
24. Jour 24 — Windows priv-esc : winPEAS, services malconfigurés, scheduled tasks, weak service binary paths. [sushant747.gitbooks.io+1](https://sushant747.gitbooks.io/total-oscp-guide/privilege_escalation_windows.html?utm_source=chatgpt.com)
    
25. Jour 25 — Powershell & outils Windows pour reconnaissance (Get-GPO, PowerView). Exercice AD local simple.
    
26. Jour 26 — Active Directory — notions (Kerberos, LDAP, AS-REP Roasting, Kerberoasting). Lecture + démo.
    
27. Jour 27 — AD pratique : BloodHound basics (collect && analyse) et exploitation AD common chains.
    
28. Jour 28 — Scénario complet AD : initial access web/service → lateral movement → DA ou compromise d’un compte à haute valeur.
    

## Semaine 5 — Affûtage & reporting

29. Jour 29 — Remédiations & mitigation : pour chaque vuln du syllabus OWASP, note les correctifs/pratiques sécurisées. [OWASP Foundation](https://owasp.org/www-project-top-ten/?utm_source=chatgpt.com)
    
30. Jour 30 — Redaction de rapport : structure attendue pour HTB (méthodologie, preuves, PoC, remediation). Réécris 2 reports précédents. [help.hackthebox.com](https://help.hackthebox.com/en/articles/9561479-academy-certifications?utm_source=chatgpt.com)
    
31. Jour 31 — Simulation d’examen : start → tenter de résoudre 1 machine « comme en examen » (conserver logs) — t’imposer un temps limité.
    
32. Jour 32 — Revue techniques faibles : analyse erreurs durant la simulation, combler lacunes (ex : bruteforce, wordlists, encodages).
    
33. Jour 33 — Répétition outils essentiels : nmap avancé, Burp workflows, linpeas/winPEAS, BloodHound, proxychains.
    
34. Jour 34 — Examen blanc final : réalise 2 machines (1 web + 1 AD/Windows) en conditions « examen ». Rédige rapport synthétique.
    
35. Jour 35 — Préparation administrative examen : checklist final (vouchers, accès VPN, format du rapport, captures, vidéos si demandé). Relecture complète du report final, mise en forme et backup.
    

---

# Conseils pratiques & outils (rapide)

- **Outils** : nmap, masscan (si autorisé), gobuster/dirb, Burp Suite, wfuzz, sqlmap (avec parcimonie), netcat, socat, ssh, meterpreter (connaissance), linpeas/winPEAS, PowerView, BloodHound.
    
- **Résultats & preuve** : capture d’écran + commandes exécutées + sortie brute (redirection stdout). HTB demande souvent un **rapport formel** (méthodologie, PoC, remediation). [help.hackthebox.com](https://help.hackthebox.com/en/articles/9561479-academy-certifications?utm_source=chatgpt.com)
    
- **Sources d’entraînement** : HTB Academy & labs, TryHackMe rooms (OWASP Top10 room recommandé), writeups (pour apprendre après tentative)