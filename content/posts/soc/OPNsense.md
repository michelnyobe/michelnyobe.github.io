


## Sommaire

1. [Introduction — Pourquoi ?](https://claude.ai/chat/b6911413-e4fe-4152-95fd-514a6655b7b8#1-introduction--pourquoi-)
2. [Présentation de la solution — Quoi ?](https://claude.ai/chat/b6911413-e4fe-4152-95fd-514a6655b7b8#2-pr%C3%A9sentation-de-la-solution--quoi-)
3. [Environnement et architecture — Où ?](https://claude.ai/chat/b6911413-e4fe-4152-95fd-514a6655b7b8#3-environnement-et-architecture--o%C3%B9-)
4. [Installation — Comment ? (partie 1)](https://claude.ai/chat/b6911413-e4fe-4152-95fd-514a6655b7b8#4-installation--comment--partie-1)
5. [Configuration — Comment ? (partie 2)](https://claude.ai/chat/b6911413-e4fe-4152-95fd-514a6655b7b8#5-configuration--comment--partie-2)
6. [Tests et validation](https://claude.ai/chat/b6911413-e4fe-4152-95fd-514a6655b7b8#6-tests-et-validation)
7. [Exploitation et maintenance](https://claude.ai/chat/b6911413-e4fe-4152-95fd-514a6655b7b8#7-exploitation-et-maintenance)
8. [Annexes](https://claude.ai/chat/b6911413-e4fe-4152-95fd-514a6655b7b8#8-annexes)

---

## 1. Introduction — Pourquoi ?

### 1.1. Contexte du projet

> Décrire le contexte du déploiement réseau au sein de l'agence : besoins, situation existante, raisons du projet.

### 1.2. Objectifs de la mise en place d'OPNsense

> Sécurisation du réseau, filtrage du trafic, segmentation (VLAN), accès distant (VPN), etc.

### 1.3. Périmètre et limites du document

> Ce que couvre (et ne couvre pas) ce document.

---

## 2. Présentation de la solution — Quoi ?

### 2.1. Qu'est-ce qu'OPNsense ?

> OPNsense est un pare-feu / routeur open source basé sur FreeBSD. Rôle et principales fonctions.

### 2.2. Fonctionnalités retenues pour l'agence

> Lister les fonctions activées : pare-feu, DHCP, DNS, VPN, IDS/IPS, proxy, portail captif…

### 2.3. Justification du choix

> Pourquoi OPNsense plutôt qu'une autre solution (le cas échéant).

### 2.4. Version et licence

- **Version OPNsense :** [ex. 24.x]
- **Licence :** BSD (open source)

---

## 3. Environnement et architecture — Où ?

### 3.1. Schéma de l'architecture réseau cible

![Schéma d'architecture réseau](https://claude.ai/chat/captures/figure-01-architecture.png) _Figure 1 — Architecture réseau cible de l'agence._

### 3.2. Plan d'adressage IP

|Interface / VLAN|Rôle|Sous-réseau|Passerelle|
|---|---|---|---|
|WAN|Accès Internet|[x.x.x.x/xx]|[x.x.x.x]|
|LAN|Réseau interne|[192.168.x.0/24]|[192.168.x.1]|
|VLAN 10|[Ex. Bureautique]|[192.168.10.0/24]|[192.168.10.1]|
|VLAN 20|[Ex. Wi-Fi invités]|[192.168.20.0/24]|[192.168.20.1]|

### 3.3. Prérequis matériels

- Appliance / serveur : [modèle, CPU, RAM, stockage]
- Interfaces réseau : [nombre et type]

### 3.4. Prérequis logiciels

- Image ISO OPNsense : [version]
- Support d'installation : clé USB / accès console
- Poste d'administration avec navigateur

---

## 4. Installation — Comment ? (partie 1)

### 4.1. Préparation du support d'installation

![Préparation du support](https://claude.ai/chat/captures/figure-02-preparation-support.png) _Figure 2 — Écriture de l'image ISO sur la clé USB._

### 4.2. Démarrage et boot sur l'ISO

![Boot sur l'ISO](https://claude.ai/chat/captures/figure-03-boot-iso.png) _Figure 3 — Démarrage sur le support d'installation._

### 4.3. Étapes de l'installation guidée

![Installation étape 1](https://claude.ai/chat/captures/figure-04-install-1.png) _Figure 4 — [Décrire l'étape]._

![Installation étape 2](https://claude.ai/chat/captures/figure-05-install-2.png) _Figure 5 — [Décrire l'étape]._

### 4.4. Premier démarrage et accès à l'interface web

![Accès interface web](https://claude.ai/chat/captures/figure-06-interface-web.png) _Figure 6 — Page de connexion à l'interface d'administration._

---

## 5. Configuration — Comment ? (partie 2)

### 5.1. Assistant de configuration initial

![Assistant de configuration](https://claude.ai/chat/captures/figure-07-wizard.png) _Figure 7 — Assistant de configuration initial._

### 5.2. Configuration des interfaces (WAN / LAN / VLAN)

![Configuration des interfaces](https://claude.ai/chat/captures/figure-08-interfaces.png) _Figure 8 — Paramétrage des interfaces réseau._

### 5.3. Règles de pare-feu

![Règles de pare-feu](https://claude.ai/chat/captures/figure-09-regles-firewall.png) _Figure 9 — Création des règles de filtrage._

> Documenter chaque règle : source, destination, port, action, description.

### 5.4. Services réseau : DHCP et DNS

![Configuration DHCP](https://claude.ai/chat/captures/figure-10-dhcp.png) _Figure 10 — Configuration du serveur DHCP._

### 5.5. VPN (site-à-site ou accès distant)

![Configuration VPN](https://claude.ai/chat/captures/figure-11-vpn.png) _Figure 11 — Paramétrage du VPN._

> À compléter uniquement si le VPN fait partie du périmètre.

### 5.6. Autres services activés

> Proxy, IDS/IPS (Suricata), portail captif, etc.

---

## 6. Tests et validation

### 6.1. Vérification de la connectivité

> Tests de ping, accès Internet, résolution DNS.

### 6.2. Tests des règles de filtrage

> Vérifier que les flux autorisés passent et que les flux bloqués sont refusés.

### 6.3. Tableau de recette

|N°|Test réalisé|Résultat attendu|Résultat obtenu|Statut|
|---|---|---|---|---|
|1|Accès Internet depuis le LAN|Accès autorisé||✅ / ❌|
|2|Blocage inter-VLAN|Accès refusé||✅ / ❌|
|3|Connexion VPN|Tunnel établi||✅ / ❌|

---

## 7. Exploitation et maintenance

### 7.1. Sauvegarde et restauration de la configuration

> Procédure d'export/import de la configuration (System > Configuration > Backups).

### 7.2. Mises à jour du système

> Procédure de mise à jour OPNsense.

### 7.3. Supervision et journaux (logs)

> Où consulter les logs, alertes et tableaux de bord.

### 7.4. Procédure de retour arrière (rollback)

> Étapes pour revenir à une configuration antérieure en cas de problème.

---

## 8. Annexes

### 8.1. Glossaire

|Terme|Définition|
|---|---|
|VLAN|Réseau local virtuel|
|WAN|Réseau étendu (accès Internet)|
|LAN|Réseau local|
|IDS/IPS|Système de détection / prévention d'intrusion|

### 8.2. Références et documentation officielle

- Documentation officielle OPNsense : https://docs.opnsense.org
- [Autres références internes]

### 8.3. Historique des versions du document

|Version|Date|Auteur|Modifications|
|---|---|---|---|
|1.0|[JJ/MM/AAAA]|[Votre nom]|Création initiale|