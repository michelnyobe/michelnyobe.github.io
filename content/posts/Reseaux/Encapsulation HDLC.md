---
title: "Encapsulation HDLC"
date: 2026-09-02
draft: false
tags: ["Reseaux", "CISCO", "HDLC", "WAN", "Encapsulation"]
categories: ["Reseaux"]
summary: "Introduction à l'encapsulation HDLC (High-Level Data Link Control), protocole de liaison de données utilisé par défaut sur les interfaces série Cisco. Fonctionnement, configuration et comparaison avec PPP."
showToc: true
tocOpen: true
---

## Introduction 

![Encapsulation HDLC](/images/20260902165216.png)

Cisco définit HDLC (High-level Data Link Control) comme un groupe de protocoles de liaison de données (couche 2) utilisés pour transmettre des paquets de données synchrones entre des nœuds point à point. 

## Origine et standardisation 

HDLC est un protocole développé par l'Organisation internationale de normalisation (ISO). La norme actuelle du protocole HDLC est **ISO 13239**. 
Il dérive du protocole SDLC (Synchronous Data Link Control) créé par IBM dans les années 1970, dont il étend les capacités pour en faire un standard ouvert et interopérable. 

## Modes de fonctionnement

Le protocole HDLC offre à la fois un service **orienté connexion** et **sans connexion**, ce qui lui permet de s'adapter à différents contextes réseau. 
Il supporte trois modes de transfert : 

- **NRM** (Normal Response Mode) — architecture maître/esclave 
- **ARM** (Asynchronous Response Mode) — l'esclave peut initier des transmissions 
- **ABM** (Asynchronous Balanced Mode) — les deux nœuds sont égaux, le plus utilisé 

## Caractéristiques techniques

HDLC est un protocole **orienté bits** qui permet de transmettre des données de manière fiable et efficace. 
Il utilise des **flags de délimitation** (01111110) pour marquer le début et la fin des trames, ainsi qu'un mécanisme de **bit stuffing** pour éviter toute confusion avec les données.

## Cisco HDLC (cHDLC) 

Cependant, les entreprises privilégient PPP à Cisco HDLC pour de nombreuses raisons, notamment parce que ce dernier est **propriétaire** et impose des limitations lors de l'utilisation avec des équipements non Cisco. Cisco a développé une extension du protocole HDLC afin de remédier au problème posé par l'incapacité de ce protocole natif à prendre en charge **plusieurs protocoles simultanément**. 
Bien que Cisco HDLC (également appelé **cHDLC**) soit une norme propriétaire, Cisco a permis à de nombreux fournisseurs d'équipement de l'implémenter, ce qui lui assure une certaine adoption dans des environnements mixtes. 

> 💡 Par défaut, toutes les interfaces série Cisco utilisent l'encapsulation cHDLC. Il est donc important de vérifier la cohérence du protocole des deux côtés d'un lien série. 
 
## Références 

- [ISO 13239 — Norme officielle HDLC](https://www.iso.org/fr/standard/8561.html)
- https://youtu.be/LsLKs2yPmBI?si=Pzw0oaPY_7VbpNqm
- https://github.com/vladimirlarass/lab-CCNA