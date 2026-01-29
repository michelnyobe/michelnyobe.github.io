
L'objectif principal de cette opération Red Team est d'évaluer la sécurité de l'environnement de l'entreprise. Cette mission vise à identifier les vulnérabilités et les erreurs de configuration de l'environnement AD et à formuler des recommandations concrètes pour renforcer la sécurité de l'infrastructure.

VPN Credential

Username: Larass0x04
Password: odzDc#La

my ip : 10.10.200.115/24


Etapes / 
- injection de commande 
- Proxychain
- dump Databases MYSQL.
- 1

# Exploitation du reseaux externe de l'entreprise 
# Enumeration 
# initial access
nmap 
la plage du reseaux  192.168.80.0/24

```
nmap -sn 192.168.80.0/24                                  
Starting Nmap 7.95 ( https://nmap.org ) at 2025-12-07 17:20 CET
Nmap scan report for 192.168.80.10
Host is up (0.030s latency).
Nmap done: 256 IP addresses (1 host up) scanned in 5.08 seconds
```

nmap -sV -sC 192.168.80.10 -p- -vv

```
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 8d:c3:a7:a5:bf:16:51:f2:03:85:a7:37:ee:ae:8d:81 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC/QYfVmhSmPbov28ma0gUwDZPU3qIwuN2OaGckoc/eP/5tblHwLLn6OVLXL/COy+zvA2XDy5BYk9222v0q4du/YGhJmhi6E3vnn72gb9YYi2vilIYksFy1pRURXdCqZjSWLxVXjBx7+stPrFCOpUZ5OjBdOW+61mJbOaDEr17SG7TYGfZ0aSDe8PduFbeIYJflM4Mw+517EcJGj5qfTWxdXZYLzk+0YBD6DXs6JRbDDWPyjabTQJgXrV9XliZPlT67xa5qi89xP1shgx613prGZOPVH5q6XvRAoVxwaXe4+OC3t2V3So7xUQg4klBYKwMPXX1eOW12Jh/wUFUeliGfvVpRsQKrGoBeKuzAGqjyVvSv/e7rOPPsng241MUW4BD2Dz3VTVcvFOqJqhsjjDLRu2LLQyEVK8sDRSgRdgHB4+fZb1tyNjDHnSS+mx5fLJl15gfjPfDbnmHOGPHPylDmV4mnbB3R4UHBmMhJJ24PBzHFzeyd1VCjDEHiGShb8b8=
|   256 9a:b2:73:5a:e5:36:b4:91:d8:8c:f7:4a:d0:15:65:28 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBFcuhdRAURNIUCbK9v9MdTzpeo8GwIbxbtJE8bjvaxM5VtWPogEiXTDoKh3Yf2EelkMb7jZHly13jkI4Bv+c/mY=
|   256 3c:16:a7:6a:b6:33:c5:83:ab:7f:99:60:6a:4c:09:11 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIA8e3jMqD/fzQKhBTIKHNuHwYMWGEpMXJ7odN6C2JiKK
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Cyber WareFare Labs
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

nous avons trouvez deux port 80 et 22 .