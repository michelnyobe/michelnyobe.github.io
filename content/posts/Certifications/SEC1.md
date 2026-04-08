---
title: "SEC 1"
date: 2026-03-06T10:00:00+02:00
draft: true
tags: ["pentest", "Active Directory", "PowerView", "reconnaissance", "CRTP", "red team"]
categories: ["Red Team", "pentester"]
summary: ""
showToc: true
tocOpen: true
---
Validez l'identité du système et vérifiez les comptes utilisateurs et les configurations des groupes locaux.

Vérifiez les autorisations d'accès aux fichiers critiques et la configuration des dossiers partagés.

Assurez-vous de la présence d'une configuration adéquate en vérifiant les services et les tâches planifiées.

Vérifiez l'intégrité et la protection des données système grâce aux fonctions de hachage et de récupération.


érifiez la version du système d'exploitation du poste de travail et les détails de configuration système de base.

Vérifiez les comptes d'utilisateurs locaux et les affectations de groupes pour confirmer que des contrôles d'accès appropriés sont en place.

Vérifiez les paramètres de propriété et d'autorisation des fichiers pour la documentation opérationnelle sensible.

Utilisez la documentation interne du système pour localiser et analyser les fichiers de configuration clés et les enregistrements des utilisateurs autorisés.

Examinez les processus en cours d'exécution, les liens symboliques et les logiciels installés pour évaluer l'état de préparation opérationnelle.

Analysez les captures de paquets à l'aide de Wireshark pour étudier l'activité réseau.

Identifier les hôtes, les adresses et les services observés dans le trafic réseau capturé

Analyser les protocoles réseau de base pour déterminer le comportement et les résultats du système

Analysez les communications basées sur TCP pour comprendre le comportement des sessions.

Interpréter le trafic capturé pour évaluer le comportement normal du réseau dans un environnement de petite entreprise


Identifier les services Web exposés et les points d'entrée des applications grâce à une énumération de base du réseau et des services.

Analysez les requêtes et les réponses HTTP pour comprendre les flux d'authentification et le comportement de l'application.

Découvrez les répertoires, journaux et fichiers accessibles susceptibles de révéler des informations sensibles.

Évaluer la sécurité du tableau de bord de connexion et des API afin de déterminer l'impact d'un accès non autorisé.



Briefing
HelioTech Consulting , une entreprise de services professionnels de taille moyenne spécialisée dans la gestion des données financières et de projets de ses clients, utilise principalement des postes de travail Windows 10 dotés d'une protection de base et gérés par une petite équipe informatique et de sécurité interne. Récemment, l'une de leurs utilisatrices, Janet Holiday, a signalé qu'après avoir ouvert une pièce jointe de facture provenant d'un nouveau fournisseur, son poste de travail a commencé à présenter des dysfonctionnements, notamment des ralentissements et des blocages intermittents. En réponse, l'équipe de sécurité a collecté des éléments clés du poste de travail de Janet, tels que les journaux système, les journaux du pare-feu, les exportations du registre et les rapports d'analyse statique des logiciels malveillants, afin de les préparer pour une analyse hors ligne.

Vous avez été mandaté en tant que prestataire externe en cybersécurité pour effectuer un tri des éléments de preuve hébergés sur une machine dédiée. Votre mission consiste à déterminer si le poste de travail de Janet a été compromis, à reconstituer l'activité des utilisateurs et des processus concernés, à identifier les fichiers exécutables malveillants et les mécanismes de persistance, à évaluer les signes de propagation interne ou d'abus potentiels, et à aligner vos conclusions sur les procédures internes de réponse aux incidents d'HelioTech.

Objectifs de la section
Analysez les artefacts fournis du poste de travail Windows, y compris les événements de sécurité et les traces d'exécution, afin de reconstituer l'activité clé des utilisateurs et des processus liée à l'incident signalé.

Identifier les fichiers exécutables suspects ou malveillants et déterminer comment ils sont lancés et maintenus sur le système.

Utilisez les rapports d'analyse statique des logiciels malveillants pour classifier le comportement des fichiers binaires impliqués, comprendre leurs capacités et identifier les correspondances MITRE ATT&CK associées.

Corréler les résultats techniques avec le manuel de réponse aux incidents d'HelioTech afin de déterminer la phase active de réponse à l'incident et de recommander des mesures de confinement immédiates appropriées.

Question 1
Quel est l'identifiant utilisateur (UserSid) de l'utilisateur qui a signalé l'incident lié à cette enquête ?
par exemple
S-1-5-21-1111111111...
S-1-5-21-1966530601-3185510712-10604624-500
Question 2
Quel est le nom exact du fichier joint suspect que Janet a ouvert ?
Invoice_Apr2025
Question 3
À quelle heure Janet a-t-elle exécuté la première pièce jointe suspecte ? Réponse au format HH:MM (sur 24 heures).
14:14
Question 4
Quel était le chemin d'accès complet du fichier exécutable malveillant configuré pour persister sur HTC-WKS01 ?
C:\Windows\system32\SecurityHealthSystray.exe
Question 5
L'équipe d'analyse de logiciels malveillants a analysé le fichier. À quelle catégorie de logiciel malveillant MAEC ce fichier est-il attribué ?
trojan
Question 6
Quel mécanisme de persistance Windows ce fichier utilise-t-il pour démarrer automatiquement ?
Startup Folder
Question 7
Quelle technique MITRE ATT&CK est associée à cette persistance ? (Format de réponse : TXXXX.XXX)
T1547.001
Question 8
Quelle adresse IP interne a tenté de se connecter au poste de travail de Janet via SMB ?
par exemple
192.168.15.7
Question 9
Dans quelle phase de réponse aux incidents opérez-vous principalement lors du triage des postes de travail ?
Identification
Question 10
Quelle est la phrase de deux mots recommandée par HelioTech comme mesure de confinement immédiate en cas d'infection par un logiciel malveillant confirmée sur un poste de travail ?
Isolate Host



SentinelWorks is a small technology firm that manages data processing for its clients and for its own internal needs. To support its day-to-day operations, the company hosts two lightweight PHP web applications on its server.

During a routine audit, the Head of Security raised concerns about inconsistent password hygiene, scattered leftover credentials, and the possibility that different teams may have been reusing weak or outdated secrets. You have been engaged under contract to audit the organisation’s password policy and key management practices across both applications and the server that hosts them.

Your role as an auditor is to assess whether SentinelWorks’ server enforces a consistent password policy and to identify any leftover credentials or artefacts that may undermine the company’s security.

Section objectives
Assess exposed network services to identify potential entry points that could enable unauthorised access to the system

Evaluate the effectiveness of authentication controls by reviewing password strength

Identify unpublished application endpoints and validate that appropriate authentication and authorisation controls are enforced

Analyse stored credentials and cryptographic artefacts to assess the security of hashing and key protection mechanisms

Identify potential exposure of sensitive user or system data through files, archives, keys, or residual artefacts within the environment

Question 1
What TCP ports are open on the server?
eg.
21, 25, 110
22,80,19283
Question 2
What is the admin password for the SentinelFiles application?
letmein
Question 3
Which noteworthy endpoint can be found in the SentinelFiles application?
eg.
/index.php
/files
Question 4
What password was used to protect the ZIP archive?
Question 5
Which user account is configured with a weak password?
Question 6
What is the SHA-256 hash of the note in the user's home directory?
Question 7
Which hashing algorithm was used to produce the password hash in the note?
Question 8
What is the login password for the account that provides access to the KeyWatch application?
Question 9
What is the passphrase of the stored root SSH private key?
Question 10
What is the subject line of the caesar cipher encrypted .eml file in the root user’s directory?


Briefing
Ledger Solutions, a small tech startup that specialises in financial software, recently identified suspicious activity originating from one of its internal finance systems. The company detected some repeated micro-transactions being sent to unknown digital wallets, each transfer deliberately small enough to bypass automated alerting mechanisms. However, over time, the consistent pattern raised concerns. The activity began shortly after the finance team installed a cracked version of a commercial accounting application, causing concerns that the software may have introduced a hidden malicious component into the environment.

You have been engaged as an independent cyber security contractor by Ledger Solutions to perform an initial inspection of the suspicious executable bundled with the accounting tool. Your role is to conduct a basic static analysis of the binary to understand its behaviour, uncover embedded indicators such as encoded strings or network destinations, and determine whether the file poses an ongoing security risk to the organisation.

Section objectives
Identify the file type, architecture, entry point, and compilation metadata of the suspicious executable.

Analyse the binary for embedded strings, encoded data, and hardcoded URLs or domains.

Decode and validate any obfuscated indicators to uncover hidden download or execution behaviour.

Classify the executable based on its observed functionality and determine its malware category.