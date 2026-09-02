---
title: "ffuf"
date: 2025-05-07T15:00:00+02:00
draft: false
tags: ["Outils", "Pentester"]
categories: ["Outils"]
summary: "ffuf"
showToc: true
tocOpen: true
---
https://github.com/ffuf/ffuf
https://github.com/danielmiessler/SecLists

# Fuzzing de répertoire 
les deux principales options concernent -wles listes de mots et l'URL. On peut associer une liste de mots à un mot-clé pour y faire référence dans l'environnement de test

```bash
ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ
```

Ensuite, comme nous souhaitons effectuer un fuzzing pour les répertoires web, nous pouvons placer le FUZZmot-clé à l'endroit où le répertoire se trouverait dans notre URL, avec :

```
ffuf -w <SNIP> -u http://SERVER_IP:PORT/FUZZ
```

# Fuzzing de page 

Une méthode courante pour identifier un serveur consiste à déterminer son type via les en-têtes de réponse HTTP et à deviner son extension

```
ffuf -w /opt/useful/seclists/Discovery/Web-Content/web-extensions.txt:FUZZ -u http://SERVER_IP:PORT/blog/indexFUZZ
```

```
ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/blog/FUZZ.php
```


# Fuzzing récursif

```
ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ -recursion -recursion-depth 1 -e .php -v
```

## Fuzzing de sous-domaines

L'énumération des sous-domaines consiste à identifier tous les sous-domaines d'un  
domaine donné. Cette opération peut s'avérer utile à diverses fins, comme  
l'identification de cibles potentielles pour une attaque ou simplement à  
des fins organisationnelles.

```
ffuf -w /opt/useful/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u https://FUZZ.inlanefreight.com/
```


## Fuzzing de VHosts

La principale différence entre les VHosts et les sous-domaines est qu'un VHost (hôte virtuel) est essentiellement un « sous-domaine » servi sur le même serveur et ayant la même adresse IP, de sorte qu'une seule IP peut servir deux sites web différents ou plus.
Pour scanner les VHosts, sans ajouter manuellement toute la liste de mots à notre fichier /etc/hosts, nous allons fuzzer les en-têtes HTTP, plus précisément l'en-tête Host:. Pour ce faire, nous pouvons utiliser l'option -H pour spécifier un en-tête et nous utiliserons le mot-clé FUZZ à l'intérieur, comme suit :


```
ffuf -w /opt/useful/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://domain:PORT/ -H 'Host: FUZZ.domain'

ffuf -w /opt/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:31525/ -H 'Host: FUZZ.academy.htb' -fs 986
```

## Fuzzing de paramètres - GET

De la même manière que nous avons fuzzé diverses parties d'un site web, nous utiliserons ffuf pour énumérer des paramètres. Commençons d'abord par fuzzer les requêtes GET, q

```
http://admin.academy.htb:PORT/admin/admin.php?param1=key

ffuf -w /opt/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:31525/admin
[12:40:06:269] [225108:00036f56] [INFO][com.freerdp.channels.drdynvc.client] - [dvcman_load_addin]: Loading Dynamic Vi│/admin.php?FUZZ=key -fs 986

```