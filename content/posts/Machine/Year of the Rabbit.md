---
title: "Year of the Rabbit"
date: 2026-04-19T10:00:00+02:00
draft: true
tags: ["pentest", "FTP", "challenge"]
categories: ["Red Team", "pentester"]
summary: ""
showToc: true
tocOpen: true
---
# enumeration 
# nmap 


![[Pasted image 20260419210610.png]]


```
PORT   STATE SERVICE REASON         VERSION
21/tcp open  ftp     syn-ack ttl 62 vsftpd 3.0.2
22/tcp open  ssh     syn-ack ttl 62 OpenSSH 6.7p1 Debian 5 (protocol 2.0)
80/tcp open  http    syn-ack ttl 62 Apache httpd 2.4.10 ((Debian))
```

j'ai trois ports ouverts , 21 , 22 et 80 

## ftp 
![[Pasted image 20260419211007.png]]

pas de reponse 

# http 

![[Pasted image 20260419211054.png]]
nous avons un serveur web apache 2 ,  version Apache httpd 2.4.10 
nous allons faire une enumeration de sous domaine  avec gobuster 

```
└─$ gobuster dir -u http://10.130.163.4/ -w /usr/share/seclists/Discovery/Web-Content/big.txt
```

```
Starting gobuster in directory enumeration mode
===============================================================
/.htpasswd            (Status: 403) [Size: 277]
/.htaccess            (Status: 403) [Size: 277]
/assets               (Status: 301) [Size: 313] [--> http://10.130.163.4/assets/]
/server-status        (Status: 403) [Size: 277]
Progress: 20478 / 20479 (100.00%)
===============================================================

```


apres avoir visiter le site  " http://10.130.163.4/assets/ " nous avons trouvé deux fichiers 

![[Pasted image 20260419212847.png]]

dans le fichier css , il y avait un indice 

```
  }
  /* Nice to see someone checking the stylesheets.
     Take a look at the page: /sup3r_s3cr3t_fl4g.php
  */
  div.main_page {
```

http:///10.130.163.4/sup3r_s3cr3t_fl4g.php nous l'avons visité mais elas une redirection , maintenant utilisons burpsuite pour annalyser la redirection 


```
GET /intermediary.php?hidden_directory=/WExYY2Cv-qU HTTP/1.1
Host: 10.130.163.4
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Upgrade-Insecure-Requests: 1
Priority: u=0, i
```
 
 
 qui nous ramene encore vers une autre image 
 
 ![[Pasted image 20260419213331.png]]

http://10.130.163.4/WExYY2Cv-qU/Hot_Babe.png

je télécharge l'image et je commence a analyser


```
 strings Hot_Babe.png 

```

j'ai vue ce mesage 

```
Ot9RrG7h2~24?
Eh, you've earned this. Username for FTP is ftpuser
One of these is the password:
```


j'ai utilisé hydra que cracker le mot de passe 

```
[21][ftp] host: 10.130.163.4   login: ftpuser   password: 5iez1wGXKfPKQ
```


![[Pasted image 20260419214624.png]]

 je me suis connecté en ftp et j'ai trouvé un fichier que j'ai telechargé , qui etait ecrit en  **Brainfuck**  
   j'ai dechiffré et j'ai eu des identifiant 
   
 
 ![[Pasted image 20260419215143.png]]
 
User: eli
Password: DSpDiM1wAEwid


je me suis connecté en SSH 

![[Pasted image 20260419215354.png]] je ne peux pas lire le fichier user.txt 
 seule gwendoline peut lire le fichier  ; essayons de changer d'utilisateur 
  lors de notre connexion nous avons eu un message 
  
```
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.130.163.4' (ED25519) to the list of known hosts.
eli@10.130.163.4's password: 
1 new message
Message from Root to Gwendoline:

"Gwendoline, I am not happy with you. Check our leet s3cr3t hiding place. I've left you a hidden message there"


```

nous avons fait la recherche , pour trouver le fichier  " s3cr3t " et avons trouvé le mot de passe 

```
 locate s3cr3t
/usr/games/s3cr3t
/usr/games/s3cr3t/.th1s_m3ss4ag3_15_f0r_gw3nd0l1n3_0nly!
/var/www/html/sup3r_s3cr3t_fl4g.php
eli@year-of-the-rabbit:/home/gwendoline$ cat /usr/games/s3cr3t/.th1s_m3ss4ag3_15_f0r_gw3nd0l1n3_0nly!
Your password is awful, Gwendoline. 
It should be at least 60 characters long! Not just MniVCQVhQHUNI
Honestly!

Yours sincerely
   -Root
eli@year-of-the-rabbit:/home/gwendoline$ su gwendoline
Password: 


```


![[Pasted image 20260419221447.png]]

nous avons pue lire le flag 

![[Pasted image 20260419221527.png]]

maintenant nous allons elever nos privilege root pour pouvoir lire le fichier  root.txt 
 commencons par lister les commande que l'utilisateur peut utiliser  en etant root 
  " sudo -l "
  