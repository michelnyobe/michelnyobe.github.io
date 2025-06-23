---
title: "Enumeration IMAP - POP3"
date: 2025-06-21
draft: false
tags: ["Enumeration", "IMAP" , "POP3"]
categories: ["Enumeration"]
summary: "IMAP/POP3"
showToc: true
tocOpen: true
---

Par défaut, les ports `110`et `995`sont utilisés pour POP3, et les ports `143`et `993`sont utilisés pour IMAP.
Les ports supérieurs ( `993`et `995`) utilisent TLS/SSL pour chiffrer les communications entre le client et le serveur.
Grâce à Nmap, nous pouvons analyser le serveur à la recherche de ces ports

Pour interagir avec le serveur IMAP ou POP3 via SSL, nous pouvons utiliser `openssl`, ainsi que `ncat`. Les commandes correspondantes sont les suivantes :
```bash
openssl s_client -connect 10.129.14.128:pop3s
```

```sh
nmap -p 143,993 --script imap-ntlm-info 10.0.0.3
```

```
msf6 auxiliary(scanner/pop3/pop3_version)
```


#### Commandes IMAP


`1 LOGIN username password`   Connexion de l'utilisateur.
`1 LIST "" *`     Répertorie tous les répertoires.
`1 CREATE "INBOX"` Crée une boîte aux lettres avec un nom spécifié.
`1 DELETE "INBOX"` Supprime une boîte aux lettres.
`1 RENAME "ToRead" "Important"` Renomme une boîte aux lettres.
`1 LSUB "" *` Renvoie un sous-ensemble de noms de l'ensemble de noms que l'utilisateur a déclaré comme étant `active`ou `subscribed`.
`1 SELECT INBOX` Sélectionne une boîte aux lettres afin que les messages qu'elle contient soient accessibles.
`1 UNSELECT INBOX` Quitte la boîte aux lettres sélectionnée.
`1 FETCH <ID> all` Récupère les données associées à un message dans la boîte aux lettres.
`1 CLOSE`Supprime tous les messages avec l' `Deleted`indicateur défini
`1 LOGOUT`Ferme la connexion avec le serveur IMAP.

#### Commandes POP3



`USER username` Identifie l'utilisateur.
`PASS password` Authentification de l'utilisateur à l'aide de son mot de passe.
`STAT` Demande le nombre d'e-mails enregistrés auprès du serveur.
`LIST` Demande au serveur le nombre et la taille de tous les e-mails.
`RETR id` Demande au serveur de livrer l'e-mail demandé par ID.
`DELE id` Demande au serveur de supprimer l'e-mail demandé par ID.
`CAPA`Demande au serveur d'afficher les capacités du serveur.
`RSET`Demande au serveur de réinitialiser les informations transmises.
`QUIT` Ferme la connexion avec le serveur POP3.


https://donsutherland.org/crib/imap
https://www.atmail.com/blog/imap-101-manual-imap-sessions/