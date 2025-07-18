---
title: Azure managed identities
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

La gestion des secrets, des identifiants, des certificats et des clés utilisés pour sécuriser les communications entre les services constitue un défi courant pour les développeurs.
## Que sont les identités gérées ?

Les identités gérées par Azure sont une fonctionnalité d'Azure qui permet aux applications de s'authentifier auprès des services Azure sans avoir à gérer les informations d'identification. Cette fonctionnalité renforce la sécurité en évitant aux développeurs de stocker les informations d'identification dans leur code, réduisant ainsi le risque de fuite d'informations d'identification. Les identités gérées sont entièrement gérées par Azure, ce qui signifie que la plateforme prend en charge leur cycle de vie, y compris la rotation automatique des informations d'identification.

Il existe deux types d'identités gérées : attribuées par le système et attribuées par l'utilisateur. Une identité gérée attribuée par le système est créée directement dans une instance de service Azure, tandis qu'une identité gérée attribuée par l'utilisateur peut être créée en tant que ressource Azure autonome et attribuée à une ou plusieurs instances de service Azure. Ces deux types permettent aux applications d'accéder en toute sécurité aux ressources Azure prenant en charge l'authentification Azure Active Directory.

L'utilisation d'identités gérées simplifie le processus d'authentification des applications exécutées dans Azure, leur permettant de se connecter en toute sécurité à d'autres services Azure sans avoir recours à des secrets codés en dur. Cette fonctionnalité est particulièrement utile pour les applications nécessitant un accès à des ressources Azure telles qu'Azure Key Vault, Azure Storage et Azure SQL Database. En exploitant les identités gérées, les entreprises peuvent renforcer leur sécurité tout en simplifiant le processus de développement.

## Avantages 

Voici quelques-uns des avantages de l’utilisation d’identités gérées :

- Vous n'avez pas besoin de gérer vos identifiants. Vous n'y avez même pas accès.
- Vous pouvez utiliser des identités gérées pour vous authentifier auprès de n’importe quelle ressource prenant en charge l’authentification Microsoft Entra , y compris vos propres applications.
- Les identités gérées peuvent être utilisées sans frais supplémentaires.

## Types d'identités gérées

**Affecté par le système** . Certaines ressources Azure, comme les machines virtuelles, permettent d'activer une identité managée directement sur la ressource.
**Affectée par l'utilisateur** . Vous pouvez également créer une identité gérée en tant que ressource Azure autonome. Vous pouvez créer une identité gérée affectée par l'utilisateur et l'affecter à une ou plusieurs ressources Azure




https://learn.microsoft.com/en-us/entra/identity/managed-identities-azure-resources/overview