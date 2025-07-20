---
title: "Azure Hacking"
date: 2025-07-18
draft: false
tags: ["azure", "security", "Hacking"]
categories: ["azure"]
summary: "Azure Hacking"
showToc: true
tocOpen: true
---

Avec l'adoption massive du cloud computing, les entreprises migrent de plus en plus leurs infrastructures vers des environnements flexibles, scalables et accessibles à distance. Parmi les principaux fournisseurs de services cloud, **Microsoft Azure** occupe une place de leader, offrant un large éventail de services allant de la simple machine virtuelle à des solutions complexes d'intelligence artificielle, de gestion d'identité, ou encore de DevOps.
Cependant, cette complexité fonctionnelle ouvre également la voie à de nombreuses **erreurs de configuration**, **abus de privilèges** et **vulnérabilités potentielles**. Azure, en tant que plateforme, peut être la cible de différentes techniques offensives utilisées par des attaquants internes ou externes cherchant à compromettre l’intégrité, la disponibilité ou la confidentialité des ressources cloud d’une organisation.

# Accès Initial (Initial Access)

- Credential stuffing (Azure AD / Entra ID)
- Phishing avec MFA Fatigue / Consent Phishing
- Token replay (refresh ou access tokens volés)
- Abuse des partages publics (Blob/Files avec ACL public)
- Exploitation de credentials hardcodés (dans code, Azure DevOps, GitHub Actions)
- Exploitation de mots de passe faibles sur comptes invités ou service principals
- Exploitation d’une app mal configurée (malicious redirect URI, consent mal géré)

# Évasion des Défenses (Defense Evasion)

- Suppression des logs dans Azure Monitor / Log Analytics
- Usage de credentials temporaires (MSI tokens, access tokens via IMDS) 
- Utilisation d’applications "trusted" (abuse de consentements OAuth)
- Ajout de règles de suppression d’alertes dans Microsoft Defender for Cloud
- ByPass Conditional Access (via Identity Protection, ou device spoofing)

# Escalade de Privilèges (Privilege Escalation)

- Abus de rôles mal configurés :
    - _Owner_, _User Access Administrator_, _Contributor_, etc. 
- Chaining permissions :
    - Lecture de secrets dans Key Vault → accès admin
    - Escalade via Automation Account / Runbook
    - Abuse d’un Logic App ou Function App
- Abus de Managed Identities (MSI) pour obtenir des tokens avec haut privilège