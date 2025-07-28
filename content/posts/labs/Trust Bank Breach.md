In this Red Team exercise, students will simulate a financially motivated Advanced Persistent Threat (APT) attack against "Trust Bank," aiming to compromise the branch manager’s email account and exfiltrate sensitive financial data including internal communications all while maintaining stealth to mimic real-world banking threats like FIN7. The Red Team will go through Initial Access, Privilege Escalation, Lateral Movement & Data Exfiltration.

# Description

To conduct a **targeted cyber operation** against “**Trust Bank**” with the primary goal of **compromising the branch manager’s mailbox** and **exfiltrating sensitive financial data** (e.g., customer records, transaction logs, internal communications). The operation will simulate a **financially motivated Advanced Persistent Threat (APT)** attack, leveraging advanced tradecraft to evade detection while achieving mission objectives.

# Scope of Engagement:

- Target
    - **Primary Objective:** Gain unauthorized mail access to the **branch manager’s account.**
    - **Secondary Objective:** Extract sensitive data (PII, account details, internal memos).
    - **IP Range:** 172.16.100.0/24 (Trust Bank’s internal network segment).
- **Tactics & Techniques (Simulated APT Approach):**
    - **Initial Access:**
        - Exploiting public-facing services.
    - **Privilege Escalation:**
        - Sensitive information disclosure
        - Exploiting misconfigured service permissions.
    - **Lateral Movement:**
        - Weak protocol Implementation
        - SSH Agent Abuse
    - **Data Exfiltration:**
        - Stealthy data compression & exfil via encrypted channels (DNS tunneling, HTTPS).
- **Rules of Engagement (ROE):**
    - **Avoid disruption:** Do not crash systems or trigger incident response unnecessarily.
    - **Legal compliance:** Operate under authorized red team agreements.
- **Deliverables:**
    - **Post-Compromise Report:**
        - Attack timeline & techniques used
        - Security recommendations (patch weak credentials, MFA enforcement).
        - **[IMP] Flag present at the Bank Manager Mailbox**
![[lab1.png]]





# Decouverte du reseaux 
## Enumeration 
### Nmap

Scannons tous les hôtes actifs sur le réseau local **172.16.100.0/24** pour identifier les machines accessibles

```shell
nmap 172.16.100.0/24        
```

```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-25 16:01 CEST
Nmap scan report for 172.16.100.1
Host is up (0.22s latency).
Not shown: 998 filtered tcp ports (no-response)
PORT   STATE SERVICE
53/tcp open  domain
80/tcp open  http

Nmap scan report for 172.16.100.25
Host is up (0.20s latency).
Not shown: 997 closed tcp ports (reset)
PORT    STATE SERVICE
25/tcp  open  smtp
143/tcp open  imap
993/tcp open  imaps

Nmap scan report for 172.16.100.70
Host is up (0.20s latency).
Not shown: 999 closed tcp ports (reset)
PORT     STATE SERVICE
8080/tcp open  http-proxy

Nmap scan report for 172.16.100.98
Host is up (0.18s latency).
Not shown: 994 closed tcp ports (reset)
PORT     STATE SERVICE
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
1433/tcp open  ms-sql-s
5000/tcp open  upnp
5357/tcp open  wsdapi

Nmap scan report for 172.16.100.102
Host is up (0.21s latency).
Not shown: 988 filtered tcp ports (no-response)
PORT     STATE SERVICE
22/tcp   open  ssh
53/tcp   open  domain
88/tcp   open  kerberos-sec
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
389/tcp  open  ldap
445/tcp  open  microsoft-ds
464/tcp  open  kpasswd5
593/tcp  open  http-rpc-epmap
636/tcp  open  ldapssl
3268/tcp open  globalcatLDAP
3269/tcp open  globalcatLDAPssl
```

La commande a permis d’identifier **5 hôtes actifs** sur le réseau **172.16.100.0/24**. Cela signifie que ces machines ont répondu à des requêtes réseau et ont au moins un port ouvert.

- `172.16.100.1`
- `172.16.100.25`
- `172.16.100.70`
- `172.16.100.98`
- `172.16.100.102`

# Machine 172.16.100.98
## Enumeration 

```
PORT     STATE SERVICE       VERSION
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds?
1433/tcp open  ms-sql-s      Microsoft SQL Server 2019 15.00.2000.00; RTM
| ms-sql-info: 
|   172.16.100.98\MSSQLSERVER: 
|     Instance name: MSSQLSERVER
|     Version: 
|       name: Microsoft SQL Server 2019 RTM
|       number: 15.00.2000.00
|       Product: Microsoft SQL Server 2019
|       Service pack level: RTM
|       Post-SP patches applied: false
|     TCP port: 1433
|     Named pipe: \\172.16.100.98\pipe\sql\query
|_    Clustered: false
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Issuer: commonName=SSL_Self_Signed_Fallback
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-07-04T01:28:55
| Not valid after:  2055-07-04T01:28:55
| MD5:   6f59:6c52:3bde:7eb3:03f2:43c5:86d4:4db8
|_SHA-1: ddf4:261b:fdff:f390:48e6:d13e:73f7:3035:5943:231e
|_ssl-date: 2025-07-25T08:52:56+00:00; -5h18m23s from scanner time.
| ms-sql-ntlm-info: 
|   172.16.100.98\MSSQLSERVER: 
|     Target_Name: SQL-SRV
|     NetBIOS_Domain_Name: SQL-SRV
|     NetBIOS_Computer_Name: SQL-SRV
|     DNS_Domain_Name: SQL-Srv
|     DNS_Computer_Name: SQL-Srv
|_    Product_Version: 10.0.20348
5000/tcp open  http          Werkzeug httpd 3.1.3 (Python 3.13.5)
| http-methods: 
|_  Supported Methods: GET HEAD OPTIONS
|_http-server-header: Werkzeug/3.1.3 Python/3.13.5
|_http-title: SRC BANK
5357/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Service Unavailable
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
|_clock-skew: mean: -5h18m23s, deviation: 0s, median: -5h18m23s
| smb2-time: 
|   date: 2025-07-25T08:52:49
|_  start_date: N/A
```

Le scan révèle que l’hôte 172.16.100.98 expose plusieurs services critiques Windows, dont SMB et SQL Server 2019, ainsi que deux interfaces HTTP, ce qui en fait une cible potentielle pour une exploitation approfondie.
la liste des ports ouverts : 135, 139, 445, 1433, 5000, 5357

# Serveur Web 

![[Pasted image 20250725161630.png]]