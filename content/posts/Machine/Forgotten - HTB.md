# Enumeration
## Nmap

nmap -sV -sC 10.129.48.163 -vv -p- --min-rate 1000 -Pn

```
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 28:c7:f1:96:f9:53:64:11:f8:70:55:68:0b:e5:3c:22 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBMIbLmW6I3vlf8QRrAaFLhH3Ao7CFIvqPPmQG0Z14i0SlPfX9IZobRkjLOB0ncKb5oQ/0SXLnU60rnUe+7Xe6BU=
|   256 02:43:d2:ba:4e:87:de:77:72:ce:5a:fa:86:5c:0d:f4 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICGL/2c6HVh+6F9RbNsZpoYJ2jv4C8SGqtskv0GGuU2P
80/tcp open  http    syn-ack ttl 62 Apache httpd 2.4.56
|_http-title: 403 Forbidden
| http-methods: 
|_  Supported Methods: GET POST OPTIONS HEAD
|_http-server-header: Apache/2.4.56 (Debian)
Service Info: Host: 172.17.0.2; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

## Web

gobuster dir -u http://10.129.48.163 -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt

/survey               
