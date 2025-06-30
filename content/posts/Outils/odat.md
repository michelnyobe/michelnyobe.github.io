un outil de test de pénétration open source qui teste la sécurité des bases de données Oracle à distance.
Grâce à ODAT, vous pouvez :

- rechercher **un SID valide** sur un écouteur de base de données Oracle distant via :
    - une attaque par dictionnaire
    - une attaque par force brute
    - ALIAS de l'auditeur
- **rechercher des comptes** Oracle en utilisant :
    - une attaque par dictionnaire
    - chaque utilisateur Oracle aime le mot de passe (besoin d'un compte avant d'utiliser cette attaque)
- **exécuter des commandes système** sur le serveur de base de données en utilisant :
    - DBMS_SCHEDULER
    - JAVA
    - tables externes
    - oradbg
- **télécharger les fichiers** stockés sur le serveur de base de données en utilisant :
    - FICHIER_UTL
    - DBMS_XSLPROCESSOR
    - tables externes
    - CTXSYS
    - DBMS_LOB
- **télécharger des fichiers** sur le serveur de base de données en utilisant :
    - FICHIER_UTL
    - DBMS_XSLPROCESSOR
    - DBMS_ADVISOR
- **supprimer des fichiers** en utilisant :
    - FICHIER_UTL
- **obtenez un accès privilégié** en utilisant les combinaisons de privilèges système suivantes (voir l'aide pour les commandes du module _privesc_ ) :
    - CRÉER N'IMPORTE QUELLE PROCÉDURE
    - CRÉER UNE PROCÉDURE et EXÉCUTER N'IMPORTE QUELLE PROCÉDURE
    - CRÉER N'IMPORTE QUEL DÉCLENCHEUR (ET CRÉER UNE PROCÉDURE)
    - ANALYSER TOUT (ET CRÉER UNE PROCÉDURE)
    - CRÉER N'IMPORTE QUEL INDEX (ET CRÉER UNE PROCÉDURE)
- **envoyer/recevoir des requêtes HTTP** depuis le serveur de base de données en utilisant :
    - UTL_HTTP
    - HttpUriType
- **scanner les ports** du serveur local ou d'un serveur distant en utilisant :
    - UTL_HTTP
    - HttpUriType
    - UTL_TCP
- **capturer une authentification SMB** via :
    - un index afin de déclencher une connexion SMB
- exploiter certains CVE :
    - la [**CVE-2012-3137**](http://cvedetails.com/cve/2012-3137)
        - récupérer la clé de session et le sel pour des utilisateurs arbitraires
        - attaque par dictionnaire sur les sessions
    - le [**CVE-2012-????**](https://twitter.com/gokhanatil/status/595853921479991297) : Un utilisateur authentifié peut modifier toutes les tables qu'il peut sélectionner même s'il ne peut pas les modifier normalement (pas de privilège ALTER).
    - l' attaque [**CVE-2012-1675**](http://seclists.org/fulldisclosure/2012/Apr/204) (alias attaque par empoisonnement TNS)
- **rechercher dans les noms de colonnes** grâce au module _de recherche_ :
    - rechercher un modèle (ex : mot de passe) dans les noms de colonnes
- **déballer** le code source PL/SQL (10g/11g et 12c)
- Obtenir les privilèges et les rôles **système** **accordés** . Il est également possible d'obtenir les privilèges et les rôles des rôles accordés.
- exécuter des requêtes SELECT arbitraires (alias shell SQL minimal)


documentation : https://github.com/quentinhardy/odat/wiki

https://www.kali.org/tools/odat/