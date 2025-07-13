---
title: "DNS"
date: 2025-07-20T20:00:00+02:00
draft: false
tags: ["resources", "DNS", "autoapprentissage"]
categories: ["resources"]
summary: "DNS"
showToc: true
tocOpen: true
---

## Comment fonctionne le DNS
1. Lorsque vous saisissez le nom de domaine, votre ordinateur vérifie d'abord sa mémoire (cache) pour voir s'il se souvient de l'adresse IP d'une visite précédente. Dans le cas contraire, il fait appel à un résolveur DNS, généralement fourni par votre fournisseur d'accès à Internet (FAI).
2. Le résolveur dispose également d'un cache. S'il n'y trouve pas l'adresse IP, il entame un parcours dans la hiérarchie DNS. Il commence par interroger un serveur de noms racine, qui agit comme le bibliothécaire d'Internet.
3. Le serveur racine ne connaît pas l'adresse exacte, mais il sait qui la connaît : le serveur de noms de domaine de premier niveau (TLD) responsable de l'extension du domaine (par exemple, .com, .org). Il oriente le résolveur dans la bonne direction.


|`A`|Enregistrement d'adresse|Mappe un nom d'hôte à son adresse IPv4.|`www.example.com.`DANS UN`192.0.2.1`|
|`AAAA`|Enregistrement d'adresse IPv6|Mappe un nom d'hôte à son adresse IPv6.|`www.example.com.`EN AAAA`2001:db8:85a3::8a2e:370:7334`|
|`CNAME`|Enregistrement de nom canonique|Crée un alias pour un nom d'hôte, le pointant vers un autre nom d'hôte.|`blog.example.com.`EN CNAME`webserver.example.net.`|
|`MX`|Enregistrement d'échange de courrier|Spécifie le(s) serveur(s) de messagerie chargé(s) de gérer les e-mails pour le domaine.|`example.com.`EN MX 10`mail.example.com.`|
|`NS`|Enregistrement du serveur de noms|Délégue une zone DNS à un serveur de noms faisant autorité spécifique.|`example.com.`EN N.-É.`ns1.example.com.`|
|`TXT`|Enregistrement de texte|Stocke des informations textuelles arbitraires, souvent utilisées pour la vérification de domaine ou les politiques de sécurité.|`example.com.`EN TXT `"v=spf1 mx -all"`(enregistrement SPF)|
|`SOA`|Début de l'enregistrement d'autorité|Spécifie les informations administratives sur une zone DNS, y compris le serveur de noms principal, l'e-mail de la personne responsable et d'autres paramètres.|`example.com.`À SOA`ns1.example.com. admin.example.com. 2024060301 10800 3600 604800 86400`|
|`SRV`|Dossier de service|Définit le nom d'hôte et le numéro de port pour des services spécifiques.|`_sip._udp.example.com.`DANS SRV 10 5 5060`sipserver.example.com.`|
|`PTR`|Enregistrement du pointeur|Utilisé pour les recherches DNS inversées, mappant une adresse IP à un nom d'hôte.|`1.2.0.192.in-addr.arpa.`EN PTR`www.example.com.`|

## Outils DNS
**dig** : Outil de recherche DNS polyvalent qui prend en charge différents types de requêtes (A, MX, NS, TXT, etc.) et une sortie détaillée.
**nslookup** : Outil de recherche DNS plus simple, principalement pour les enregistrements A, AAAA et MX.
**dnsenum** : Outil d'énumération DNS automatisé, attaques par dictionnaire, force brute, transferts de zone (si autorisé).
**dnsrecon** : Combine plusieurs techniques de reconnaissance DNS et prend en charge divers formats de sortie.
**theHarvester** : Outil OSINT qui collecte des informations provenant de diverses sources, notamment les enregistrements DNS (adresses e-mail).

## Dig
La `dig`commande ( `Domain Information Groper`) est un utilitaire polyvalent et puissant permettant d'interroger les serveurs DNS et de récupérer différents types d'enregistrements DNS. Sa flexibilité et ses résultats détaillés et personnalisables en font un choix incontournable.

## Sous-domain

### Enumeration de sous-domains 

il s'agit du processus d'identification et de recensement systématiques de ces sous-domaines. Du point de vue DNS, les sous-domaines sont généralement représentés par des enregistrements `A`(ou, `AAAA`pour IPv6, des enregistrements), qui associent le nom du sous-domaine à son adresse IP correspondante.

#### Énumération des sous-domaines actifs
Une technique active plus courante `brute-force enumeration`consiste à tester systématiquement une liste de noms de sous-domaines potentiels par rapport au domaine cible. Des outils comme `dnsenum`, `ffuf`et `gobuster`peuvent automatiser ce processus en utilisant des listes de mots de noms de sous-domaines courants ou des listes personnalisées basées sur des modèles spécifiques.

####  Énumération passive des sous-domaines
Cette méthode s'appuie sur des sources d'informations externes pour découvrir les sous-domaines sans interroger directement les serveurs DNS de la cible. `Certificate Transparency (CT) logs`Les référentiels publics de certificats SSL/TLS constituent une ressource précieuse. Ces certificats incluent souvent une liste de sous-domaines associés dans leur champ « Subject Alternative Name » (SAN), offrant ainsi une mine d'informations sur les cibles potentielles.

#####  Bruteforcement de sous-domaine
il existe plusieurs outils disponibles qui excellent dans l’énumération par force brute :
[dnsénum](https://github.com/fwaeytens/dnsenum)
[fierce](https://github.com/mschwager/fierce)
[dnsrecon](https://github.com/darkoperator/dnsrecon)
[amass](https://github.com/owasp-amass/amass)
[assetfinder](https://github.com/tomnomnom/assetfinder)
[puredns](https://github.com/d3mondev/puredns)

##### DNSEnum

```bash
dnsenum --enum inlanefreight.com -f /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -r
```

#### Transferts de zone DNS
Un transfert de zone DNS consiste essentiellement à copier tous les enregistrements DNS d'une zone (un domaine et ses sous-domaines) d'un serveur de noms à un autre.

Vous pouvez utiliser la `dig`commande pour demander un transfert de zone :

```bash
dig axfr @nsztm1.digi.ninja zonetransfer.me
```

#### Hotes Virtuels 

Une fois le DNS dirigé vers le bon serveur, la configuration du serveur web devient cruciale pour déterminer le traitement des requêtes entrantes.

`Virtual Hosts`( `VHosts`) : Les hôtes virtuels sont des configurations au sein d'un serveur web permettant d'héberger plusieurs sites web ou applications sur un même serveur.
Plusieurs outils sont disponibles pour aider à la découverte des hôtes virtuels :
[gobuster](https://github.com/OJ/gobuster)
[Feroxbuster](https://github.com/epi052/feroxbuster)
[ffuf](https://github.com/ffuf/ffuf)

##### Gobuster

Gobuster est un outil polyvalent couramment utilisé pour le forçage de répertoires et de fichiers, mais il excelle également dans la découverte d'hôtes virtuels. Il envoie systématiquement des requêtes HTTP avec différents `Host`en-têtes à une adresse IP cible, puis analyse les réponses pour identifier les hôtes virtuels valides.

```bash
gobuster vhost -u http://<target_IP_address> -w <wordlist_file> --append-domain
```

#### Certificate Transparency Logs

### Banner Grabbing
La première étape consiste à collecter les informations directement depuis le serveur web. Pour ce faire, nous utilisons la `curl`commande avec l' `-I`indicateur (ou `--head`) pour récupérer uniquement les en-têtes HTTP, et non l'intégralité du contenu de la page.
### Nikto
https://web.archive.org/