 listen 


 teamserver 
Votre nom              : Service Admin
Your server's name     : mgmt-core-01
Nom d'utilisateur      : svcmgmt
Mot de passe           : (voir ci-dessous)

Votre nom              : Service Proxy
Your server's name     : proxy-eu-01
Nom d'utilisateur      : svcproxy
Mot de passe           : (différent du teamserver)

## Objectifs

À l'issue de ce TP, les étudiants seront capables de :

1. **Installation et configuration** du serveur d'équipe, du client et des extensions AdaptixC2
2. **Déployez des redirecteurs de trafic** à l'aide de Nginx, iptables et socat pour la sécurité opérationnelle.
3. **Déployez l'agent Kharon** avec des capacités d'évasion avancées (PIC, obfuscation du sommeil, appels système indirects).
4. **Créez des écouteurs HTTP/HTTPS** avec des profils C2 malléables qui imitent le trafic légitime
5. **Générer des charges utiles** dans plusieurs formats (EXE, DLL, Service, Shellcode) avec des configurations de sécurité opérationnelles
6. **Effectuer la post-exploitation** sur les cibles Windows et Linux
7. **Attaquer Active Directory** en utilisant Kerberoasting, AS-REP Roasting, DCSync, Pass-the-Hash, Golden Tickets et l'abus des ACL
8. **Compromettre les systèmes Linux** via l'exploitation d'applications web, l'élévation de privilèges et la persistance
9. **Exécuter un déplacement latéral** via WinRM, WMI, SCM, DCOM et liaison de balise SMB/TCP
10. **Pivotez à travers les réseaux** en utilisant le proxy SOCKS5 et la redirection de ports
11. **Développer des BOF personnalisés** conformément à la spécification de l'API Beacon avec compilation croisée
12. **Appliquez des techniques furtives** telles que l'obfuscation du sommeil, le masquage du tas, l'usurpation d'appels système et le contournement d'AMSI/ETW.


### Installation et configuration d'AdaptixC2
Installez et configurez le framework AdaptixC2 sur Kali Linux, y compris l'agent avancé Kharon, la collection Extension-Kit BOF, et préparez l'infrastructure C2 pour les opérations d'équipe rouge.

### ## Composants de laboratoire

|Composant|Version|But|
|---|---|---|
|Kali Linux|Dernier|Machine d'attaque, |
|Serveur ubuntu|Dernier|serveur c2 |
|Serveur Ubuntu|Dernier|Redirection de trafic (Redirection RTL)|
|Windows Server 2016|Standard|Contrôleur de domaine, cible AD (RTL-AD)|
|Serveur Ubuntu|Dernier|Cible Linux avec services vulnérables (RTL-Linux)|
|AdaptixC2|v1.1|Cadre C2 (serveur + client)|
|Agent Kharon|v0.2|Agent C2 PIC avancé|
|Kit d'extension|Dernier|Collection BOF pour la post-exploitation|
|vulnérable-AD|Dernier|script de déploiement de vulnérabilité AD|


#### exigence reseau 

