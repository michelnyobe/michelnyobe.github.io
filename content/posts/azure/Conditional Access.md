---
title: Conditional Access
date: 2025-06-30
draft: true
tags:
  - azure
  - Hacking
  - azure
  - IAM
categories:
  - azure
summary: Azure managed identities
showToc: true
tocOpen: true
---
La sécurité moderne s'étend au-delà du périmètre réseau d'une organisation et inclut l'identité des utilisateurs et des appareils. Les organisations utilisent désormais des signaux basés sur l'identité pour prendre des décisions de contrôle d'accès.

## Signaux communs
L'accès conditionnel prend en compte les signaux provenant de diverses sources lors de la prise de décisions d'accès.
Ces signaux comprennent :
- Appartenance d'utilisateur ou de groupe
    - Les politiques peuvent être ciblées sur des utilisateurs et des groupes spécifiques, offrant aux administrateurs un contrôle précis sur l'accès.
- Informations de localisation IP
    - Les organisations peuvent créer des plages d’adresses IP fiables qui peuvent être utilisées lors de la prise de décisions stratégiques.
    - Les administrateurs peuvent spécifier des plages IP de pays ou de régions entières à partir desquelles bloquer ou autoriser le trafic.
- Appareil
    - Les utilisateurs disposant d'appareils de plates-formes spécifiques ou marqués d'un état spécifique peuvent être utilisés lors de l'application des politiques d'accès conditionnel.
    - Utilisez des filtres pour les appareils afin de cibler les politiques sur des appareils spécifiques tels que les postes de travail à accès privilégié.
- Application
    - Les utilisateurs qui tentent d’accéder à des applications spécifiques peuvent déclencher différentes politiques d’accès conditionnel.
- Détection des risques en temps réel et calculés
    - L'intégration de Signals avec Microsoft Entra ID Protection permet aux stratégies d'accès conditionnel d'identifier et de corriger les utilisateurs à risque et les comportements de connexion.
- Microsoft Defender pour les applications cloud
    - Permet de surveiller et de contrôler en temps réel l'accès aux applications et les sessions des utilisateurs. Cette intégration améliore la visibilité et le contrôle des accès et des activités au sein de votre environnement cloud.