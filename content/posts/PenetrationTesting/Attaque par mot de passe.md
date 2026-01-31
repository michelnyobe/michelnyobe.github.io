# Introduction au craquage de mots de passe

# Attaque par force brute
Une brute-forceattaque par force brute consiste à tester toutes les combinaisons possibles de lettres, de chiffres et de symboles jusqu'à trouver le bon mot de passe. 
## Attaque par dictionnaire

# Générer des listes de mots à l'aide de CeWL

Nous pouvons utiliser l'outil CeWL pour analyser les mots potentiels du site web d'une entreprise et les enregistrer dans une liste distincte. Cette liste peut ensuite être combinée aux règles souhaitées afin de créer une liste de mots de passe personnalisée, présentant une plus grande probabilité de contenir le mot de passe correct d'un employé. Nous spécifions certains paramètres, tels que la profondeur d'exploration ( -d), la longueur minimale des mots ( -m), le stockage des mots trouvés en minuscules ( --lowercase), ainsi que le fichier de destination des résultats ( -w).

```
cewl https://www.inlanefreight.com -d 4 -m 6 --lowercase -w inlane.wordlist
```