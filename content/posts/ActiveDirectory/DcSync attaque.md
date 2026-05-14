---
title: "Dcsync Attaque"
date: 2026-03-06T10:00:00+02:00
draft: true
tags: ["pentest", "Active Directory", "PowerView", "reconnaissance", "CRTP", "red team"]
categories: ["Red Team", "pentester"]
summary: ""
showToc: true
tocOpen: true
---
### Qu'est-ce que DCSync ?

DCSync est une technique d'attaque post-exploitation qui permet à un attaquant de **récupérer les hashes de mots de passe** (NTLM, Kerberos keys, historiques) de **n'importe quel compte** d'un domaine Active Directory — y compris le compte ultime : **krbtgt** — sans jamais exécuter de code sur un contrôleur de domaine.

L'attaque a été popularisée par Benjamin Delpy (auteur de Mimikatz) et Vincent Le Toux en 2015. Son originalité : elle ne nécessite **aucun accès physique ou RDP** sur le DC. L'attaquant se fait passer pour un contrôleur de domaine légitime et demande une réplication.