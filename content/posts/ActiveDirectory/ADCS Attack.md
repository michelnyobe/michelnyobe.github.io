---
title: "ADCS Attack"
date: 2026-03-06T10:00:00+02:00
draft: true
tags: ["pentest", "Active Directory", "PowerView", "reconnaissance", "CRTP", "red team"]
categories: ["Red Team", "pentester"]
summary: ""
showToc: true
tocOpen: true
---
AD CS est un rôle serveur qui fonctionne comme l'implémentation de l'infrastructure à clés publiques PKI de Microsoft.
Comme prévu, il s'intègre étroitement à Active Directory et permet l'émission de certificats, qui sont des documents électroniques signés numériquement au format X.509, utilisables pour le chiffrement, la signature de messages et/ou l'authentification (notre axe de recherche).



https://github.com/GhostPack/PSPKIAudit
https://posts.specterops.io/certified-pre-owned-d95910965cd2

# Énumération des modèles de certificats 

Windows dispose d'outils intégrés très performants permettant de lister tous les modèles de certificats et leurs stratégies associées. La méthode la plus courante consiste à utiliser certutil.

```
certutil -v -template > cert_templates.txt
```

### Paramètre 1 : Autorisations pertinentes

Pour que cette faille fonctionne, nous devons disposer des autorisations nécessaires pour générer une demande de certificat. Nous recherchons donc un modèle où notre utilisateur possède l' autorisation « Autoriser l'inscription » ou « Autoriser le contrôle total » . 

### Paramètre 2 : Authentification du client
ne fois la liste des modèles de certificats autorisés réduite, l'étape suivante consiste à vérifier que le certificat possède l' EKU d'authentification client . Cet EKU signifie que le certificat peut être utilisé pour l'authentification Kerberos . Il existe d'autres méthodes d'exploitation des certificats, mais nous nous concentrerons ici principalement sur cet EKU.

### Paramètre 3 : Le client spécifie le SAN
Enfin, et c'est un point crucial, nous devons vérifier que le modèle nous permet, en tant que client de certificat, de spécifier le nom alternatif du sujet (SAN). Le SAN correspond généralement à l'URL du site web à chiffrer
Pour trouver ces modèles, nous recherchons l' indicateur de propriété CT_FLAG_ENROLLEE_SUPPLIES_SUBJECT qui doit être défini sur 1. Cela indique que nous pouvons spécifier nous-mêmes le SAN.
