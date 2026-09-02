« living off the land » (exploiter les ressources locales) 

# Enumeration acec authentification - Linux
## CrackMapExec

est une puissante suite d'outils pour aider à l'évaluation des environnements AD. 
- enumeration des utilisateurs du domaine 
```
	sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --users
```
- Enumeration des groupes du domaine 
```
	sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --groups
```
- Utilisateurs connectés 
```
	sudo crackmapexec smb 172.16.5.130 -u forend -p Klmcargo2 --loggedon-users
```
-  des partages 
```
	sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --shares
```

## SMBMap

SMBMap est excellent pour énumérer les partages SMB depuis un hôte d'attaque Linux. Il peut être utilisé pour obtenir une liste des partages, des permissions et du contenu des partages si accessible.

```
smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5
```


Liste récursive de tous les répertoires

```
smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5 -R 'Department Shares' --dir-only
```

## rpcclient

rpcclient est un outil pratique créé pour être utilisé avec le protocole Samba et pour fournir des fonctionnalités supplémentaires via MS-RPC. Il peut énumérer, ajouter, modifier et même supprimer des objets d'AD.

```
rpcclient -U "" -N 172.16.5.5
```

Cependant, il y a des comptes que vous remarquerez qui ont le même RID quel que soit l'hôte sur lequel vous vous trouvez. Des comptes comme l'Administrateur intégré d'un domaine auront un RID administrator rid:0x1f4, qui, converti en valeur décimale, équivaut à 500. Le compte Administrateur intégré aura toujours la valeur RID Hex 0x1f4, ou 500. Ce sera toujours le cas

Énumération d'utilisateur par RID avec RPCClient

```
rpcclient $> queryuser 0x457
```

Enumdomusers

```
rpcclient $> enumdomusers
```

L'utiliser de cette manière affichera tous les utilisateurs du domaine par nom et RID. Notre énumération peut aller dans les moindres détails en utilisant rpcclient.

## La boîte à outils Impacket

Impacket est une boîte à outils polyvalente qui nous offre de nombreuses manières différentes d'énumérer, d'interagir avec et d'exploiter les protocoles Windows et de trouver les informations dont nous avons besoin en utilisant Python.

Psexec.py
L'un des outils les plus utiles de la suite Impacket est psexec.py. Psexec.py est un clone de l'exécutable psexec de Sysinternals, mais fonctionne légèrement différemment de l'original. 
Pour se connecter à un hôte avec psexec.py, nous avons besoin des identifiants d'un utilisateur ayant des privilèges d'administrateur local.

```
psexec.py inlanefreight.local/wley:'transporter@4'@172.16.5.125 
```

wmiexec.py
Wmiexec.py utilise un shell semi-interactif où les commandes sont exécutées via Windows Management Instrumentation. Il ne dépose aucun fichier ou exécutable sur l'hôte cible et génère moins de journaux que d'autres modules.

```
wmiexec.py inlanefreight.local/wley:'transporter@4'@172.16.5.5  
```

Notez que cet environnement de shell n'est pas entièrement interactif, donc chaque commande émise exécutera un nouveau cmd.exe depuis WMI et exécutera votre commande.

Windapsearch
Windapsearch est un autre script Python pratique que nous pouvons utiliser pour énumérer les utilisateurs, les groupes et les ordinateurs d'un domaine Windows en utilisant des requêtes LDAP.

```
python3 windapsearch.py --dc-ip 172.16.5.5 -u forend@inlanefreight.local -p Klmcargo2 --da
```

Exécution de BloodHound.py

```
sudo bloodhound-python -u 'forend' -p 'Klmcargo2' -ns 172.16.5.5 -d inlanefreight.local -c all 
```


Module PowerShell ActiveDirectory

Le module PowerShell ActiveDirectory est un groupe de cmdlets PowerShell pour administrer un environnement Active Directory depuis la ligne de commande. Il se compose de 147 cmdlets différents au moment de la rédaction.

```
Import-Module ActiveDirectory
```

PowerView

PowerView est un outil écrit en PowerShell pour nous aider à obtenir une conscience situationnelle (situational awareness) dans un environnement AD. Tout comme BloodHound, il offre un moyen d'identifier où les utilisateurs sont connectés sur un réseau, d'énumérer les informations du domaine telles que les utilisateurs, les ordinateurs, les groupes, les ACL, les approbations, de rechercher des partages de fichiers et des mots de passe, d'effectuer du Kerberoasting, et plus encore. 


Snaffler
Snaffler est un outil qui peut nous aider à obtenir des identifiants (credentials) ou d'autres données sensibles dans un environnement Active Directory. Snaffler fonctionne en obtenant une liste d'hôtes au sein du domaine, puis en énumérant ces hôtes à la recherche de partages (shares) et de répertoires lisibles. 

```
.\SharpHound.exe -c All --zipfilename ILFREIGHT

 
```