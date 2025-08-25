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

# Machine 1 10.0.1.7
# Enumeration
## nmap 

```
PORT      STATE SERVICE   VERSION
22/tcp    open  ssh       OpenSSH 9.2p1 Debian 2+deb12u4 (protocol 2.0)
| ssh-hostkey: 
|   256 a9:d7:82:27:91:88:94:30:81:98:e7:b2:a5:f9:be:c9 (ECDSA)
|_  256 6e:05:ff:63:e8:f6:ef:0e:8e:06:70:9d:89:83:2e:11 (ED25519)
111/tcp   open  rpcbind   2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100003  3,4         2049/tcp   nfs
|   100005  1,2,3      38461/tcp   mountd
|   100005  1,2,3      55568/udp   mountd
|   100021  1,3,4      40590/udp   nlockmgr
|   100021  1,3,4      43107/tcp   nlockmgr
|   100024  1          38937/tcp   status
|   100024  1          41307/udp   status
|_  100227  3           2049/tcp   nfs_acl
2049/tcp  open  nfs_acl   3 (RPC #100227)
3232/tcp  open  mdtp?
6549/tcp  open  apc-6549?
37897/tcp open  mountd    1-3 (RPC #100005)
38461/tcp open  mountd    1-3 (RPC #100005)
38937/tcp open  status    1 (RPC #100024)
43107/tcp open  nlockmgr  1-4 (RPC #100021)
46903/tcp open  mountd    1-3 (RPC #100005)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

Comme indiqué ci-dessus, la cible exécute NFS, ce qui permet une énumération plus poussée.

```
red@start:~$ showmount -e 10.0.1.7
Export list for 10.0.1.7:
/var/backup *

```

Comme indiqué ci-dessus dans la liste d'exportation reçue de la cible, un répertoire  `/var/backup *`  est disponible pour le montage.

```
sudo mount -t nfs 10.0.1.7:/var/backup /tmp/mount
mount | grep /tmp/mount
mount | grep /tmp/mount

```

ls    
etc.zip  mfa.zip  NOTES  zi7zp9Uv