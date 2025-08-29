# Introduction

Les services de domaine **Active Directory (AD DS)** et leurs services associés constituent la base des réseaux d'entreprise exécutant des systèmes d'exploitation **Windows** et non Windows.  
La base de données **AD DS** est le référentiel central de tous les objets du domaine, tels que les **comptes d'utilisateurs**, les **comptes d'ordinateurs** et les **groupes**.

---

## Les services de domaine Active Directory

**AD DS** comprend des **composants logiques** et **physiques**.  
Il est important de comprendre comment ces composants interagissent pour gérer efficacement votre infrastructure.  

De plus, vous pouvez utiliser les options AD DS pour effectuer des actions telles que :

- **Installation, configuration et mise à jour des applications**  
- **Gestion de l'infrastructure de sécurité**  
- **Activation du service d'accès à distance et de DirectAccess**  
- **Émission et gestion de certificats numériques**  

---

### Les composants logiques  

Les composants logiques **AD DS** sont des structures permettant de mettre en œuvre une conception AD DS adaptée à une organisation.  

- **Partition** : Une partition, ou contexte de nommage, est une partie de la base de données AD DS.  
  - La **partition de schéma** contient une copie du schéma Active Directory.  
  - La **partition de configuration** contient les objets de configuration de la forêt.  
  - La **partition de domaine** contient les utilisateurs, les ordinateurs, les groupes et autres objets spécifiques au domaine.  

- **Schéma** : Ensemble des définitions des types d’objets et des attributs utilisés pour définir les objets créés dans AD DS.  

- **Domaine** : Conteneur administratif logique pour des objets tels que des utilisateurs et des ordinateurs.  
  - Organisé selon des relations **parent-enfant** avec d'autres domaines.  

- **Arborescence de domaine** : Collection hiérarchique de domaines partageant un **domaine racine commun** et un **espace de noms DNS contigu**.  

- **Forêt** : Ensemble d’un ou plusieurs domaines ayant une **racine AD DS commune**, un **schéma commun** et un **catalogue global commun**.  

- **OU (Organizational Unit)** : Conteneur pour les **utilisateurs**, **groupes** et **ordinateurs**.  
  - Fournit un cadre pour la **délégation des droits administratifs** et l’administration via les **GPO**.  

- **Container** : Objet fournissant un cadre organisationnel dans AD DS.  
  - Utilisation possible des **conteneurs par défaut** ou **conteneurs personnalisés**.  

---

### Les composants physiques  

Les composants physiques dans **AD DS** sont des objets tangibles ou décrivant des composants tangibles dans le monde réel.  

- **Contrôleur de domaine** : Contient une copie de la **base de données AD DS**.  

- **Data store** : Copie du magasin de données présente sur chaque contrôleur de domaine.  
  - La base de données **AD DS** utilise **Microsoft Jet** et stocke les informations dans le fichier `Ntds.dit` et les fichiers journaux associés.  
  - Par défaut, les fichiers se trouvent dans `C:\Windows\NTDS`.  

- **Global catalog server** : Contrôleur de domaine hébergeant le **catalogue global**, une copie partielle en lecture seule de tous les objets d'une forêt multi-domaines.  

- **Contrôleur de domaine en lecture seule (RODC)** : Installation spéciale d'AD DS en lecture seule.  
  - Courant dans les succursales où la **sécurité physique** est limitée ou où le **support informatique** est moins avancé.  

- **Site** : Conteneur pour les objets AD DS spécifiques à un emplacement physique, comme les ordinateurs et services.  

- **Sous-réseau** : Partie des adresses IP réseau d'une organisation attribuées aux ordinateurs d’un site.


## les forêts et les domaines des services de domaine Active Directory

Une forêt AD DS (Active Directory Domain Services) est un ensemble d'une ou plusieurs arborescences AD DS contenant un ou plusieurs domaines AD DS. Les domaines d'une forêt partagent :
- Une racine commune.
- Un schéma commun.
- Un catalogue mondial.
Un domaine AD DS est un conteneur administratif logique pour des objets tels que :
- Utilisateurs
- Groupes
- Ordinateurs

### Qu'est-ce qu'une forêt AD DS ?

Une **forêt** est un conteneur de niveau supérieur dans **AD DS**.  
Chaque forêt est un ensemble d'une ou plusieurs **arborescences de domaines** partageant un **schéma d'annuaire commun** et un **catalogue global**.  
Une arborescence de domaines est un ensemble d'un ou plusieurs domaines partageant un **espace de noms contigu**.  
Le **domaine racine** de la forêt est le premier domaine créé dans la forêt.

> **Conseil :**  
> En règle générale, une organisation ne crée qu’une seule forêt.

Les objets suivants existent dans chaque domaine (y compris la racine de la forêt) :

- **Rôle maître d'ID relatif (RID)**  
- **Rôle de maître d'infrastructure**  
- **Rôle maître de l'émulateur du contrôleur de domaine principal (PDC)**  
- **Groupe d'administrateurs de domaine**  

---

### Qu'est-ce qu'un domaine AD DS ?

Un **domaine AD DS** est un conteneur logique permettant de gérer les **utilisateurs**, **ordinateurs**, **groupes** et autres objets.  
La base de données **AD DS** stocke tous les objets du domaine, et chaque **contrôleur de domaine** en conserve une copie.

Les objets les plus couramment utilisés sont :

- **Comptes utilisateurs** : contiennent des informations sur les utilisateurs, y compris celles requises pour **authentifier un utilisateur** pendant la connexion et créer le **jeton d'accès** de l'utilisateur.  
- **Comptes informatiques** : chaque ordinateur joint à un domaine possède un compte dans **AD DS**. Ces comptes sont utilisés de la même manière que les comptes utilisateur.  
- **Groupes** : organisent les utilisateurs ou ordinateurs pour simplifier la **gestion des autorisations** et des objets de **stratégie de groupe** dans le domaine.

---

## Les objets Active Directory

### Utilisateurs, groupes et ordinateurs

Dans **AD DS**, chaque utilisateur ayant besoin d'accéder aux ressources réseau doit disposer d'un **compte utilisateur**.  
Ce compte permet de **s'authentifier** auprès du domaine AD DS et d'accéder aux ressources réseau.

Sous **Windows Server**, un compte utilisateur est un objet contenant toutes les informations définissant un utilisateur.  
Un compte utilisateur comprend :

- **Nom d'utilisateur**  
- **Mot de passe utilisateur**  
- **Adhésions de groupe**  

Avant d'implémenter des groupes dans votre organisation, il est important de comprendre la **portée** des différents types de groupes AD DS et comment les utiliser pour **gérer l'accès aux ressources** ou **attribuer des droits et responsabilités de gestion**.

---

### Types de groupes

Dans un réseau d’entreprise **Windows Server**, il existe deux types de groupes :

- **Sécurité** : les groupes de sécurité permettent d’attribuer des **autorisations** à diverses ressources.  
  - Ils peuvent être utilisés dans les **listes de contrôle d'accès (ACL)** pour contrôler l'accès aux ressources.  

- **Distribution** : utilisés généralement par les **applications de messagerie**, ces groupes ne sont pas sécurisés.

---

### Unités organisationnelles (UO)

Une **unité organisationnelle (UO)** est un objet conteneur au sein d'un domaine permettant de consolider les **utilisateurs**, **ordinateurs**, **groupes** et autres objets.  

Vous pouvez **lier directement des objets de stratégie de groupe (GPO)** à une UO pour gérer les utilisateurs et ordinateurs qu'elle contient.


## Les  stratégie de groupe dans Active Directory
###  Qu'est-ce que la stratégie de groupe ?
La stratégie de groupe est une infrastructure des systèmes d'exploitation Windows dont les composants résident dans Active Directory (AD), sur les contrôleurs de domaine et sur chaque serveur et client Windows.
Grâce à ces composants, vous pouvez gérer la configuration d'un domaine AD DS. Les paramètres de stratégie de groupe sont définis dans un objet de stratégie de groupe (GPO). Un GPO est un objet contenant un ou plusieurs paramètres de stratégie s'appliquant à un ou plusieurs paramètres de configuration d'un utilisateur ou d'un ordinateur.

### Que sont les GPO ?
Le composant le plus précis d'une stratégie de groupe est un paramètre de stratégie individuel. Un paramètre de stratégie individuel définit une configuration spécifique, par exemple un paramètre empêchant un utilisateur d'accéder aux outils d'édition du registre.

##  la sécurité dans Active Directory
###  les droits du compte utilisateur
Lors de la configuration des droits des utilisateurs, il est important de respecter le principe du moindre privilège. Cela signifie qu'il faut accorder aux utilisateurs uniquement les droits et privilèges nécessaires à l'exécution de leurs tâches, sans plus.

### Windows Defender Credential Guard
Windows Defender Credential Guard vous protège contre les attaques NTLM Pass-the-Hash et Kerberos Pass-the-Ticket. Il vous protège en limitant l'accès aux hachages de mots de passe NTLM (Pass-the-Hash), aux TGT Kerberos (Pass-the-Ticket) et aux identifiants d'application stockés comme identifiants de domaine aux processus et à la mémoire spécifiques qui gèrent et stockent ces données d'autorisation et d'authentification.
Windows Defender Credential Guard ne prend pas en charge :

- Délégation Kerberos sans contrainte
- NTLMv1
- MS-CHAPv2
- Authentification Digest
- Délégation d'accréditation (CredSSP)
- Cryptage Kerberos DES (Data Encryption Standard)


# Enumeration 
# les outils 

PowerShell
BloodHound
**Impacket**
CrackMapExec
ldapsearch
Nmap + scripts NSE
PowerView (PowerShell)

## Enumeration du domaine 



