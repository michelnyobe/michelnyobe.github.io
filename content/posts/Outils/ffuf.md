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