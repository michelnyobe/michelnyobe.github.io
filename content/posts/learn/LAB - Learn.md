---
title: "Lab Cybersécurité Entreprise Moderne"
date: 2025-08-20
draft: false
tags: ["cybersécurité", "Active Directory", "Azure", "SIEM", "EDR", "Red Team", "Blue Team", "Pentest"]
categories: ["labs", "infrastructures", "cybertraining"]
summary: "Mise en place d’un lab complet d’entreprise moderne avec plus de 10 machines, 2 forêts Active Directory, serveurs applicatifs, SIEM, pare-feux, messagerie, PKI, base de données, et une machine d’attaque Kali Linux pour la simulation Red Team et Blue Team."
showToc: true
tocOpen: true
cover:
  image: "/images/labs/entreprise-lab.png"
  alt: "Lab cybersécurité entreprise moderne"
  caption: "Infrastructure d’entreprise simulée pour la cybersécurité offensive et défensive"
---
## Introduction

Dans le domaine de la cybersécurité, rien ne vaut la pratique. Pour comprendre réellement les mécanismes d’attaque et de défense, il est indispensable de travailler sur un environnement réaliste qui reproduit les infrastructures d’une entreprise moderne. C’est dans cette optique que j’ai conçu un **lab complet**, intégrant aussi bien les composants techniques qu’organisationnels d’un système d’information.

Ce lab simule une PME/ETI avec **plus de dix machines**, réparties entre différents rôles stratégiques :

- **Deux forêts Active Directory** avec leurs domaines et relations d’approbation ;
- **Contrôleurs de domaine, serveurs applicatifs, bases de données et messagerie** ;
- **Infrastructure de sécurité** avec pare-feux, IDS/IPS, SIEM et supervision ;
- **PKI et gestion des certificats** pour renforcer l’authentification ;
- Et bien sûr, une **machine d’attaque Kali Linux** pour tester les scénarios Red Team.
- 
- # L’objectif de ce projet est double 
- 
1. **Offensif (Red Team)** → s’exercer aux techniques d’attaque modernes (AD exploitation, phishing, Azure, évasion EDR) ;
2. **Défensif (Blue Team)** → apprendre à détecter, analyser et répondre aux menaces grâce au SIEM et aux journaux d’audit.

Ce lab servira ainsi de terrain d’entraînement pour expérimenter plus de **50 scénarios réalistes**, couvrant tout le cycle de vie d’une cyberattaque : de l’intrusion initiale au mouvement latéral, jusqu’à l’évasion des systèmes de détection.
