---
title: "Inclusion de Fichier Local"
date: 2025-05-07T15:00:00+02:00
draft: false
tags: ["Outils", "Pentester"]
categories: ["Outils"]
summary: "SQLMAp"
showToc: true
tocOpen: true
---

#### LFI Basique
##### Path Traversal
Encodage

```
i l'application web cible n'autorisait pas . et / dans notre entrée, nous pouvons encoder en URL ../ en %2e%2e%2f
```


Filtres PHP

Il existe quatre différents types de filtres disponibles, qui sont 
- les Filtres de Chaînes de Caractères , 
- les Filtres de Conversion 
- les Filtres de Compression 
- les Filtres de Chiffrement . 

Vous pouvez en lire plus sur chaque filtre sur leur lien respectif, mais le filtre qui est utile pour les attaques LFI est le filtre convert .base64-encode, sous Filtres de Conversion.

#### Test d'inclusion de fichiers distants

La vulnérabilité d'inclusion de fichiers permet à un attaquant d'inclure un fichier, généralement en exploitant un mécanisme d'« inclusion dynamique de fichiers » implémenté dans l'application cible.
##### Vérifier la RFI
toute inclusion d'URL distante en PHP nécessiterait que le paramètre allow_url_include soit activé. 


```
echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep allow_url_include
```


#### FTP

```
sudo python -m pyftpdlib -p 21
```


LFI et téléversement de fichiers

Les fonctionnalités de téléversement de fichiers sont omniprésentes dans la plupart des applications web modernes, car les utilisateurs ont généralement besoin de configurer leur profil et leur utilisation de l'application web en téléversant leurs données. 

Empoisonnement de journaux (Log Poisoning)

#### Analyse Automatisée
