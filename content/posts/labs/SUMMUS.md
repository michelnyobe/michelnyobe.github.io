SUMMUS: Extreme Red Teamer Lab
https://extremeredlab.0x29a.it/redteamlabs

# Enumeration
## nmap 

 nmap -sn 10.8.0.130/24 

```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-31 14:50 CEST
Nmap scan report for 10.8.0.1
Host is up (0.10s latency).
Nmap scan report for 10.8.0.34
Host is up (0.37s latency).
Nmap scan report for 10.8.0.46
Host is up (0.38s latency).
Nmap scan report for 10.8.0.130
Host is up.
Nmap done: 256 IP addresses (4 hosts up) scanned in 35.63 seconds
```

nmap -sV -sC 10.8.0.46 -v  --min-rate 1000 -Pn -T4 -p-


nmap -sV -sC 10.8.0.34 -v  --min-rate 1000 -Pn -T4 -p-

```
PORT     STATE SERVICE    VERSION
1716/tcp open  tcpwrapped
```
