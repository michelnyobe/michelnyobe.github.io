# Tunneling SSH

Le tunneling SSH peut être utilisé de différentes manières pour rediriger des ports via une connexion SSH , selon la situation. Pour expliquer chaque cas, imaginons que nous ayons pris le contrôle de la machine PC-1 (sans accès administrateur) et que nous souhaitions l'utiliser comme pivot pour accéder à un port d'une autre machine auquel nous ne pouvons pas nous connecter directement. Nous allons créer un tunnel depuis la machine PC-1, agissant comme client SSH , vers le PC de l'attaquant, qui agira comme serveur SSH . En effet, les machines Windows disposent souvent d'un client SSH , mais la plupart du temps, aucun serveur SSH n'est disponible.
![SSh tunneling](/images/ssh_tunneling.png)

Étant donné que nous allons établir une connexion avec la machine de notre attaquant, nous voudrons créer un utilisateur sans accès à aucune console pour le tunneling et définir un mot de passe à utiliser pour créer les tunnels :

```shell-session
useradd tunneluser -m -d /home/tunneluser -s /bin/true
passwd tunneluser
```

