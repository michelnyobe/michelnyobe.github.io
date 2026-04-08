---
title: "SQL injection"
date: 2026-03-06T10:00:00+02:00
draft: true
tags: ["pentest", "Active Directory", "PowerView", "reconnaissance", "CRTP", "red team"]
categories: ["Red Team", "pentester"]
summary: ""
showToc: true
tocOpen: true
---
# In-band SQL Injection
Elle consiste à utiliser le même canal de communication pour l'injection et la récupération des données.
## Injection SQL basée sur les erreurs
L’attaquant manipule la requête SQL pour générer des messages d’erreur provenant de la base de données. 
## Injection SQL basée sur l'union
L'attaquant utilise l'opérateur SQL UNION pour combiner les résultats de deux ou plusieurs instructions SELECT en un seul résultat, récupérant ainsi des données provenant d'autres tables.

# (Blind) SQL Injection
 L'attaquant envoie plutôt des requêtes et observe le comportement et les temps de réponse de l'application pour déduire des informations sur la base de données.
## Injection SQL aveugle booléenne
L’attaquant envoie une requête SQL à la base de données, forçant l’application à renvoyer un résultat différent selon que la condition est vraie ou fausse. En analysant la réponse de l’application, l’attaquant peut déduire si la requête était vraie ou fausse.
## Injection SQL aveugle temporelle 
L’attaquant envoie une requête SQL à la base de données, ce qui retarde la réponse d’une durée déterminée si la condition est vraie

## Injection SQL de second ordre

L'injection SQL de second ordre , également appelée injection SQL stockée , exploite les vulnérabilités liées à l'enregistrement et à l'utilisation ultérieure des données saisies par l'utilisateur dans une autre partie de l'application, éventuellement après un traitement initial. 