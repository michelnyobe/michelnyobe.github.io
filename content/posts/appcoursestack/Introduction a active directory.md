---
title: "intro AD"
date: 2026-03-06T10:00:00+02:00
draft: true
tags: ["pentest", "Active Directory", "PowerView", "reconnaissance", "CRTP", "red team"]
categories: ["Red Team", "pentester"]
summary: ""
showToc: true
tocOpen: true
---
# sommaires

1. Defintion des concepts 
2. Gerer les controleurs de  domaine ADDS et les roles FSMO

### Partie 1 — Fondamentaux et architecture

Comprendre ce qu'est un annuaire : forêts, domaines, arbres et unités d'organisation (OU). Les objets d'AD (utilisateurs, groupes, ordinateurs, contacts), le schéma, et la structure logique vs physique. Le rôle central du DNS, les sites et sous-réseaux, et la réplication entre contrôleurs de domaine.

### Partie 2 — Installation et configuration

Promouvoir un contrôleur de domaine, gérer plusieurs DC pour la redondance, comprendre les rôles FSMO (les 5 rôles à maître unique) et le catalogue global. Configurer un domaine de zéro dans un laboratoire.

### Partie 3 — Gestion quotidienne (administration)

Le cœur du métier : créer et gérer les comptes utilisateurs et ordinateurs, structurer les groupes (portées : domaine local, global, universel), organiser les OU de façon cohérente, et automatiser ces tâches avec PowerShell (le module Active Directory est incontournable ici).

### Partie 4 — Stratégies de groupe (GPO)

Comprendre le fonctionnement des GPO, leur ordre d'application (LSDOU), l'héritage et le blocage, le filtrage de sécurité et WMI. Déployer des paramètres de sécurité, des scripts, des lecteurs réseau et des configurations logicielles à grande échelle.

### Partie 5 — DNS, DHCP et services associés

Bien maîtriser le DNS intégré à AD (zones, enregistrements, redirecteurs), son lien avec le bon fonctionnement d'AD, et les services que l'infra gère souvent en parallèle comme DHCP.

### Partie 6 — Sécurisation et durcissement

Du point de vue infra : politique de mots de passe et de verrouillage, modèle de délégation d'administration (déléguer sans donner trop de droits), séparation des comptes admin et comptes utilisateurs, LAPS pour les mots de passe admin locaux, sécurisation des contrôleurs de domaine, et suppression des protocoles obsolètes (SMBv1, NTLMv1).

### Partie 7 — Sauvegarde, supervision et continuité

Sauvegarder l'état système des DC, comprendre la restauration (normale et faisant autorité), supervision de la santé d'AD (réplication, services), et plan de reprise en cas de sinistre. C'est ce qui distingue un bon administrateur : anticiper la panne.

### Partie 8 — Maintenance et évolution

Mises à jour et niveaux fonctionnels de forêt/domaine, migration et montée de version des DC, nettoyage des objets obsolètes, et bonnes pratiques de documentation de l'infrastructure.

- monter le niveau fonctionnel 
- 