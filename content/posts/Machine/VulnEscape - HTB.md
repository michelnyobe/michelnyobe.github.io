## Enumeration 
### Nmap 

```
PORT     STATE SERVICE       REASON          VERSION
3389/tcp open  ms-wbt-server syn-ack ttl 127 Microsoft Terminal Services
|_ssl-date: 2025-12-28T22:45:46+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=Escape
| Issuer: commonName=Escape
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-12-27T22:39:20
| Not valid after:  2026-06-28T22:39:20
| MD5:   beb0:afd1:f12f:b364:ef0f:9c2a:4328:eba5
| SHA-1: 2fae:461d:59b1:98bf:d626:d2f0:7ed1:2c2e:b026:9681
| -----BEGIN CERTIFICATE-----
| MIIC0DCCAbigAwIBAgIQfxExIe2NtJVCmcjEtXQ5CDANBgkqhkiG9w0BAQsFADAR
| MQ8wDQYDVQQDEwZFc2NhcGUwHhcNMjUxMjI3MjIzOTIwWhcNMjYwNjI4MjIzOTIw
| WjARMQ8wDQYDVQQDEwZFc2NhcGUwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEK
| AoIBAQC4wyMYLvnw0O1MmQxcxuVohVhGAjVylcGJOnqbJabQXvrwZLSQvx//pz0Z
| m9GEJmg82g9RFezAJqXr/qHg58Ml1oyuKfFIJd6NiCFiyc0UHOLj9c46Kv3iOC4I
| p51omXdI4KjaamyG2af0jA1XbYd9XJ+A6yKC5DabhqY3moz47dJjBNwyw8yOYEG8
| Sde5XXEB4dVXGTEYwdttzJPWj26H9nYJGggory7n4Rmjp7N9sDSO4yjhQlAjG57z
| 5fQD95wQRR2WmsYZV+7Pv+cSKILUR/JCD59M2DTKfM8jVx3461Q8qh523i/tMVkj
| VX4ehJZGeGs8E0/GtRShCxhBvi3VAgMBAAGjJDAiMBMGA1UdJQQMMAoGCCsGAQUF
| BwMBMAsGA1UdDwQEAwIEMDANBgkqhkiG9w0BAQsFAAOCAQEAnTDQ6a4WPzA7VlDG
| nKl6z6LM6xnSVpFvLhuiJcJW1L/ZdllRNvQHGzIM/gNG+eAj6KYdMLboa1SFmMRL
| CAavc1xUOg8PcDcm72rJeodO5HL3oyf3UJwoVsUFHS1+Bbm152L7J7iuWkWzoh4+
| kJGiOuIveu1Ram+MXTE06a5bddQ9NFDYKRJe2jD5e/4yN0x9rhcS8JnnHYvHII4A
| Y7oU9vQ5u9TyGQoWbcGHBtYrFXjVdprFnU7YG4c5/xOitAXXizA9kR55ihgLmogj
| sIV0l94GK3PvoXgk/R3nivDOjQ8vBlX1HtWb4wvymO0UN3PapMTiKA/6iXfTsvrI
| uxGfNQ==
|_-----END CERTIFICATE-----
| rdp-ntlm-info: 
|   Target_Name: ESCAPE
|   NetBIOS_Domain_Name: ESCAPE
|   NetBIOS_Computer_Name: ESCAPE
|   DNS_Domain_Name: Escape
|   DNS_Computer_Name: Escape
|   Product_Version: 10.0.19041
|_  System_Time: 2025-12-28T22:45:41+00:00
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 0s, deviation: 0s, median: 0s
```


connection RDP

```
xfreerdp /v:10.129.234.51 /dynamic-resolution -sec-nla
```

KIoskUser0
