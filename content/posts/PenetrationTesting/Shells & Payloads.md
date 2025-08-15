```shell-session
python -c 'import pty; pty.spawn("/bin/sh")' 
```

# Génération de Shell interactifs
## Qu'est-ce qu'un Web Shell ?
https://attack.mitre.org/techniques/T1505/003/
Les fonctions d'exécution système en PHP , telles que shell_exec(), exec(), system()et passthru(), peuvent être utilisées abusivement pour obtenir l'exécution de commandes.