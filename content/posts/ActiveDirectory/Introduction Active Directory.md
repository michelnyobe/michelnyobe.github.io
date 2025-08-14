
Active Directory Domain Services (AD DS) et ses services connexes constituent la base des réseaux d’entreprise qui exécutent des systèmes d’exploitation Windows et non-Windows.
## les composants logiques ?

Les composants logiques AD DS sont des structures que vous utilisez pour implémenter une conception AD DS appropriée pour une organisation.
- Partition : Une partition, ou contexte de nommage, est une partie de la base de données AD DS. Bien que la base de données se compose d’un fichier nommé Ntds.dit, les différentes partitions contiennent des données différentes.
- schéma : Un schéma est l’ensemble des définitions des types d’objets et des attributs que vous utilisez pour définir les objets créés dans AD DS.
- Domain : Un domaine est un conteneur d’administration logique pour les objets tels que les utilisateurs et les ordinateurs. Un domaine est associé à une partition spécifique et vous pouvez organiser le domaine avec des relations parent-enfant avec d’autres domaines.
- Arborescence de domaine : Une arborescence de domaine est une collection hiérarchique de domaines qui partagent un domaine racine commun et un espace de noms DNS (Domain Name System) contigu
- Foret : Une forêt est un regroupement d’un ou de plusieurs domaines qui ont une racine AD DS commune, un schéma commun et un catalogue global commun
- OU : Une unité d’organisation est un objet conteneur pour les utilisateurs, les groupes et les ordinateurs qui fournit un cadre pour déléguer les droits administratifs et l’administration en liant des objets Stratégies de groupe (GPOs).
- Conteneur : Un conteneur est un objet qui fournit une infrastructure organisationnelle à utiliser dans AD DS

## les composants physiques ?
- Contrôleur de domaine : Un contrôleur de domaine contient une copie de la base de données AD DS
- Magasin de données : Une copie du magasin de données existe sur chaque contrôleur de domaine. La base de données AD DS utilise la technologie de base de données Microsoft Jet et stocke les informations d’annuaire dans le fichier Ntds.dit et les fichiers journaux associés.
- Serveur du catalogue global : Un serveur de catalogue global est un contrôleur de domaine qui héberge le catalogue global, qui est une copie partielle en lecture seule de tous les objets dans une forêt multi-domaines.
- Contrôleur de domaine en lecture seule (RODC) : Un contrôleur de domaine en lecture seule est une installation spéciale et en lecture seule de AD DS.
- Site : Un site est un conteneur pour les objets AD DS, tels que les ordinateurs et les services spécifiques à un emplacement physique
- Sous-réseau : Un sous-réseau est une portion des adresses IP du réseau d’une organisation attribuée aux ordinateurs dans un site.

# forêts et des domaines Active Directory Domain Services
Une forêt Active Directory Domain Services (AD DS) est une collection d’une ou plusieurs arborescences AD DS qui contiennent un ou plusieurs domaines AD DS
- Une racine commune.
- Un schéma commun.
- Un catalogue global.

Un domaine AD DS est un conteneur d’administration logique pour les objets tels que :

- Utilisateurs
- Groupes
- Ordinateurs
## Qu’est-ce qu’une forêt AD DS ?

Une forêt est un conteneur de niveau supérieur dans AD DS. Chaque forêt est un regroupement d’une ou plusieurs arborescences de domaines qui partagent un schéma de répertoire commun et un catalogue global.

## Qu’est-ce qu’un domaine AD DS ?
Un domaine AD DS est un conteneur logique pour la gestion de l’utilisateur, de l’ordinateur, du groupe et d’autres objets. La base de données AD DS stocke tous les objets de domaine, et chaque contrôleur de domaine stocke une copie de la base de données.

## Que sont les relations d’approbation ?
Les approbations AD DS permettent l’accès aux ressources dans un environnement AD DS complexe. Lorsque vous déployez un domaine unique, vous pouvez facilement accorder l’accès aux ressources au sein du domaine aux utilisateurs et aux groupes à partir du domaine. Lorsque vous implémentez plusieurs domaines ou forêts, vous devez vous assurer que les approbations appropriées sont en place pour permettre le même accès aux ressources.


# creation d'un nouveau Domaine 


**Avant de commencer, veillez à effectuer ces différentes actions sur votre serveur :**

- Définir un nom d'hôte sur le serveur
- Définir une adresse IP statique (configuration réseau manuelle)
- Effectuer les dernières mises à jour Windows
- Définissez un mot de passe complexe pour le compte Administrateur de votre serveur (ce sera votre futur compte Administrateur du domaine)

Cette procédure est réalisée sous Windows Server 2019, mais vous pouvez l'appliquer sur les versions plus récentes

## Créer un domaine Active Directory
### A. Installer le rôle ADDS

La première étape étape, avant de créer le domaine Active Directory, consiste à installer le rôle "**ADDS**" : **Active Directory Domain Services**. Il s'agit du rôle permettant de créer un domaine Active Directory.
Ouvrez le Gestionnaire de serveur, puis cliquez sur "**Gérer**" puis "**Ajouter des rôles et fonctionnalités**".

![[Pasted image 20250812104728.png]]

L'étape cruciale de l'installation du rôle est ici, puisqu'il va falloir cocher "**Services AD DS**" dans la liste. Une second fenêtre va apparaître pour vous proposer d'installer les outils de gestion : validez. Qui dit outils de gestion, dit console d'administration comme "**Utilisateurs et ordinateurs [Active Directory](https://www.it-connect.fr/cours-tutoriels/administration-systemes/windows-server/windows-active-directory/ "Active Directory")**" mais aussi le module PowerShell pour Active Directory.

![[Pasted image 20250812105052.png]]
Lisez les messages indiqués à l'étape "**AD DS**". Microsoft en propose pour évoquer la synchronisation Active Directory avec Microsoft Entra ID (Azure Active Directory), son penchant dans le [Cloud](https://www.it-connect.fr/cours-tutoriels/administration-systemes/stockage/cloud/ "Cloud") Microsoft.

![[Pasted image 20250812105358.png]]
### Créer le domaine Active Directory

Comme il s'agit d'un nouveau domaine dans une nouvelle forêt, choisissez "**Ajouter une nouvelle forêt**" et indiquez le nom de domaine. Si vous utilisez un nom de domaine tel que "".

![[Pasted image 20250812110935.png]]
