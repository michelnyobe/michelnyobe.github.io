---
title: "DNS inverse"
date: 2026-03-06T10:00:00+02:00
draft: true
tags: ["pentest", "Active Directory", "PowerView", "reconnaissance", "CRTP", "red team"]
categories: ["Red Team", "pentester"]
summary: ""
showToc: true
tocOpen: true
---
# Qu'est-ce que le DNS inverse ?

Le DNS inverse (rDNS) convertit les adresses IP en noms de domaine grâce aux enregistrements PTR.
Alors que le DNS classique associe un nom à une adresse IP, le DNS inverse effectue l'opération inverse. Il est utilisé pour la journalisation, la vérification des adresses e-mail, les contrôles de sécurité et le dépannage.

Le DNS inverse utilise des domaines spéciaux qui représentent les adresses IP dans l'ordre inverse

- IPv4 utilise le domaine .in-addr.arpa 
- IPv6 utilise le domaine .ip6.arpa 
L'adresse IP est inversée et chaque partie devient une étiquette dans le nom de domaine, puis un enregistrement PTR fournit le nom d'hôte.

## DNS inversé IPv4 (in-addr.arpa)

Étapes du processus
1. Prenez l'adresse IPv4 (par exemple, 192.0.2.1)
2. Inverser les octets (1.2.0.192)
3. Ajouter le domaine .in-addr.arpa (1.2.0.192.in-addr.arpa)
4. Recherche d'enregistrement PTR à ce nom
5. L'enregistrement PTR contient le nom d'hôte


## Exemple 

Adresse IP : 192.0.2.1
Nom inversé : 1.2.0.192.in-addr.arpa
Record PTR : host1.example.com
Explication : Chaque octet devient une étiquette, dans l'ordre inverse

### DNS inverse IPv6 (ip6.arpa)

Étapes du processus

1. Prenez une adresse IPv6 et développez-la en format complet.
2. Supprimez les deux-points et inversez tous les chiffres hexadécimaux.
3. Insérez des points entre chaque chiffre hexadécimal. 
4. Ajouter le domaine .ip6.arpa
5. Requête pour un enregistrement PTR


# Enumeration 

Analysons et Identifions le serveur DNS 

```
nmap -sU -p 53 --open 10.5.2.0/24
```

Nous allons tout d'abord effectuer un balayage des enregistrements PTR (IP vers FQDN) afin de comprendre les enregistrements inverses existants.

```
for i in {1..254}; do echo -n "10.5.2.$i "; dig +short -x 10.5.2.$i; done >
```


```
dig -x 10.5.2.8 @10.5.2.99
```

```
nslookup 10.5.2.8 10.5.2.99
```


https://networkingtoolbox.net/reference/reverse-dns