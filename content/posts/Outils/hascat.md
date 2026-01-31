---
title: "hascat"
date: 2025-05-07T15:00:00+02:00
draft: false
tags: ["Outils", "Pentester"]
categories: ["Outils"]
summary: "hascat"
showToc: true
tocOpen: true
---
Hashcat est un outil de craquage de mots de passe réputé pour Linux, Windows et macOS. Logiciel propriétaire de 2009 à 2015, il est désormais libre de droits.
https://hashcat.net/hashcat/

```
hashcat -a 0 -m 0 <hashes> [wordlist, rule, mask, ...]
```

# Types de hachage

Hashcat prend en charge des centaines de types de hachage différents, chacun se voyant attribuer un identifiant. Une liste des identifiants associés peut être générée en exécutant la commande suivante

```
hashcat --help
```

https://hashcat.net/wiki/doku.php?id=example_hashes
https://github.com/psypanda/hashID

```
hashid -m '$1$FNr44XZC$wQxY6HHLrgrGX0e1195k.1'
```

# Modes d'attaque
## Attaque par dictionnaire

 est, comme son nom l'indique, une attaque par dictionnaire. L'utilisateur fournit des hachages de mots de passe et une liste de mots en entrée, et Hashcat teste chaque mot de la liste comme un mot de passe potentiel jusqu'à ce que le bon soit trouvé ou que la liste soit épuisée.

```
hashcat -a 0 -m 0 e3e3ec5831ad5e7288241960e5d4fdb8 /usr/share/wordlists/rockyou.txt
```

## Attaque par masque 

n type d'attaque par force brute où l'espace des clés est explicitement défini par l'utilisateur. Par exemple, si l'on sait qu'un mot de passe comporte huit caractères, plutôt que de tester toutes les combinaisons possibles, on peut définir un masque qui teste les combinaisons de six lettres suivies de deux chiffres.

```
hashcat -a 3 -m 0 1e293d6912d074c0fd15844d803400dd '?u?l?l?l?l?d?s'
```