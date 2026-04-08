---
title: "Shell et payloads"
date: 2026-03-06T10:00:00+02:00
draft: true
tags: ["pentest", "Active Directory", "PowerView", "reconnaissance", "CRTP", "red team"]
categories: ["Red Team", "pentester"]
summary: ""
showToc: true
tocOpen: true
---

```shell-session
python -c 'import pty; pty.spawn("/bin/sh")' 
```

# Génération de Shell interactifs
## Qu'est-ce qu'un Web Shell ?
https://attack.mitre.org/techniques/T1505/003/
Les fonctions d'exécution système en PHP , telles que shell_exec(), exec(), system()et passthru(), peuvent être utilisées abusivement pour obtenir l'exécution de commandes.

https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys