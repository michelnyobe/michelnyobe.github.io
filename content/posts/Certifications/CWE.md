---
title: "CRTA"
date: 2026-03-06T10:00:00+02:00
draft: true
tags: ["pentest", "Active Directory", "PowerView", "reconnaissance", "CRTP", "red team"]
categories: ["Red Team", "pentester"]
summary: ""
showToc: true
tocOpen: true
---


Liste des tâches et compétences à réviser, organisée par domaine (basée sur les 20 modules du **Web Penetration Tester** path, ex-Bug Bounty Hunter).

> ℹ️ La CWES (anciennement CBBH) est la certification web intermédiaire de HTB. Examen pratique de **7 jours** sur plusieurs applications réelles + rapport.

---

## 1. Web Requests

- [ ] Comprendre HTTP/HTTPS, méthodes (GET, POST, PUT, DELETE, PATCH, OPTIONS, HEAD)
- [ ] Headers HTTP (request & response)
- [ ] Status codes (1xx-5xx) et leur signification
- [ ] Cookies, sessions, structure d'une requête
- [ ] cURL : maîtrise des flags (-X, -H, -d, -b, -c, --data-binary, etc.)

---

## 2. Introduction to Web Applications

- [ ] Front-end vs back-end, architecture client/serveur
- [ ] Languages côté serveur (PHP, Python, Node.js, Java, .NET)
- [ ] Frameworks courants (Laravel, Django, Flask, Express, Spring)
- [ ] Bases de données (MySQL, PostgreSQL, MongoDB, MSSQL)
- [ ] CMS courants (WordPress, Joomla, Drupal)

---

## 3. Using Web Proxies

- [ ] **Burp Suite** : Proxy, Repeater, Intruder, Decoder, Comparer
- [ ] Burp Scanner, extensions (BApp Store)
- [ ] Configuration du proxy + certificat CA
- [ ] OWASP ZAP en alternative
- [ ] Match & Replace, scope, target tree

---

## 4. Information Gathering – Web Edition

- [ ] WHOIS, DNS enumeration (dig, host, nslookup)
- [ ] Sous-domaines : Subfinder, Amass, Sublist3r, crt.sh
- [ ] Virtual hosts (vhost) discovery
- [ ] Web crawling (gospider, hakrawler)
- [ ] Fingerprinting : Wappalyzer, WhatWeb, BuiltWith
- [ ] Recherche dans archives (Wayback Machine, archive.org)
- [ ] Google dorking

---

## 5. Web Fuzzing

- [ ] **ffuf** : fuzz directories, fichiers, paramètres, vhosts, sub-domains
- [ ] gobuster, dirb, dirbuster, feroxbuster
- [ ] Wordlists : SecLists (raft, common, big.txt)
- [ ] Filtrage des réponses (status, size, words, regex)
- [ ] Recursive fuzzing
- [ ] Extension fuzzing (.php, .bak, .old, .zip, .git)

---

## 6. JavaScript Deobfuscation

- [ ] Identifier du JavaScript obfusqué
- [ ] Beautifier / unminifier
- [ ] Outils : de4js, JSNice, Chrome DevTools (Sources, Debugger)
- [ ] Analyser des fichiers JS pour trouver endpoints, secrets, API keys
- [ ] LinkFinder, JSMiner

---

## 7. Cross-Site Scripting (XSS)

- [ ] **Reflected XSS**
- [ ] **Stored XSS**
- [ ] **DOM-based XSS**
- [ ] Détection manuelle et automatique (XSStrike, dalfox)
- [ ] Payloads de bypass de filtres (encodage, tags alternatifs, events)
- [ ] Exploitation : vol de cookies, keylogger, phishing
- [ ] Blind XSS (XSS Hunter)
- [ ] CSP : compréhension et bypass basique

---

## 8. SQL Injection Fundamentals

- [ ] Détection (single quote, double quote, comments)
- [ ] **UNION-based SQLi** (déterminer le nombre de colonnes, types)
- [ ] **Error-based SQLi**
- [ ] **Blind SQLi** : boolean-based, time-based
- [ ] Lecture de fichiers (`LOAD_FILE`, `INTO OUTFILE`)
- [ ] RCE via SQLi (selon le DBMS)
- [ ] Bypass de filtres (commentaires, casse, encodage)

---

## 9. SQLMap Essentials

- [ ] Flags de base (`-u`, `--data`, `--cookie`, `--headers`)
- [ ] DBMS detection, énumération (`--dbs`, `--tables`, `--columns`, `--dump`)
- [ ] Tampers pour bypass de WAF
- [ ] `--os-shell`, `--sql-shell`, `--file-read`, `--file-write`
- [ ] Niveau et risque (`--level`, `--risk`)
- [ ] Utiliser un fichier requête (`-r request.txt`)

---

## 10. Command Injections

- [ ] Identifier les points d'injection (paramètres système)
- [ ] Opérateurs (`;`, `|`, `&`, `&&`, `||`, `` ` ``, `$()`)
- [ ] Bypass de blacklists (espaces, caractères, encodage)
- [ ] Blind command injection (time-based, OOB via DNS/HTTP)
- [ ] Exfiltration : Burp Collaborator, requestbin, interactsh

---

## 11. File Upload Attacks

- [ ] Bypass d'extensions (double extension, null byte, casse)
- [ ] Bypass MIME type (Content-Type spoofing)
- [ ] Bypass magic bytes (polyglottes JPG/PHP)
- [ ] .htaccess upload, web.config upload
- [ ] Upload vers webshell (PHP, ASPX, JSP)
- [ ] Upload XSS via SVG / HTML
- [ ] LFI + upload chaining

---

## 12. Server-side Attacks

- [ ] **SSRF (Server-Side Request Forgery)** : interne, cloud metadata (AWS/GCP/Azure)
- [ ] **SSTI (Server-Side Template Injection)** : Jinja2, Twig, Freemarker
- [ ] **XXE (XML External Entity)** : in-band, blind, exfiltration de fichiers
- [ ] Bypass de filtres SSRF (DNS rebinding, redirections, IPv6, encodage)
- [ ] Outils : tplmap, SSRFmap

---

## 13. Login Brute Forcing

- [ ] **Hydra** (HTTP forms, basic auth, services)
- [ ] **Medusa**, **Patator**
- [ ] Burp Intruder (Sniper, Battering Ram, Pitchfork, Cluster Bomb)
- [ ] Gestion des CSRF tokens dans le brute force
- [ ] Username enumeration (timing attack, error messages)
- [ ] Wordlists ciblées (rockyou, custom)

---

## 14. Broken Authentication

- [ ] Default credentials, credentials guessing
- [ ] Session management flaws (predictable IDs, no expiration)
- [ ] Cookie security (HttpOnly, Secure, SameSite)
- [ ] **JWT** : algorithm none, weak secret, kid injection
- [ ] OAuth flaws (basique pour CWES)
- [ ] Password reset poisoning
- [ ] 2FA bypass simple

---

## 15. Web Attacks

- [ ] **HTTP Verb Tampering**
- [ ] **IDOR (Insecure Direct Object Reference)**
- [ ] **XXE** (rappel)
- [ ] CSRF (Cross-Site Request Forgery) + bypass de tokens
- [ ] Open Redirect
- [ ] CORS misconfigurations
- [ ] Host header injection

---

## 16. File Inclusion

- [ ] **LFI (Local File Inclusion)** : path traversal, lecture de fichiers sensibles
- [ ] **RFI (Remote File Inclusion)**
- [ ] PHP wrappers : `php://filter`, `php://input`, `data://`, `expect://`, `phar://`
- [ ] **Log poisoning** (Apache, SSH, mail)
- [ ] LFI → RCE via wrappers ou session files
- [ ] Bypass de filtres (null byte, encodage, double encodage)

---

## 17. API Attacks ⚠️ _Module récent et important_

- [ ] REST API enumeration et fingerprinting
- [ ] Documentation : Swagger/OpenAPI, Postman collections
- [ ] **OWASP API Top 10** (BOLA, BFLA, mass assignment, etc.)
- [ ] Authentification API : API keys, JWT, OAuth tokens
- [ ] Rate limiting bypass
- [ ] Excessive data exposure
- [ ] Outils : Postman, Insomnia, Burp

---

## 18. Attacking GraphQL ⚠️ _Module récent_

- [ ] Comprendre le schéma GraphQL (queries, mutations, subscriptions)
- [ ] Introspection queries (récupérer le schéma)
- [ ] Outils : GraphQL Voyager, InQL, GraphQLmap
- [ ] Authorization bypass (BOLA en GraphQL)
- [ ] Injection (SQLi, NoSQLi via GraphQL)
- [ ] Batching attacks, query depth/complexity DoS
- [ ] CSRF sur GraphQL

---

## 19. Attacking Common Applications ⚠️ _Critique_

- [ ] **WordPress** : enum users/plugins, brute force xmlrpc, plugins vulnérables
- [ ] **Joomla**, **Drupal**
- [ ] **Tomcat** : default creds, manager upload WAR
- [ ] **Jenkins** : Groovy console, jobs RCE
- [ ] **Splunk**, **GitLab**
- [ ] Outils : wpscan, joomscan, droopescan, CMSeek

---

## 20. Bug Bounty Hunting Process ⚠️ _Déterminant pour le rapport_

- [ ] Lire et interpréter une letter of engagement / scope
- [ ] Méthodologie de chasse (recon → mapping → exploit → impact)
- [ ] Rédaction d'un rapport commercial (executive summary, technical findings)
- [ ] CVSS scoring
- [ ] Captures d'écran, étapes reproductibles, PoC
- [ ] Recommandations de remédiation

---

## Compétences transverses

- [ ] Maîtriser **Burp Suite** à fond (Repeater, Intruder, extensions)
- [ ] Lire / écrire du **Python** simple pour automatiser des exploits
- [ ] Comprendre HTTP en profondeur
- [ ] Savoir **chaîner** plusieurs vulnérabilités pour démontrer un impact

---

## Conseils pratiques de révision

- [ ] Refaire **tous les Skills Assessments** sans aide
- [ ] Compléter les **PortSwigger Web Security Academy labs** (Apprentice + Practitioner)
- [ ] Pratiquer sur des boxes web HTB : Trick, Encoding, Pikaboo, Validation, etc.
- [ ] Construire un **cheatsheet personnel** par type de vuln
- [ ] **Utiliser les cheat sheets fournis par HTB** pour l'examen (autorisés)
- [ ] S'entraîner à la **rédaction de rapport** sur des labs anciens
- [ ] Préparer une **checklist méthodologique** à dérouler pendant l'exam (évite les rabbit holes)

---

## Modules les plus critiques

D'après les retours d'examen :

1. **Attacking Common Applications** — souvent un point d'entrée
2. **API Attacks** + **Attacking GraphQL** — modules récents très présents
3. **Server-side Attacks** (SSRF, SSTI, XXE)
4. **File Inclusion** + **File Upload Attacks** (chaînage fréquent)
5. **SQL Injection** (fundamentals + SQLMap)
6. **Bug Bounty Hunting Process** (pour le rapport, qui peut faire échouer)

---

## Conseils spécifiques à l'examen

- ⏱️ **7 jours** : ne pas se précipiter, prendre des pauses
- 🎯 Plusieurs applications à compromettre — chaîner les vulns
- 📋 **Suivre une méthodologie stricte**, ne pas tomber dans les rabbit holes
- 📝 Le **rapport est obligatoire** et évalué — soigner la qualité
- 🛠️ Utiliser le Pwnbox ou sa propre VM via VPN
- 💡 Si un payload ne marche pas : vérifier la syntaxe, encodage, contexte avant de partir sur une autre piste