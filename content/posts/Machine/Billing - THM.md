---
title: "Billing"
date: 2026-03-06T10:00:00+02:00
draft: true
tags: ["pentest", "Active Directory", "PowerView", "reconnaissance", "CRTP", "red team"]
categories: ["Red Team", "pentester"]
summary: ""
showToc: true
tocOpen: true
---
# Enumeration
## Nmap

22/tcp   open  ssh      syn-ack ttl 62 OpenSSH 9.2p1 Debian 2+deb12u6 (protocol 2.0)
| ssh-hostkey: 
|   256 75:14:5b:7c:79:88:76:d3:e4:02:ed:32:d4:8c:8b:5a (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBJhZXSr4birosroF8RFVvAilj25TwBqb3Vv8M6O+VH21qwfsLxG/n//F5gpP0M7oBQ94Uq4JC/Rp/xh4irbTyd4=
|   256 82:98:b4:a9:3d:5e:fb:cb:26:22:a6:81:e7:72:4e:16 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIHj3pJyTjjiY1DaYUiPiQTCjW1KDTFetV+jRYtf1xbKb
80/tcp   open  http     syn-ack ttl 62 Apache httpd 2.4.62 ((Debian))
| http-title:             MagnusBilling        
|_Requested resource was http://10.81.184.148/mbilling/
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| http-robots.txt: 1 disallowed entry 
|_/mbilling/
|_http-server-header: Apache/2.4.62 (Debian)
3306/tcp open  mysql    syn-ack ttl 62 MariaDB 10.3.23 or earlier (unauthorized)
5038/tcp open  asterisk syn-ack ttl 62 Asterisk Call Manager 2.10.6
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
