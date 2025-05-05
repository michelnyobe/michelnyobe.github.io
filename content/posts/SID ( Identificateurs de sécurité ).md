---
title: "SID ( Identificateurs de sécurité )"
date: 2025-05-05T19:00:00+02:00
draft: false
tags: ["Active Directory", "blog", "SID", "attaques", "windows"]
categories: ["Active Directory"]
summary: "Un identificateur de sécurité (SID) est essentiel dans la gestion des accès et de la sécurité dans les systèmes Windows. Découvrez comment il fonctionne, son rôle dans l'authentification et sa structure binaire."
cover:
  image: "images/"
  alt: "SID"
showToc: true
tocOpen: true
---
# Comprendre les Identificateurs de Sécurité (SID) dans Active Directory

Un **identificateur de sécurité (SID)** est utilisé pour identifier de manière unique un principal de sécurité ou un groupe de sécurité dans un environnement Windows. Ces identifiants jouent un rôle fondamental dans la gestion des autorisations et l'authentification au sein d’un domaine Active Directory.

## 1. Qu’est-ce qu’un principal de sécurité ?

Un **principal de sécurité** est toute entité pouvant être authentifiée par le système d’exploitation. Il peut s’agir :

- d’un **compte d’utilisateur**,
    
- d’un **compte d’ordinateur**,
    
- ou d’un **thread ou processus** exécuté dans le contexte de sécurité d’un compte existant.
    

Chaque fois qu’un utilisateur se connecte, le système génère un **jeton d’accès** contenant :

- le **SID de l’utilisateur**,
    
- les **droits de l’utilisateur**,
    
- et les **SID des groupes** auxquels l’utilisateur appartient.
    

Ce **jeton d’accès** définit le **contexte de sécurité** utilisé par l’utilisateur pour interagir avec le système.

## 2. Architecture d’un SID

Un SID est une **structure de données binaire** contenant un nombre variable de valeurs. Les premières valeurs décrivent la structure du SID, tandis que les suivantes identifient l’autorité, le domaine et l’objet.

Une fois converti au format lisible, un SID suit généralement cette notation standard :

CopierModifier

`S-R-X-Y1-Y2-Yn-1-Yn`

### Exemple :

Le SID du groupe intégré **Administrateurs** est représenté comme suit :

CopierModifier

`S-1-5-32-544`

Décomposons ce SID :

- **S** : identifie qu’il s’agit d’un SID.
    
- **1** : niveau de révision.
    
- **5** : autorité d’identification (Autorité NT).
    
- **32** : identifiant du domaine (Builtin).
    
- **544** : identifiant relatif (RID) pour le groupe « Administrateurs ».
    

## 3. SID connus

Certains SID sont **pré-définis** et restent constants, quel que soit l’environnement ou le système. Ces **SID connus** sont générés automatiquement lors de l’installation du système d’exploitation ou de la création du domaine. Ils sont utilisés pour identifier :

- des **groupes génériques** (Administrateurs, Utilisateurs, etc.),
    
- ou des **comptes système** standards.
    

Par exemple :

- **S-1-5-18** : représente le compte **SYSTEM**.
    
- **S-1-5-19** : correspond au **Service local**.
    
- **S-1-5-32-544** : désigne le groupe **Administrateurs**.
    

---

## Références

- [Comprendre les identificateurs de sécurité - Microsoft Docs](https://learn.microsoft.com/fr-fr/windows-server/identity/ad-ds/manage/understand-security-identifiers)