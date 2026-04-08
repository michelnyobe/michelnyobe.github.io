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

# C'est quoi un access conditionnel 

Pendant longtemps, la sécurité informatique des entreprises reposait sur un modèle simple : le périmètre réseau. L'idée était claire si un utilisateur se trouvait à l'intérieur du réseau de l'entreprise, il était considéré comme fiable et pouvait accéder aux ressources dont il avait besoin. Ce modèle fonctionnait bien à une époque où les employés travaillaient uniquement depuis leurs bureaux, sur des machines gérées par l'entreprise, connectées à un réseau local.

Mais le monde a profondément changé. Avec l'essor du télétravail, de la mobilité et de l'adoption massive du cloud, les frontières du réseau d'entreprise ont éclaté. Les collaborateurs se connectent aujourd'hui depuis leur domicile, un aéroport ou un espace de coworking, depuis des appareils personnels, pour accéder à des applications hébergées dans le cloud comme Microsoft 365, Salesforce ou Google Workspace. Le réseau d'entreprise traditionnel n'est plus le point de passage obligatoire.

Cette évolution a créé une faille majeure : **l'identité est devenue la nouvelle frontière de sécurité**, mais elle reste vulnérable. Un simple identifiant et un mot de passe peuvent être volés, phishés ou compromis. Une fois en possession de ces credentials, un attaquant peut se connecter comme s'il était un employé légitime  depuis n'importe où dans le monde sans que personne ne le détecte.

C'est face à ce constat que la question s'impose : **comment accorder le bon accès, à la bonne personne, dans le bon contexte, tout en bloquant les tentatives malveillantes ?** C'est précisément le problème que l'accès conditionnel cherche à résoudre.
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
# mettre en place un access conditionnel 
L’accès conditionnel Azure Active Directory est une fonctionnalité avancée d’Azure AD qui vous permet de spécifier des stratégies détaillées qui contrôlent qui peuvent accéder à vos ressources.

>   [!CAUTION] 
>   *Important*
> L’accès conditionnel Azure Active Directory est disponible dans le niveau Premium d’Azure AD. Pour plus d’informations sur Azure AD Premium,


Dans le [portail Azure](https://portal.azure.com/), ouvrez votre locataire Active Directory, puis ouvrez les paramètres **de sécurité** , puis cliquez sur **Accès conditionnel**.

![[Pasted image 20260408132707.png]]

