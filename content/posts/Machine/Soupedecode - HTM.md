# Enumeration
## nmap 

```
PORT      STATE SERVICE       REASON          VERSION
53/tcp    open  domain        syn-ack ttl 126 Simple DNS Plus
88/tcp    open  kerberos-sec  syn-ack ttl 126 Microsoft Windows Kerberos (server time: 2026-01-31 09:46:07Z)
135/tcp   open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 126 Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack ttl 126 Microsoft Windows Active Directory LDAP (Domain: SOUPEDECODE.LOCAL0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds? syn-ack ttl 126
464/tcp   open  kpasswd5?     syn-ack ttl 126
593/tcp   open  ncacn_http    syn-ack ttl 126 Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped    syn-ack ttl 126
3268/tcp  open  ldap          syn-ack ttl 126 Microsoft Windows Active Directory LDAP (Domain: SOUPEDECODE.LOCAL0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped    syn-ack ttl 126
3389/tcp  open  ms-wbt-server syn-ack ttl 126 Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: SOUPEDECODE
|   NetBIOS_Domain_Name: SOUPEDECODE
|   NetBIOS_Computer_Name: DC01
|   DNS_Domain_Name: SOUPEDECODE.LOCAL
|   DNS_Computer_Name: DC01.SOUPEDECODE.LOCAL
|   Product_Version: 10.0.20348
|_  System_Time: 2026-01-31T09:46:56+00:00
|_ssl-date: 2026-01-31T09:47:36+00:00; -2s from scanner time.
| ssl-cert: Subject: commonName=DC01.SOUPEDECODE.LOCAL
| Issuer: commonName=DC01.SOUPEDECODE.LOCAL
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-01-30T09:39:33
| Not valid after:  2026-08-01T09:39:33
| MD5:   6f4e:c1e8:213f:2155:8f12:2f95:e707:a8d0
| SHA-1: d7c3:df44:4c7b:9430:1534:7adc:b999:c58a:9f5a:7b3c
| -----BEGIN CERTIFICATE-----
| MIIC8DCCAdigAwIBAgIQTvulgCBMwqpCJNB34w9sDDANBgkqhkiG9w0BAQsFADAh
| MR8wHQYDVQQDExZEQzAxLlNPVVBFREVDT0RFLkxPQ0FMMB4XDTI2MDEzMDA5Mzkz
| M1oXDTI2MDgwMTA5MzkzM1owITEfMB0GA1UEAxMWREMwMS5TT1VQRURFQ09ERS5M
| T0NBTDCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAMLOW5lCftIBAbnh
| vgZyV0NTv1D2NCS+2YzIDPze1v5M27PoJBKiUyBn959sNE5YnBMbQAubChYFzBEC
| wbwvH4pOpgEjdoteXhAGQdTcPhqyqD/daGEAeFtB0BMhNcWGx67PTf4ewWqSQ56Z
| g9ivRetAVuiuSs2KIwIY6ldqQI2fb8HwtR8WUXm7GU4VNJjZwzTj+dMq61cazu/r
| bBkWgalhw7MWKISoBc2X+fb+bb3aroMFUfVHzc8JlAps7/t+Y40btAGwfY5uqLRf
| dezaKEDkr6XELGkecFppPUJ2O6mDzkNke8Zdeo6LV60C3pGF20Sj88vT09u0TQOF
| POUa71UCAwEAAaMkMCIwEwYDVR0lBAwwCgYIKwYBBQUHAwEwCwYDVR0PBAQDAgQw
| MA0GCSqGSIb3DQEBCwUAA4IBAQAn/A2IRFUgh0+inMRpGm6I4My23SIaIEBkay3q
| yMWgMihrEUSMuIF+XKG4o8iLUzxEQygzUdlvSTKmHwK69JTTkEvMHlgyAvpvHOkw
| NlqV22sIECHoM+BrNiYemtqmxPxeAoHGvQZ7ySSPWO8Dcp6Xo17tDuL96SQCe/Dd
| XhEeZmbik+fz99XjDNgLHk7pWM7cYeU48cFN6hmwmPgDCgZqWpJ3nI9eBCJFtlkc
| riLNV/sJ/Hir0uHD/g8YoZLHZAoXJwcYfdofC/y5frBEkjOOms5zPZP0qoEVtP8f
| aAeOqA3IP4fbWK0LQjv3+qALuBdvZWZrAw1cWsVbe8TCaUlb
|_-----END CERTIFICATE-----
9389/tcp  open  mc-nmf        syn-ack ttl 126 .NET Message Framing
49664/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49670/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49671/tcp open  ncacn_http    syn-ack ttl 126 Microsoft Windows RPC over HTTP 1.0
49714/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
49799/tcp open  msrpc         syn-ack ttl 126 Microsoft Windows RPC
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 60770/tcp): CLEAN (Timeout)
|   Check 2 (port 49560/tcp): CLEAN (Timeout)
|   Check 3 (port 19044/udp): CLEAN (Timeout)
|   Check 4 (port 24349/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2026-01-31T09:46:58
|_  start_date: N/A
|_clock-skew: mean: -2s, deviation: 0s, median: -2s
```