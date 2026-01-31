https://github.com/openwall/john

est un outil de test d'intrusion réputé, utilisé pour casser les mots de passe grâce à diverses attaques, notamment par force brute et par dictionnaire. Ce logiciel libre, initialement développé pour les systèmes UNIX, a été lancé en 1996. Il est devenu un incontournable du secteur de la sécurité grâce à ses nombreuses fonctionnalités.

# Modes de craquage
## single crack mode 

```
john --single passwd
```

## mode liste de mot 

```
john --wordlist=<wordlist_file> <hash_file>
```

## mode incrémental 

Il s'agit d'un mode de craquage de mots de passe puissant, de type force brute, qui génère des mots de passe candidats à partir d'un modèle statistique

https://en.wikipedia.org/wiki/Markov_chain

```
john --incremental <hash_file>
```

https://openwall.info/wiki/john/sample-hashes
# Fichiers de craquage

Il est également possible de déchiffrer des fichiers protégés par mot de passe ou chiffrés avec JtR. Plusieurs "2john"outils fournis avec JtR permettent de traiter les fichiers et de produire des hachages compatibles avec JtR. La syntaxe générale de ces outils est la suivante :

```
<tool> <file_to_crack> > file.hash
```

```
john --format=raw-ripemd --wordlist=rockyou.txt hashes.txt
```