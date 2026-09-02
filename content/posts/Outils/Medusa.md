---
title: "Medusa"
date: 2026-08-13
draft: false
tags: ["CPTS", "Pentester"]
categories: ["CPTS"]
summary: "Atttaque par force brute"
showToc: true
tocOpen: true
---

Medusa, un outil de premier plan dans l'arsenal de la cybersécurité, est conçu pour être un outil de brute-force de connexion rapide, massivement parallèle et modulaire. Son objectif principal est de prendre en charge un large éventail de services qui permettent l'authentification à distance, permettant aux testeurs d'intrusion (penetration testers) et aux professionnels de la sécurité d'évaluer la résilience des systèmes de connexion contre les attaques par force brute.

## Syntaxe des commandes

```bash
medusa [target_options] [credential_options] -M module [module_options]
```

Cibler un serveur SSH

```bash
medusa -h IP -U usernames.txt -P passwords.txt -M ssh 
```

Cibler plusieurs serveurs Web avec une authentification HTTP de base

```shell
medusa -H web_servers.txt -U usernames.txt -P passwords.txt -M http -m GET
```

Tester les mots de passe vides ou par défaut

```shell
medusa -h 10.0.0.5 -U usernames.txt -e ns -M service_name
```

## Services Web

```shell
medusa -h <IP> -n <PORT> -u sshuser -P 2023-200_most_used_passwords.txt -M ssh -t 3
```

Cibler le serveur FTP

```shell
medusa -h 127.0.0.1 -u ftpuser -P 2020-200_most_used_passwords.txt -M ftp -t 5
```

