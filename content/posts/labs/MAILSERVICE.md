Kali Jump machine:
 Server: 10.0.5.200
 User: red
 Password: I'mthebest


# Machine 1 : 10.0.5.5
# Enumeration 
## Nmap

```
nmap -sV -sC 10.0.5.5 -v  --min-rate 1000 -Pn -T4 -p-
```


```
PORT      STATE    SERVICE        VERSION
22/tcp    open     ssh            OpenSSH 9.2p1 Debian 2+deb12u5 (protocol 2.0)
| ssh-hostkey: 
|   256 19:86:8f:39:ff:0b:83:67:d8:44:64:7c:b1:4b:5b:16 (ECDSA)
|_  256 8d:b8:c5:d7:4b:59:d5:83:a4:5d:8d:ec:98:55:3e:23 (ED25519)
25/tcp    open     smtp
| fingerprint-strings: 
|   Hello: 
|     220 mailserver SMTP - IMPORTANT: procmail and forward allowed - accepted email ONLY From:<someone@localhost>
|_    Syntactically invalid EHLO argument(s)
|_ssl-date: TLS randomness does not represent time
| smtp-commands: mailserver Hello nmap.scanme.org [10.0.5.200], SIZE 52428800, 8BITMIME, PIPELINING, PIPECONNECT, CHUNKING, STARTTLS, PRDR, HELP
|_ Commands supported: AUTH STARTTLS HELO EHLO MAIL RCPT DATA BDAT NOOP QUIT RSET HELP
139/tcp   open     netbios-ssn    Samba smbd 4
445/tcp   open     netbios-ssn    Samba smbd 4
1080/tcp  open     nagios-nsca    Nagios NSCA
1234/tcp  open     hotline?
4242/tcp  open     tcpwrapped
|_dicom-ping: ERROR: Script execution failed (use -d to debug)
6666/tcp  open     irc?
|_irc-info: Unable to open connection
6667/tcp  open     irc?
|_irc-info: Unable to open connection
6789/tcp  open     ibm-db2-admin?
7530/tcp  open     unknown
7531/tcp  open     http           SimpleHTTPServer 0.6 (Python 3.11.2)
| http-methods: 
|_  Supported Methods: GET HEAD
|_http-server-header: SimpleHTTP/0.6 Python/3.11.2
|_http-title: Directory listing for /
7532/tcp  open     unknown
8080/tcp  filtered http-proxy
8300/tcp  open     tmi?
8400/tcp  open     cvd?
8585/tcp  open     http           SimpleHTTPServer 0.6 (Python 3.11.2)
| http-methods: 
|_  Supported Methods: GET HEAD
|_http-title: Independent HTTP Node :8585
|_http-server-header: SimpleHTTP/0.6 Python/3.11.2
9631/tcp  open     peocoll?
9632/tcp  open     mc-comm?
9999/tcp  open     http           SimpleHTTPServer 0.6 (Python 3.11.2)
| http-methods: 
|_  Supported Methods: GET HEAD
|_http-title: Directory listing for /
|_http-server-header: SimpleHTTP/0.6 Python/3.11.2
14465/tcp open     unknown
31008/tcp open     tcpwrapped
32001/tcp open     tcpwrapped
32002/tcp open     tcpwrapped
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port25-TCP:V=7.95%I=7%D=8/26%Time=68AD84FF%P=x86_64-pc-linux-gnu%r(Hell
SF:o,9A,"220\x20mailserver\x20SMTP\x20-\x20IMPORTANT:\x20procmail\x20and\x
SF:20forward\x20allowed\x20-\x20accepted\x20email\x20ONLY\x20From:<someone
SF:@localhost>\r\n501\x20Syntactically\x20invalid\x20EHLO\x20argument\(s\)
SF:\r\n");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
| nbstat: NetBIOS name: MAILSERVER, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| Names:
|   MAILSERVER<00>       Flags: <unique><active>
|   MAILSERVER<03>       Flags: <unique><active>
|   MAILSERVER<20>       Flags: <unique><active>
|   \x01\x02__MSBROWSE__\x02<01>  Flags: <group><active>
|   WORKGROUP<00>        Flags: <group><active>
|   WORKGROUP<1d>        Flags: <unique><active>
|_  WORKGROUP<1e>        Flags: <group><active>
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
|_clock-skew: 2s
| smb2-time: 
|   date: 2025-08-26T09:58:52
|_  start_date: N/A

```