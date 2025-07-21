---
title: Entra ID
date: 2025-06-21T20:00:00+02:00
draft: true
tags:
  - resources
  - lien
  - autoapprentissage
categories:
  - resources
summary: Entra ID
showToc: true
tocOpen: true
---
# Qu'est-ce que Microsoft Entra ID ?

Microsoft Entra ID est un service cloud pour la gestion des identité et des accès que des employés peuvent utiliser pour accéder  a des ressources externes .

## Que sont les licences Microsoft Entra ID ?

- **Microsoft Entra ID gratuit.** Offre la gestion des utilisateurs et des groupes, la synchronisation des annuaires locaux, des rapports de base, la modification des mots de passe en libre-service pour les utilisateurs cloud et l'authentification unique sur Azure, Microsoft 365 et de nombreuses applications SaaS populaires.
- **Microsoft Entra ID P1.** Outre les fonctionnalités gratuites, P1 permet également à vos utilisateurs hybrides d'accéder aux ressources locales et cloud. Il prend également en charge des fonctions d'administration avancées, telles que les groupes d'appartenance dynamiques, la gestion des groupes en libre-service, Microsoft Identity Manager et les fonctionnalités de réécriture dans le cloud, permettant la réinitialisation des mots de passe en libre-service pour vos utilisateurs locaux.
- **Microsoft Entra ID P2.** inclut des fonctionnalités supplémentaires à celles des versions gratuite et P1. P2 inclut Microsoft Entra ID Protection pour fournir un accès conditionnel basé sur les risques à vos applications et données critiques de l'entreprise, ainsi que la gestion des identités privilégiées pour identifier, restreindre et surveiller les administrateurs, leur accès aux ressources et fournir un accès juste-à-temps en cas de besoin.

Les identités gérées par Azure sont une fonctionnalité d'Azure qui permet aux applications de s'authentifier auprès des services Azure sans avoir à gérer les informations d'identification.

## Types d'utilisateurs

- **Membre interne** : ces utilisateurs sont très probablement des employés à temps plein de votre organisation.  
- **Invité interne** : ces utilisateurs possèdent un compte dans votre locataire, mais disposent de privilèges d'invité. Il est possible qu'ils aient été créés dans votre locataire avant la disponibilité de la collaboration B2B.
- **Membre externe** : ces utilisateurs s'authentifient via un compte externe, mais disposent d'un accès membre à votre locataire. Ces types d'utilisateurs sont courants dans les organisations multilocataires.
- **Invité externe** : ces utilisateurs sont de véritables invités de votre locataire qui s'authentifient à l'aide d'une méthode externe et qui disposent de privilèges de niveau invité.
# les groupes Microsoft Entra
Il existe deux types de groupes et trois types d'adhésion. Consultez les options pour trouver la combinaison adaptée à votre situation.
### Types de groupes
**Sécurité**: utilisé pour gérer l'accès des utilisateurs et des ordinateurs aux ressources partagées.
**Microsoft 365** : offre des opportunités de collaboration en donnant aux membres du groupe l’accès à une boîte aux lettres partagée, un calendrier, des fichiers, des sites SharePoint, etc.

### Types d'adhésion
**Attribué** : vous permet d'ajouter des utilisateurs spécifiques en tant que membres d'un groupe et de disposer d'autorisations uniques.
**Utilisateur dynamique** : vous permet d'utiliser des règles d'adhésion dynamiques pour ajouter et supprimer automatiquement des membres.
**Appareil dynamique** : vous permet d'utiliser des règles de groupe dynamiques pour ajouter et supprimer automatiquement des appareils.

Après avoir créé un groupe Microsoft Entra, vous devez lui accorder les accès appropriés.

## la gestion des accès dans Microsoft Entra ID
L'identifiant Microsoft Entra vous permet d'accéder aux ressources de votre organisation en attribuant des droits d'accès à un utilisateur unique ou à un groupe Microsoft Entra entier. L'utilisation de groupes permet au propriétaire de la ressource ou du répertoire Microsoft Entra d'attribuer un ensemble d'autorisations d'accès à tous les membres du groupe. Le propriétaire de la ressource ou du répertoire peut également accorder des droits de gestion à un responsable de service ou à un administrateur du support technique, lui permettant ainsi d'ajouter et de supprimer des membres.

Après avoir créé un groupe, vous devez décider comment attribuer les droits d'accès. Explorez les différentes méthodes d'attribution des droits d'accès pour déterminer la procédure la plus adaptée à votre situation.
- **Affectation directe**. Le propriétaire de la ressource affecte directement l'utilisateur à la ressource.  
- **Affectation de groupe**. Le propriétaire de la ressource lui attribue un groupe Microsoft Entra, ce qui donne automatiquement accès à tous ses membres. L'appartenance à un groupe est gérée par le propriétaire du groupe et celui de la ressource, ce qui permet à chacun d'eux d'ajouter ou de supprimer des membres du groupe.  
- **Affectation basée sur des règles**. Le propriétaire de la ressource crée un groupe et utilise une règle pour définir les utilisateurs affectés à une ressource spécifique. Cette règle est basée sur des attributs attribués à chaque utilisateur. Le propriétaire de la ressource gère la règle et détermine les attributs et les valeurs requis pour autoriser l'accès à la ressource.  
- **Attribution d'autorité externe**. L'accès provient d'une source externe, telle qu'un annuaire local ou une application SaaS. Dans ce cas, le propriétaire de la ressource attribue un groupe pour lui donner accès, puis la source externe gère les membres du groupe

Il peut également configurer le groupe pour qu'il accepte automatiquement tous les utilisateurs qui le rejoignent ou qu'il exige leur approbation.

## Microsoft Entra External ID

Microsoft Entra External ID combine des solutions performantes pour collaborer avec des personnes extérieures à votre organisation. Grâce à ces fonctionnalités, vous pouvez autoriser des identités externes à accéder en toute sécurité à vos applications et ressources.
