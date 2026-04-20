# Installation du lab Active Directory 
## Creation des VM et preparation  de l'infrastructure 
 ### DC01 
 ip :  192.168.17.136
 system : windows server 2019
nom du domaine : nyom.local

![[Pasted image 20260410092015.png]]

## Creation des roles ACtive directory

![[Pasted image 20260410092613.png]]
##  Creation des utilisateurs 
nous avons creer 20 utilisateurs 
## OU

## add 

![[Pasted image 20260410094759.png]]

![[Pasted image 20260410094825.png]]


## Enumeration AD

![[Pasted image 20260410105303.png]]

```
PORT      STATE SERVICE       REASON          VERSION
53/tcp    open  domain        syn-ack ttl 128 Simple DNS Plus
88/tcp    open  kerberos-sec  syn-ack ttl 128 Microsoft Windows Kerberos (server time: 2026-04-10 08:48:31Z)
135/tcp   open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 128 Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack ttl 128 Microsoft Windows Active Directory LDAP (Domain: nyoma.local0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds? syn-ack ttl 128
464/tcp   open  kpasswd5?     syn-ack ttl 128
593/tcp   open  ncacn_http    syn-ack ttl 128 Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped    syn-ack ttl 128
3268/tcp  open  ldap          syn-ack ttl 128 Microsoft Windows Active Directory LDAP (Domain: nyoma.local0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped    syn-ack ttl 128
3389/tcp  open  ms-wbt-server syn-ack ttl 128 Microsoft Terminal Services
| ssl-cert: Subject: commonName=DC01.nyoma.local
| Issuer: commonName=DC01.nyoma.local
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-01-28T18:25:20
| Not valid after:  2026-07-30T18:25:20
| MD5:   2c2b:1e0c:36e6:d8de:d745:a6a9:5c6e:27a1
| SHA-1: e1fe:cdb0:a017:979d:41cb:b54a:8631:585a:ada8:539d
| -----BEGIN CERTIFICATE-----
| MIIC5DCCAcygAwIBAgIQWqkwZyg4GKBMLpqewwiuHzANBgkqhkiG9w0BAQsFADAb
| MRkwFwYDVQQDExBEQzAxLm55b21hLmxvY2FsMB4XDTI2MDEyODE4MjUyMFoXDTI2
| MDczMDE4MjUyMFowGzEZMBcGA1UEAxMQREMwMS5ueW9tYS5sb2NhbDCCASIwDQYJ
| KoZIhvcNAQEBBQADggEPADCCAQoCggEBALUaWl3/3aL9SD50lKCaDKBCTbWYPrEj
| TAiEDNR2MJy1kE/Yy8UDan2FP+O8ThgocCYioXZODNINQuTq7mWzO7HtyRTYagRq
| 2YeXfO/rdkPuSe696N/n9y8KwFoBcwYd1iACogIRr2zGDKhMvXPJjaqt4LsRgCPQ
| UFenrbh5EMnsgMEQzXZeTufkBGFn60dewdhh9dHWXnJnl1LpOB5YY82RZoQLAFqi
| Md4/tpWxTxdjOMkjt4DOOQcML6NcjWK5BstXk87PE/sDpZGsoYbs5CweS0fbNwG2
| 9NIlTeeUhaeuDQxNpzznPiCTvS9fGOzpbO2I4ZUwpmBePWnv7gUwwOECAwEAAaMk
| MCIwEwYDVR0lBAwwCgYIKwYBBQUHAwEwCwYDVR0PBAQDAgQwMA0GCSqGSIb3DQEB
| CwUAA4IBAQCgR1WmKEJaLIhCCWoDT99ab3qcwPMqR6/CaJ2SEWgHmA46JsF9Jrwi
| zMhynyGLgP+6wM8egv7186CMovUKkA+uRZWR8swBZ8ab+0dSMls37u+XcE78s/9g
| gGYJSSYd6dkjvgBskSk09FEoyeQRFAU8tBbwpNeWQzDq8XPIM8Oet0q5HS6JjaIV
| ab/aBqsv7xxvAGYltUJmWfE41Mjmq+XjWbxD/3gzk+6FRKcY2gnzJadlTTIqJHNe
| h6kY3NL+OFOfQB15yeHFBr8sqDXoBqZBSHz64HBVuVdxAoPqCTj68tk33Qswi0tc
| ZzeCTquxIiC8puxRBn8nKPGQDipbLrrd
|_-----END CERTIFICATE-----
| rdp-ntlm-info: 
|   Target_Name: NYOMA
|   NetBIOS_Domain_Name: NYOMA
|   NetBIOS_Computer_Name: DC01
|   DNS_Domain_Name: nyoma.local
|   DNS_Computer_Name: DC01.nyoma.local
|   DNS_Tree_Name: nyoma.local
|   Product_Version: 10.0.17763
|_  System_Time: 2026-04-10T08:49:19+00:00
|_ssl-date: 2026-04-10T08:49:59+00:00; 0s from scanner time.
5985/tcp  open  http          syn-ack ttl 128 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        syn-ack ttl 128 .NET Message Framing
49668/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49670/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49671/tcp open  ncacn_http    syn-ack ttl 128 Microsoft Windows RPC over HTTP 1.0
49673/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49674/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49684/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
50092/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
MAC Address: 00:0C:29:7C:BC:93 (VMware)
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| nbstat: NetBIOS name: DC01, NetBIOS user: <unknown>, NetBIOS MAC: 00:0c:29:7c:bc:93 (VMware)
| Names:
|   DC01<20>             Flags: <unique><active>
|   NYOMA<1c>            Flags: <group><active>
|   DC01<00>             Flags: <unique><active>
|   NYOMA<00>            Flags: <group><active>
|   NYOMA<1b>            Flags: <unique><active>
| Statistics:
|   00:0c:29:7c:bc:93:00:00:00:00:00:00:00:00:00:00:00
|   00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00
|_  00:00:00:00:00:00:00:00:00:00:00:00:00:00
| smb2-time: 
|   date: 2026-04-10T08:49:19
|_  start_date: N/A
|_clock-skew: mean: 0s, deviation: 0s, median: 0s
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 41481/tcp): CLEAN (Timeout)
|   Check 2 (port 46587/tcp): CLEAN (Timeout)
|   Check 3 (port 53334/udp): CLEAN (Timeout)
|   Check 4 (port 64454/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 10:49
Completed NSE at 10:49, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 10:49
Completed NSE at 10:49, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 10:49
Completed NSE at 10:49, 0.00s elapsed
Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 195.90 seconds
           Raw packets sent: 131125 (5.769MB) | Rcvd: 97 (4.252KB)

```