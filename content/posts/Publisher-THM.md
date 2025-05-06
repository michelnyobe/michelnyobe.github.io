---
title: "Publisher - THM"
date: 2025-05-06T10:00:00+02:00
draft: false
tags: ["pentest", "web", "tryhackme", "reconnaissance"]
categories: ["Tryhackme", "Challenge"]
summary: "tryhackme "
cover:
  image: "images/20250422222347.png"
  alt: "THM Publisher"
  
showToc: true
tocOpen: true
---

# Enumeration 

## Nmap 

Nous commençons par exécuter une analyse nmap sur la cible :

![Scan Nmap](/images/20250422222347.png)

Le scan nous montre que le port 22 et 80 sont ouverts.

# Webserver 

Je tombe sur un site web anodin :

![Page web](/images/20250422224600.png)

Après énumération des répertoires, je découvre `/spip` :

![SPIP découvert](/images/20250422223944.png)

![SPIP Interface](/images/20250422225019.png)

> Référence Metasploit utilisée :  
> https://github.com/rapid7/metasploit-framework/pull/17711/files

Après plusieurs heures, j’ai essayé le framework Metasploit :

![Metasploit étape 1](/images/20250423001854.png)

![Metasploit étape 2](/images/20250423001929.png)

# PATH Injection 

AppArmor est un outil qui permet de restreindre un programme avec des règles. Vous pouvez accéder à ces informations par une simple recherche sur Google.

J’ai eu cette indication dans la salle, je suis donc allé examiner les fichiers AppArmor pour mieux comprendre les règles :

![AppArmor Rules](/images/20250423142217.png)

J'ajoute le dossier `/var/tmp`, sur lequel j’ai les droits d’écriture.

Je crée ensuite un fichier binaire appelé `docker` et l'autorise à s'exécuter. Ce fichier contient `/bin/bash -p`. Je souhaite donc utiliser la commande bash en mode privilégié.

Si vous faites attention, lorsque j’imprime la valeur `$PATH`, le shell initial est `ash`, mais après l’opération, il devient `bash`.

Ensuite, je copie mon fichier `docker` dans `/opt/run_container.sh` et j'exécute le binaire **/usr/sbin/run_container** avec le bit SUID… et je suis ROOT !!!

![PrivEsc réussi](/images/20250423143022.png)

![Shell Root](/images/20250423143138.png)
