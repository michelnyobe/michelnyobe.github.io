---
title: "Anonforce - THM"
date: 2026-03-06T10:00:00+02:00
draft: true
tags: ["pentest", "Active Directory", "PowerView", "reconnaissance", "CRTP", "red team"]
categories: ["Red Team", "pentester"]
summary: ""
showToc: true
tocOpen: true
---
# enumeration 
## Nmap

21/tcp open  ftp     syn-ack ttl 62 vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| drwxr-xr-x    2 0        0            4096 Aug 11  2019 bin
| drwxr-xr-x    3 0        0            4096 Aug 11  2019 boot
| drwxr-xr-x   17 0        0            3700 Feb 21 14:54 dev
| drwxr-xr-x   85 0        0            4096 Aug 13  2019 etc
| drwxr-xr-x    3 0        0            4096 Aug 11  2019 home
| lrwxrwxrwx    1 0        0              33 Aug 11  2019 initrd.img -> boot/initrd.img-4.4.0-157-generic
| lrwxrwxrwx    1 0        0              33 Aug 11  2019 initrd.img.old -> boot/initrd.img-4.4.0-142-generic
| drwxr-xr-x   19 0        0            4096 Aug 11  2019 lib
| drwxr-xr-x    2 0        0            4096 Aug 11  2019 lib64
| drwx------    2 0        0           16384 Aug 11  2019 lost+found
| drwxr-xr-x    4 0        0            4096 Aug 11  2019 media
| drwxr-xr-x    2 0        0            4096 Feb 26  2019 mnt
| drwxrwxrwx    2 1000     1000         4096 Aug 11  2019 notread [NSE: writeable]
| drwxr-xr-x    2 0        0            4096 Aug 11  2019 opt
| dr-xr-xr-x  102 0        0               0 Feb 21 14:54 proc
| drwx------    3 0        0            4096 Aug 11  2019 root
| drwxr-xr-x   18 0        0             540 Feb 21 14:54 run
| drwxr-xr-x    2 0        0           12288 Aug 11  2019 sbin
| drwxr-xr-x    3 0        0            4096 Aug 11  2019 srv
| dr-xr-xr-x   13 0        0               0 Feb 21 14:54 sys
| drwxrwxrwt    9 0        0            4096 Feb 21 14:54 tmp [NSE: writeable]
| drwxr-xr-x   10 0        0            4096 Aug 11  2019 usr
| drwxr-xr-x   11 0        0            4096 Aug 11  2019 var
| lrwxrwxrwx    1 0        0              30 Aug 11  2019 vmlinuz -> boot/vmlinuz-4.4.0-157-generic
|_lrwxrwxrwx    1 0        0              30 Aug 11  2019 vmlinuz.old -> boot/vmlinuz-4.4.0-142-generic
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:192.168.192.83
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     syn-ack ttl 62 OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 8a:f9:48:3e:11:a1:aa:fc:b7:86:71:d0:2a:f6:24:e7 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDkGQ8G5TDLFJY+zMp5dEj6XUwoH7ojGBjGkOmAf6d9PuIsf4DPFJQmoCA/eiSZpIwfQ14hVhXJHTclmcCd+2OeriuLXq0fEn+aHTo5X82KADkJibmel86qS7ToCzcaROnUkJU17mY3MuyTbfxuqmSvTv/7NI0zRW+cJ+cqwmeSZyhLnOHZ9GT5Y3Lbpvt2w0ktQ128POyaO4GrGA0EERWstIxExpqLaLsqjQPE/hBnIgZXZjd6EL1gn1/CSQnJVdLesIWMcvT5qnm9dZn/ysvysdHHaHylCSKIx5Qu9LtsitssoglpDlhXu5kr2do6ncWMAdTW75asBh+VE+QVX3vV
|   256 73:5d:de:9a:88:6e:64:7a:e1:87:ec:65:ae:11:93:e3 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBAq1VuleOFZpJb73D/25H1l0wp9Cs/SGwWIjwtGW0/2/20+xMsac5E8rACtXtLaAuL3Dk/IRSrORuEfU11R0H3A=
|   256 56:f9:9f:24:f1:52:fc:16:b7:7b:a3:e2:4f:17:b4:ea (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIiId/YCdJZgD4/DG314U2CpAu8Y13DAx7AQ+JX+3zVc
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel


 gpg2john private.asc > hash.txt
john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
gpg --output decrypted_backup --decrypt backup.pgp
gpg --import --pinentry-mode loopback private.asc 