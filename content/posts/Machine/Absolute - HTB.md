## Enumeration 
### Nmap

```
PORT      STATE SERVICE       REASON          VERSION
53/tcp    open  domain        syn-ack ttl 127 Simple DNS Plus
80/tcp    open  http          syn-ack ttl 127 Microsoft IIS httpd 10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-title: Absolute
|_http-server-header: Microsoft-IIS/10.0
88/tcp    open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2026-01-21 06:08:43Z)
135/tcp   open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: absolute.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2026-01-21T06:09:47+00:00; +7h00m00s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc.absolute.htb, DNS:absolute.htb, DNS:absolute
| Issuer: commonName=absolute-DC-CA/domainComponent=absolute
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha1WithRSAEncryption
| Not valid before: 2025-04-23T18:13:50
| Not valid after:  2026-04-23T18:13:50
| MD5:   88cd:2d01:6795:f78f:4df0:d194:ec78:b90d
| SHA-1: 9831:ec1f:3649:814d:ef5d:6133:235c:c738:0420:b081
| -----BEGIN CERTIFICATE-----
| MIIF8jCCBNqgAwIBAgITbgAAAAcGWG4iffRa8QABAAAABzANBgkqhkiG9w0BAQUF
| ADBIMRMwEQYKCZImiZPyLGQBGRYDaHRiMRgwFgYKCZImiZPyLGQBGRYIYWJzb2x1
| dGUxFzAVBgNVBAMTDmFic29sdXRlLURDLUNBMB4XDTI1MDQyMzE4MTM1MFoXDTI2
| MDQyMzE4MTM1MFowADCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAMOE
| scFqn+1CHWKpA1edwK1615x3KaP4yRDQ+1nnAXDqurdJ/6p18fn3OhSjdg23sgML
| n2ZLklrzGRPiXp2KqeGqxjW1G4PP0gs6/pC5Lsk8zsuqhAAgxgsiU1mUXQ8vLySw
| bx81jpP4ChjsnDs35cVFD7//pOlZv0j20TJVOqw3Ip5ojb8eZS26kjQXp6j+kV5k
| jhHKjK1wTYm42pKdDijCZP5NRuqR43S1WewTo0RaL3N3vxuCwXAyxjyR8naAJeuK
| 2aWnYtN4s7wqyi1i+YTyHklh1yAauYgTbLWulTX6HxFx3bZYPfzkLeI5ZY+MfRfy
| wzLHAc0ahN11soQajtECAwEAAaOCAxswggMXMDgGCSsGAQQBgjcVBwQrMCkGISsG
| AQQBgjcVCIe89nGC4acdhPGdA4Gx8jOCz7kJgW4BIQIBbgIBADAyBgNVHSUEKzAp
| BggrBgEFBQcDAgYIKwYBBQUHAwEGCisGAQQBgjcUAgIGBysGAQUCAwUwDgYDVR0P
| AQH/BAQDAgWgMEAGCSsGAQQBgjcVCgQzMDEwCgYIKwYBBQUHAwIwCgYIKwYBBQUH
| AwEwDAYKKwYBBAGCNxQCAjAJBgcrBgEFAgMFMB0GA1UdDgQWBBRU3ws2ulNPo7O+
| rlSksas8uAtFnzAfBgNVHSMEGDAWgBSAhiBP4MNvSvhCZpCLP19QO92gNzCByAYD
| VR0fBIHAMIG9MIG6oIG3oIG0hoGxbGRhcDovLy9DTj1hYnNvbHV0ZS1EQy1DQSxD
| Tj1kYyxDTj1DRFAsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049U2Vydmlj
| ZXMsQ049Q29uZmlndXJhdGlvbixEQz1hYnNvbHV0ZSxEQz1odGI/Y2VydGlmaWNh
| dGVSZXZvY2F0aW9uTGlzdD9iYXNlP29iamVjdENsYXNzPWNSTERpc3RyaWJ1dGlv
| blBvaW50MIHBBggrBgEFBQcBAQSBtDCBsTCBrgYIKwYBBQUHMAKGgaFsZGFwOi8v
| L0NOPWFic29sdXRlLURDLUNBLENOPUFJQSxDTj1QdWJsaWMlMjBLZXklMjBTZXJ2
| aWNlcyxDTj1TZXJ2aWNlcyxDTj1Db25maWd1cmF0aW9uLERDPWFic29sdXRlLERD
| PWh0Yj9jQUNlcnRpZmljYXRlP2Jhc2U/b2JqZWN0Q2xhc3M9Y2VydGlmaWNhdGlv
| bkF1dGhvcml0eTA1BgNVHREBAf8EKzApgg9kYy5hYnNvbHV0ZS5odGKCDGFic29s
| dXRlLmh0YoIIYWJzb2x1dGUwTwYJKwYBBAGCNxkCBEIwQKA+BgorBgEEAYI3GQIB
| oDAELlMtMS01LTIxLTQwNzgzODIyMzctMTQ5MjE4MjgxNy0yNTY4MTI3MjA5LTEw
| MDAwDQYJKoZIhvcNAQEFBQADggEBACZFW9xR+dm7QH/lpvMy7hRQaksrXXzdA37l
| UNpQaHqIbSk7p48+UpkfuufXcGXLKh5Uxw1XARPRZPcQuvI5xynj6rVTm0ImsGrU
| JLijIepb89N7iglFw7E+JlsIZNN/Maw6xrryU0XhhOr3B6FX9odmYiaeh2CKClbi
| 5hpopmXzR6fBKsLOLmq/EuY7xufiv1gTJrnAD7B3mstSvfpdjcyR0aPuBIG/Jq4n
| PCHN4sCne9IqL13zelIE+m47urN6/4NoablwTH766MwTOkRiKw+HqF/QskrC51cG
| 2eGyc9b6MoO75oc7jVGm9EFq97DGLi3h6J0xb4hfMkgJlc4gLfs=
|_-----END CERTIFICATE-----
445/tcp   open  microsoft-ds? syn-ack ttl 127
464/tcp   open  kpasswd5?     syn-ack ttl 127
593/tcp   open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: absolute.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc.absolute.htb, DNS:absolute.htb, DNS:absolute
| Issuer: commonName=absolute-DC-CA/domainComponent=absolute
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha1WithRSAEncryption
| Not valid before: 2025-04-23T18:13:50
| Not valid after:  2026-04-23T18:13:50
| MD5:   88cd:2d01:6795:f78f:4df0:d194:ec78:b90d
| SHA-1: 9831:ec1f:3649:814d:ef5d:6133:235c:c738:0420:b081
| -----BEGIN CERTIFICATE-----
| MIIF8jCCBNqgAwIBAgITbgAAAAcGWG4iffRa8QABAAAABzANBgkqhkiG9w0BAQUF
| ADBIMRMwEQYKCZImiZPyLGQBGRYDaHRiMRgwFgYKCZImiZPyLGQBGRYIYWJzb2x1
| dGUxFzAVBgNVBAMTDmFic29sdXRlLURDLUNBMB4XDTI1MDQyMzE4MTM1MFoXDTI2
| MDQyMzE4MTM1MFowADCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAMOE
| scFqn+1CHWKpA1edwK1615x3KaP4yRDQ+1nnAXDqurdJ/6p18fn3OhSjdg23sgML
| n2ZLklrzGRPiXp2KqeGqxjW1G4PP0gs6/pC5Lsk8zsuqhAAgxgsiU1mUXQ8vLySw
| bx81jpP4ChjsnDs35cVFD7//pOlZv0j20TJVOqw3Ip5ojb8eZS26kjQXp6j+kV5k
| jhHKjK1wTYm42pKdDijCZP5NRuqR43S1WewTo0RaL3N3vxuCwXAyxjyR8naAJeuK
| 2aWnYtN4s7wqyi1i+YTyHklh1yAauYgTbLWulTX6HxFx3bZYPfzkLeI5ZY+MfRfy
| wzLHAc0ahN11soQajtECAwEAAaOCAxswggMXMDgGCSsGAQQBgjcVBwQrMCkGISsG
| AQQBgjcVCIe89nGC4acdhPGdA4Gx8jOCz7kJgW4BIQIBbgIBADAyBgNVHSUEKzAp
| BggrBgEFBQcDAgYIKwYBBQUHAwEGCisGAQQBgjcUAgIGBysGAQUCAwUwDgYDVR0P
| AQH/BAQDAgWgMEAGCSsGAQQBgjcVCgQzMDEwCgYIKwYBBQUHAwIwCgYIKwYBBQUH
| AwEwDAYKKwYBBAGCNxQCAjAJBgcrBgEFAgMFMB0GA1UdDgQWBBRU3ws2ulNPo7O+
| rlSksas8uAtFnzAfBgNVHSMEGDAWgBSAhiBP4MNvSvhCZpCLP19QO92gNzCByAYD
| VR0fBIHAMIG9MIG6oIG3oIG0hoGxbGRhcDovLy9DTj1hYnNvbHV0ZS1EQy1DQSxD
| Tj1kYyxDTj1DRFAsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049U2Vydmlj
| ZXMsQ049Q29uZmlndXJhdGlvbixEQz1hYnNvbHV0ZSxEQz1odGI/Y2VydGlmaWNh
| dGVSZXZvY2F0aW9uTGlzdD9iYXNlP29iamVjdENsYXNzPWNSTERpc3RyaWJ1dGlv
| blBvaW50MIHBBggrBgEFBQcBAQSBtDCBsTCBrgYIKwYBBQUHMAKGgaFsZGFwOi8v
| L0NOPWFic29sdXRlLURDLUNBLENOPUFJQSxDTj1QdWJsaWMlMjBLZXklMjBTZXJ2
| aWNlcyxDTj1TZXJ2aWNlcyxDTj1Db25maWd1cmF0aW9uLERDPWFic29sdXRlLERD
| PWh0Yj9jQUNlcnRpZmljYXRlP2Jhc2U/b2JqZWN0Q2xhc3M9Y2VydGlmaWNhdGlv
| bkF1dGhvcml0eTA1BgNVHREBAf8EKzApgg9kYy5hYnNvbHV0ZS5odGKCDGFic29s
| dXRlLmh0YoIIYWJzb2x1dGUwTwYJKwYBBAGCNxkCBEIwQKA+BgorBgEEAYI3GQIB
| oDAELlMtMS01LTIxLTQwNzgzODIyMzctMTQ5MjE4MjgxNy0yNTY4MTI3MjA5LTEw
| MDAwDQYJKoZIhvcNAQEFBQADggEBACZFW9xR+dm7QH/lpvMy7hRQaksrXXzdA37l
| UNpQaHqIbSk7p48+UpkfuufXcGXLKh5Uxw1XARPRZPcQuvI5xynj6rVTm0ImsGrU
| JLijIepb89N7iglFw7E+JlsIZNN/Maw6xrryU0XhhOr3B6FX9odmYiaeh2CKClbi
| 5hpopmXzR6fBKsLOLmq/EuY7xufiv1gTJrnAD7B3mstSvfpdjcyR0aPuBIG/Jq4n
| PCHN4sCne9IqL13zelIE+m47urN6/4NoablwTH766MwTOkRiKw+HqF/QskrC51cG
| 2eGyc9b6MoO75oc7jVGm9EFq97DGLi3h6J0xb4hfMkgJlc4gLfs=
|_-----END CERTIFICATE-----
|_ssl-date: 2026-01-21T06:09:47+00:00; +7h00m00s from scanner time.
3268/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: absolute.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2026-01-21T06:09:47+00:00; +7h00m00s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc.absolute.htb, DNS:absolute.htb, DNS:absolute
| Issuer: commonName=absolute-DC-CA/domainComponent=absolute
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha1WithRSAEncryption
| Not valid before: 2025-04-23T18:13:50
| Not valid after:  2026-04-23T18:13:50
| MD5:   88cd:2d01:6795:f78f:4df0:d194:ec78:b90d
| SHA-1: 9831:ec1f:3649:814d:ef5d:6133:235c:c738:0420:b081
| -----BEGIN CERTIFICATE-----
| MIIF8jCCBNqgAwIBAgITbgAAAAcGWG4iffRa8QABAAAABzANBgkqhkiG9w0BAQUF
| ADBIMRMwEQYKCZImiZPyLGQBGRYDaHRiMRgwFgYKCZImiZPyLGQBGRYIYWJzb2x1
| dGUxFzAVBgNVBAMTDmFic29sdXRlLURDLUNBMB4XDTI1MDQyMzE4MTM1MFoXDTI2
| MDQyMzE4MTM1MFowADCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAMOE
| scFqn+1CHWKpA1edwK1615x3KaP4yRDQ+1nnAXDqurdJ/6p18fn3OhSjdg23sgML
| n2ZLklrzGRPiXp2KqeGqxjW1G4PP0gs6/pC5Lsk8zsuqhAAgxgsiU1mUXQ8vLySw
| bx81jpP4ChjsnDs35cVFD7//pOlZv0j20TJVOqw3Ip5ojb8eZS26kjQXp6j+kV5k
| jhHKjK1wTYm42pKdDijCZP5NRuqR43S1WewTo0RaL3N3vxuCwXAyxjyR8naAJeuK
| 2aWnYtN4s7wqyi1i+YTyHklh1yAauYgTbLWulTX6HxFx3bZYPfzkLeI5ZY+MfRfy
| wzLHAc0ahN11soQajtECAwEAAaOCAxswggMXMDgGCSsGAQQBgjcVBwQrMCkGISsG
| AQQBgjcVCIe89nGC4acdhPGdA4Gx8jOCz7kJgW4BIQIBbgIBADAyBgNVHSUEKzAp
| BggrBgEFBQcDAgYIKwYBBQUHAwEGCisGAQQBgjcUAgIGBysGAQUCAwUwDgYDVR0P
| AQH/BAQDAgWgMEAGCSsGAQQBgjcVCgQzMDEwCgYIKwYBBQUHAwIwCgYIKwYBBQUH
| AwEwDAYKKwYBBAGCNxQCAjAJBgcrBgEFAgMFMB0GA1UdDgQWBBRU3ws2ulNPo7O+
| rlSksas8uAtFnzAfBgNVHSMEGDAWgBSAhiBP4MNvSvhCZpCLP19QO92gNzCByAYD
| VR0fBIHAMIG9MIG6oIG3oIG0hoGxbGRhcDovLy9DTj1hYnNvbHV0ZS1EQy1DQSxD
| Tj1kYyxDTj1DRFAsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049U2Vydmlj
| ZXMsQ049Q29uZmlndXJhdGlvbixEQz1hYnNvbHV0ZSxEQz1odGI/Y2VydGlmaWNh
| dGVSZXZvY2F0aW9uTGlzdD9iYXNlP29iamVjdENsYXNzPWNSTERpc3RyaWJ1dGlv
| blBvaW50MIHBBggrBgEFBQcBAQSBtDCBsTCBrgYIKwYBBQUHMAKGgaFsZGFwOi8v
| L0NOPWFic29sdXRlLURDLUNBLENOPUFJQSxDTj1QdWJsaWMlMjBLZXklMjBTZXJ2
| aWNlcyxDTj1TZXJ2aWNlcyxDTj1Db25maWd1cmF0aW9uLERDPWFic29sdXRlLERD
| PWh0Yj9jQUNlcnRpZmljYXRlP2Jhc2U/b2JqZWN0Q2xhc3M9Y2VydGlmaWNhdGlv
| bkF1dGhvcml0eTA1BgNVHREBAf8EKzApgg9kYy5hYnNvbHV0ZS5odGKCDGFic29s
| dXRlLmh0YoIIYWJzb2x1dGUwTwYJKwYBBAGCNxkCBEIwQKA+BgorBgEEAYI3GQIB
| oDAELlMtMS01LTIxLTQwNzgzODIyMzctMTQ5MjE4MjgxNy0yNTY4MTI3MjA5LTEw
| MDAwDQYJKoZIhvcNAQEFBQADggEBACZFW9xR+dm7QH/lpvMy7hRQaksrXXzdA37l
| UNpQaHqIbSk7p48+UpkfuufXcGXLKh5Uxw1XARPRZPcQuvI5xynj6rVTm0ImsGrU
| JLijIepb89N7iglFw7E+JlsIZNN/Maw6xrryU0XhhOr3B6FX9odmYiaeh2CKClbi
| 5hpopmXzR6fBKsLOLmq/EuY7xufiv1gTJrnAD7B3mstSvfpdjcyR0aPuBIG/Jq4n
| PCHN4sCne9IqL13zelIE+m47urN6/4NoablwTH766MwTOkRiKw+HqF/QskrC51cG
| 2eGyc9b6MoO75oc7jVGm9EFq97DGLi3h6J0xb4hfMkgJlc4gLfs=
|_-----END CERTIFICATE-----
3269/tcp  open  ssl/ldap      syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: absolute.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:dc.absolute.htb, DNS:absolute.htb, DNS:absolute
| Issuer: commonName=absolute-DC-CA/domainComponent=absolute
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha1WithRSAEncryption
| Not valid before: 2025-04-23T18:13:50
| Not valid after:  2026-04-23T18:13:50
| MD5:   88cd:2d01:6795:f78f:4df0:d194:ec78:b90d
| SHA-1: 9831:ec1f:3649:814d:ef5d:6133:235c:c738:0420:b081
| -----BEGIN CERTIFICATE-----
| MIIF8jCCBNqgAwIBAgITbgAAAAcGWG4iffRa8QABAAAABzANBgkqhkiG9w0BAQUF
| ADBIMRMwEQYKCZImiZPyLGQBGRYDaHRiMRgwFgYKCZImiZPyLGQBGRYIYWJzb2x1
| dGUxFzAVBgNVBAMTDmFic29sdXRlLURDLUNBMB4XDTI1MDQyMzE4MTM1MFoXDTI2
| MDQyMzE4MTM1MFowADCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAMOE
| scFqn+1CHWKpA1edwK1615x3KaP4yRDQ+1nnAXDqurdJ/6p18fn3OhSjdg23sgML
| n2ZLklrzGRPiXp2KqeGqxjW1G4PP0gs6/pC5Lsk8zsuqhAAgxgsiU1mUXQ8vLySw
| bx81jpP4ChjsnDs35cVFD7//pOlZv0j20TJVOqw3Ip5ojb8eZS26kjQXp6j+kV5k
| jhHKjK1wTYm42pKdDijCZP5NRuqR43S1WewTo0RaL3N3vxuCwXAyxjyR8naAJeuK
| 2aWnYtN4s7wqyi1i+YTyHklh1yAauYgTbLWulTX6HxFx3bZYPfzkLeI5ZY+MfRfy
| wzLHAc0ahN11soQajtECAwEAAaOCAxswggMXMDgGCSsGAQQBgjcVBwQrMCkGISsG
| AQQBgjcVCIe89nGC4acdhPGdA4Gx8jOCz7kJgW4BIQIBbgIBADAyBgNVHSUEKzAp
| BggrBgEFBQcDAgYIKwYBBQUHAwEGCisGAQQBgjcUAgIGBysGAQUCAwUwDgYDVR0P
| AQH/BAQDAgWgMEAGCSsGAQQBgjcVCgQzMDEwCgYIKwYBBQUHAwIwCgYIKwYBBQUH
| AwEwDAYKKwYBBAGCNxQCAjAJBgcrBgEFAgMFMB0GA1UdDgQWBBRU3ws2ulNPo7O+
| rlSksas8uAtFnzAfBgNVHSMEGDAWgBSAhiBP4MNvSvhCZpCLP19QO92gNzCByAYD
| VR0fBIHAMIG9MIG6oIG3oIG0hoGxbGRhcDovLy9DTj1hYnNvbHV0ZS1EQy1DQSxD
| Tj1kYyxDTj1DRFAsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049U2Vydmlj
| ZXMsQ049Q29uZmlndXJhdGlvbixEQz1hYnNvbHV0ZSxEQz1odGI/Y2VydGlmaWNh
| dGVSZXZvY2F0aW9uTGlzdD9iYXNlP29iamVjdENsYXNzPWNSTERpc3RyaWJ1dGlv
| blBvaW50MIHBBggrBgEFBQcBAQSBtDCBsTCBrgYIKwYBBQUHMAKGgaFsZGFwOi8v
| L0NOPWFic29sdXRlLURDLUNBLENOPUFJQSxDTj1QdWJsaWMlMjBLZXklMjBTZXJ2
| aWNlcyxDTj1TZXJ2aWNlcyxDTj1Db25maWd1cmF0aW9uLERDPWFic29sdXRlLERD
| PWh0Yj9jQUNlcnRpZmljYXRlP2Jhc2U/b2JqZWN0Q2xhc3M9Y2VydGlmaWNhdGlv
| bkF1dGhvcml0eTA1BgNVHREBAf8EKzApgg9kYy5hYnNvbHV0ZS5odGKCDGFic29s
| dXRlLmh0YoIIYWJzb2x1dGUwTwYJKwYBBAGCNxkCBEIwQKA+BgorBgEEAYI3GQIB
| oDAELlMtMS01LTIxLTQwNzgzODIyMzctMTQ5MjE4MjgxNy0yNTY4MTI3MjA5LTEw
| MDAwDQYJKoZIhvcNAQEFBQADggEBACZFW9xR+dm7QH/lpvMy7hRQaksrXXzdA37l
| UNpQaHqIbSk7p48+UpkfuufXcGXLKh5Uxw1XARPRZPcQuvI5xynj6rVTm0ImsGrU
| JLijIepb89N7iglFw7E+JlsIZNN/Maw6xrryU0XhhOr3B6FX9odmYiaeh2CKClbi
| 5hpopmXzR6fBKsLOLmq/EuY7xufiv1gTJrnAD7B3mstSvfpdjcyR0aPuBIG/Jq4n
| PCHN4sCne9IqL13zelIE+m47urN6/4NoablwTH766MwTOkRiKw+HqF/QskrC51cG
| 2eGyc9b6MoO75oc7jVGm9EFq97DGLi3h6J0xb4hfMkgJlc4gLfs=
|_-----END CERTIFICATE-----
|_ssl-date: 2026-01-21T06:09:47+00:00; +7h00m00s from scanner time.
5985/tcp  open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        syn-ack ttl 127 .NET Message Framing
47001/tcp open  http          syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49665/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49666/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49667/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49673/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49694/tcp open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
49695/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49701/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49706/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49717/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
60179/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
60207/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
|_clock-skew: mean: 6h59m59s, deviation: 0s, median: 6h59m59s
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 13413/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 37305/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 55312/udp): CLEAN (Timeout)
|   Check 4 (port 39870/udp): CLEAN (Failed to receive data)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-time: 
|   date: 2026-01-21T06:09:42
|_  start_date: N/A

```

## 