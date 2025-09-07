1. **Introduction à la cybersécurité**
    - Comprendre les bases, les enjeux, les types de cyberattaques et les métiers du domaine.
2. **Hygiène numérique et bonnes pratiques**
    - Gestion des mots de passe, MFA, mises à jour, sauvegardes, phishing et scams courants.
3. **Sécurité des réseaux pour débutants**
    - Notions simples : adresse IP, pare-feu, Wi-Fi sécurisé, VPN, attaques réseau fréquentes.
4. **Découverte des systèmes d’exploitation et de leur sécurité**
    - Windows, Linux, Android, iOS : comment ils fonctionnent et comment les protéger.
5. **Sécurité des emails et lutte contre le phishing**
    - Identifier les mails frauduleux, analyser les pièces jointes, se protéger contre l’ingénierie sociale.
6. **Introduction à la cryptographie**
    - Bases : chiffrement, hachage, signatures numériques, certificats SSL.
7. **Sécurité des applications web**
    - Comprendre les failles OWASP Top 10 (XSS, SQL Injection, etc.) avec des exemples simples.
8. **Introduction à l’ethical hacking**
    - Méthodologie, outils basiques (Nmap, Wireshark, Burp Suite), cadre légal et éthique.
9. **Cloud et cybersécurité**
    - Sensibilisation aux risques dans le cloud (AWS, Azure, GCP) et bonnes pratiques de base.
10. **Construire son premier lab de cybersécurité**

- Installer VirtualBox/VMware, créer un petit lab (Windows + Kali Linux), pratiquer en sécurité.

### **Modules Attaque & Défense Cloud Azure**

1. **Introduction à Azure et à la sécurité cloud**
    - Comprendre Entra ID (ex-Azure AD), RBAC, ressources de base (VM, Storage, Key Vault).
    - Différence entre sécurité On-Prem et sécurité Cloud.
2. **Reconnaissance et cartographie d’un tenant Azure**
    - Attaque : découverte d’utilisateurs, groupes, permissions avec AADInternals, RoadRecon.
    - Défense : journaux de connexion, Azure Monitor, Entra ID Protection.
3. **Exploitation des identités et attaques sur Entra ID**
    - Attaque : password spraying, MFA fatigue, consent phishing.
    - Défense : Conditional Access, MFA renforcé, Identity Protection.
4. **Attaques et défenses sur Azure Key Vault**
    - Attaque : extraction de secrets/keys avec des permissions mal configurées.
    - Défense : RBAC, Key Vault firewall, Managed Identity.
5. **Escalade de privilèges dans Azure**
    - Attaque : exploitation des rôles Contributor, App Owner, Automation Account.
    - Défense : principe du moindre privilège, PIM (Privileged Identity Management).
6. **Persistence dans Azure**
    - Attaque : service principals, applications OAuth, rôles persistants.
    - Défense : détection avec Microsoft Sentinel, audit des objets persistants.
7. **Attaques sur le stockage Azure (Blob, File, Disk)**
    - Attaque : accès anonyme, SAS tokens mal configurés.
    - Défense : Private endpoints, Azure Defender for Storage.
8. **Attaques et défense sur les ressources compute (VM, Containers)**
    - Attaque : récupération de credentials dans une VM, container escape.
    - Défense : Defender for Cloud, Just-In-Time VM Access.
9. **Mouvement latéral et pivoting dans Azure**
    - Attaque : abus de managed identities, rôle "User Access Administrator".
    - Défense : Zero Trust, restriction d’accès réseau, journaux détaillés.
10. **Monitoring et Threat Hunting dans Azure**
- Utilisation de **Microsoft Sentinel** pour détecter les attaques.
- Création de règles de détection (KQL), tableaux de bord de surveillance.
1. **Introduction à Active Directory et à ses composants**
    - Domaines, forêts, DC, OU, GPO, comptes utilisateurs et machines.
    - Risques liés à une mauvaise configuration.
2. **Reconnaissance Active Directory**
    - Attaque : PowerView, BloodHound, SharpHound pour cartographier un AD.
    - Défense : audits réguliers, visibilité avec Event Logs, Azure ATP/Microsoft Defender for Identity.
3. **Attaques sur les mots de passe**
    - Attaque : bruteforce, Kerberoasting, AS-REP Roasting, NTLM relay.
    - Défense : MFA, politique de mots de passe robustes, détection via logs d’authentification.
4. **Exploitation de Kerberos**
    
    - Attaque : Pass-the-Ticket, Golden Ticket, Silver Ticket.
        
    - Défense : KRBTGT reset, monitoring des tickets anormaux, détection des anomalies Kerberos.
        
5. **Escalade de privilèges**
    
    - Attaque : exploitation de GPO, abuse des ACL/ACE, priv-esc via permissions mal configurées.
        
    - Défense : principe du moindre privilège, audits BloodHound réguliers, monitoring des délégations.
        
6. **Mouvement latéral dans un environnement AD**
    
    - Attaque : Pass-the-Hash, exploitation de RDP/WinRM, exploitation de PSExec/WMI.
        
    - Défense : LAPS (gestion des mots de passe locaux), Credential Guard, restriction des comptes admin.
        
7. **Persistance dans Active Directory**
    
    - Attaque : création d’objets malveillants (backdoor accounts, SIDHistory, DCSync).
        
    - Défense : surveillance des modifications AD, alertes Sentinel/Defender, audits ACL.
        
8. **Attaques sur les contrôleurs de domaine**
    
    - Attaque : DCSync, DCShadow, vol de NTDS.dit.
        
    - Défense : durcissement des DC, contrôles d’accès, surveillance des requêtes de réplication.
        
9. **Hardening et bonnes pratiques de défense AD**
    
    - Tiering des comptes (Admin/Serveurs/Users).
        
    - Séparation des privilèges, audit régulier des droits et GPO.
        
10. **Détection et réponse aux attaques AD**
    

- Mise en place d’un lab de détection avec SIEM (Sentinel, Splunk, ELK).
    
- Hunting avec logs Windows Event, Sysmon et règles Sigma.