---
title: "Introduction a EDR"
date: 2026-03-06T10:00:00+02:00
draft: true
tags: ["pentest", "Active Directory", "PowerView", "reconnaissance", "CRTP", "red team"]
categories: ["Red Team", "pentester"]
summary: ""
showToc: true
tocOpen: true
---
# Introduction 
Endpoint Detection and Response ( EDR ) est une solution de sécurité conçue pour surveiller, détecter et contrer les menaces avancées au niveau des terminaux. 

Voici quelques-unes des solutions EDR disponibles sur le marché :

CrowdStrike Falcon
SentinelOne ActiveEDR
Microsoft Defender pour Endpoint
OpenEDR
Symantec EDR

## Caractéristique de l'EDR
Un système EDR comporte trois caractéristiques principales , que l'on peut également appeler les trois piliers d'une solution EDR .
visibilité  , détection et la réponse 
## Comment fonctionne un EDR
### Agents
Nous pouvons intégrer plusieurs terminaux à notre solution EDR et les gérer via une console centralisée. Des agents EDR doivent être déployés sur ces terminaux. Ces agents, parfois appelés capteurs, sont les yeux et les oreilles de l' EDR .

### Console EDR
Toutes les données détaillées transmises par les agents EDR sont corrélées et analysées grâce à une logique complexe et des algorithmes d'apprentissage automatique. Les informations de renseignement sur les menaces sont comparées aux données collectées. 