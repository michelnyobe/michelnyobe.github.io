## Qu'est-ce qu'une attaque Pass the Hash ?
L'attaque « Pass the Hash » est répandue dans les environnements Windows. Elle exploite le mécanisme d'authentification du système d'exploitation Windows.

Pour éviter de stocker les mots de passe en clair, Windows conserve les hachages NTLM des mots de passe des utilisateurs. Lors du processus d'authentification, Windows compare ces hachages plutôt que les mots de passe en clair. L'attaque « Pass the Hash » exploite ce mécanisme.

Au lieu de voler et d'utiliser des mots de passe en clair, les pirates se concentrent sur l'obtention et l'utilisation des hachages pour s'authentifier sur diverses ressources réseau. L'attaque « Pass the Hash » est furtive et très efficace.


Lors d'une attaque par « pass the Hash », un attaquant commence par accéder au hachage d'un utilisateur, souvent en vidant le contenu de la base de données SAM du système ou de la mémoire, à l'aide d'outils comme Mimikatz.