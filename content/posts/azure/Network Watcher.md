---
title: "Comprendre Azure Network Watcher : Supervision, Diagnostic et Analyse du Trafic"
date: 2025-08-18
draft: false
tags:
  - azure
  - network-watcher
  - cloud-security
  - monitoring
  - diagnostic
categories:
  - cloud
  - cybersecurity
  - azure
summary: "Un guide complet sur Azure Network Watcher : supervision du réseau, outils de diagnostic et analyse du trafic pour mieux surveiller et sécuriser vos environnements IaaS."
showToc: true
tocOpen: true
---

# Introduction  

**Azure Network Watcher** est un service intégré d’Azure qui fournit une suite d’outils permettant de **surveiller, diagnostiquer et analyser le trafic réseau** dans vos environnements **Infrastructure-as-a-Service (IaaS)**.  
Il offre aux administrateurs et aux ingénieurs cloud la possibilité de :  
- comprendre la topologie réseau,  
- identifier rapidement les problèmes de connectivité,  
- analyser les flux de trafic pour renforcer la sécurité,  
- optimiser la performance de l’infrastructure.  

Network Watcher se compose de trois grands ensembles d’outils et fonctionnalités :  

- **Supervision**  
- **Outils de diagnostic réseau**  
- **Analyse du trafic**  

---

## Supervision avec Azure Network Watcher  

Network Watcher propose deux outils de surveillance qui permettent de visualiser et de suivre l’état des ressources :  

### 🔹 Topologie  
La fonctionnalité **Topologie** génère une **vue graphique de l’ensemble du réseau**, incluant les réseaux virtuels, sous-réseaux, interfaces réseau (NIC), NSG (Network Security Groups), et machines virtuelles.  
➡️ Cela permet aux équipes IT de **comprendre rapidement la configuration du réseau** et de détecter des erreurs de déploiement (par exemple, une VM connectée au mauvais sous-réseau).  

### 🔹 Moniteur de connexion  
Le **Connection Monitor** fournit une **surveillance de bout en bout** entre différents points de terminaison (Azure, locaux ou hybrides).  
➡️ Il mesure la latence, les pertes de paquets et la disponibilité des connexions, ce qui est essentiel pour :  
- diagnostiquer les problèmes de performance,  
- détecter des interruptions intermittentes,  
- assurer la qualité de service (QoS) des applications critiques.  

---

## Outils de diagnostic réseau dans Azure Network Watcher  

Network Watcher inclut **7 outils puissants** pour diagnostiquer et résoudre des problèmes réseau :  

### 🔹 Vérification du flux IP  
Permet de **tester si le trafic est autorisé ou bloqué** vers/depuis une machine virtuelle.  
➡️ Très utile pour vérifier les **problèmes de filtrage liés aux règles NSG, pare-feu Azure ou UDR (User Defined Routes)**.  

### 🔹 Diagnostics NSG  
Analyse en détail les règles appliquées par un **Network Security Group**.  
➡️ Cela aide à identifier rapidement **pourquoi un trafic est bloqué** (par ex. une règle "deny all" en priorité élevée).  

### 🔹 Prochaine étape (Next Hop)  
Vérifie la **route empruntée par un paquet** vers sa destination.  
➡️ Indispensable pour comprendre si le trafic passe par une **passerelle VPN, un pare-feu Azure Firewall, ou Internet**.  

### 🔹 Règles de sécurité effectives  
Affiche l’ensemble des **règles cumulées** appliquées à une interface réseau (NSG + sécurité Azure).  
➡️ Cela évite la confusion entre les règles définies et celles réellement appliquées.  

### 🔹 Résolution des problèmes de connexion  
Teste une connexion entre une VM et une destination (FQDN, URI, IPv4).  
➡️ Très pratique pour diagnostiquer un problème d’accès à une application ou à une base de données.  

### 🔹 Capture de paquets  
Permet de lancer une **capture de trafic (PCAP)** directement depuis une VM, sans installer Wireshark localement.  
➡️ Utile pour l’analyse **intrusion, performance applicative ou investigation forensique**.  

### 🔹 Résolution des problèmes de VPN  
Outil spécifique pour diagnostiquer les problèmes liés aux **passerelles VPN et connexions site-à-site ou point-à-site**.  
➡️ Il vérifie l’état des tunnels, les négociations IPsec et la stabilité des sessions.  

---

## Analyse du trafic avec Azure Network Watcher  

Network Watcher propose également deux outils dédiés au suivi du trafic :  

### 🔹 Journaux de flux NSG  
Permettent de **consigner les informations sur le trafic IP entrant et sortant** à travers un NSG.  
➡️ Ces données peuvent être stockées dans **Azure Storage**, envoyées vers **Log Analytics** ou analysées dans **SIEM comme Microsoft Sentinel**.  

### 🔹 Traffic Analytics  
Outil avancé qui transforme les journaux de flux en **visualisations riches et exploitables**.  
➡️ Il permet d’identifier :  
- les **top talkers** (VM qui génèrent le plus de trafic),  
- les **connexions suspectes** (ex : trafic vers des pays inhabituels),  
- la répartition interne/externe du trafic.  

![Analyse de trafic](/images/20250818185109.png)  

---

## Utilisation et quotas  

La section **Utilisation + quotas** fournit un **résumé global de l’état des ressources réseau** déployées dans une région ou un abonnement.  
➡️ Elle permet de suivre :  
- le nombre de réseaux virtuels,  
- les limites actuelles (quotas),  
- la consommation par rapport aux seuils disponibles.  

![quotas](/images/20250818185451.png)  

---

## Limites de Network Watcher  

Bien que très puissant, **Network Watcher présente quelques limites** :  
- Il est **limité aux ressources IaaS** (VM, réseaux virtuels, NSG, passerelles). Les services PaaS (ex. App Service) ne sont pas directement pris en charge.  
- Certains outils (comme la capture de paquets) peuvent générer des coûts additionnels.  
- Il ne remplace pas un **SIEM ou une solution complète de monitoring** mais agit comme un outil de diagnostic et de troubleshooting complémentaire.  

---

# Conclusion  

**Azure Network Watcher** est un outil essentiel pour les administrateurs cloud, les ingénieurs réseau et les équipes de sécurité.  
Il permet de **gagner du temps dans le diagnostic**, de **visualiser en temps réel la topologie réseau**, et de **renforcer la sécurité en identifiant les flux suspects**.  

➡️ Couplé avec **Azure Monitor, Sentinel et Defender for Cloud**, Network Watcher devient un pilier central pour la **supervision et la cybersécurité dans Azure**.  
