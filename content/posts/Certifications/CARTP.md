# ✅ Checklist de préparation CARTP

Liste des compétences et tâches à maîtriser pour la certification **Certified Azure Red Team Professional (CARTP®)** — Altered Security.

---

## I. Introduction & Concepts fondamentaux

- [ ] Comprendre Azure et Azure AD (Entra ID)
- [ ] Maîtriser l'architecture Azure et les role assignments
- [ ] Comprendre les concepts : Management Groups, Subscriptions, Resource Groups, Resources
- [ ] Comprendre Azure Resource Manager (ARM)
- [ ] Comprendre les Managed Identities (System-assigned vs User-assigned)
- [ ] Comprendre les rôles Azure AD et leur relation avec les rôles Azure
- [ ] Maîtriser la méthodologie d'attaque et l'Azure Kill Chain
- [ ] Différencier Azure RBAC vs Azure AD Roles

---

## II. Recon & Discovery

- [ ] Effectuer de l'OSINT sur une organisation cible
- [ ] Faire de l'énumération non authentifiée d'un tenant Azure
- [ ] Découvrir le tenant ID à partir d'un domaine
- [ ] Énumérer les sous-domaines pour découvrir les services Azure utilisés
- [ ] Valider les adresses email d'une organisation (user enumeration)
- [ ] Comprendre les permissions par défaut d'un utilisateur Azure AD dans un tenant
- [ ] Utiliser des outils comme `o365creeper`, `MSOLSpray`, `AADInternals`
- [ ] Identifier si une organisation utilise Azure AD / Federation / Hybrid

---

## III. Initial Access

- [ ] Comprendre les opsec fails liés au password spraying en cloud
- [ ] Maîtriser les permissions MS Graph API (low-impact vs high-impact)
- [ ] Exécuter une attaque **Illicit Consent Grant** contre des simulations utilisateur
- [ ] Passer d'un access token à la compromission d'une workstation
- [ ] Abuser d'applications web internet-facing pour obtenir un foothold
- [ ] Utiliser une Managed Identity assignée à une web app pour pivoter
- [ ] Abuser de Storage Accounts non sécurisés (blobs anonymes)
- [ ] Exécuter des attaques de **phishing** pour obtenir des credentials clear-text
- [ ] Utiliser des Application credentials (client_id/secret) pour accéder au tenant
- [ ] Abuser du CI/CD (compromission compte GitHub → accès tenant Azure)
- [ ] Comprendre Device Code Phishing

---

## IV. Enumeration (authentifiée)

- [ ] Énumérer users, groups, devices, service principals, applications, roles dans Azure AD
- [ ] Lister les objets possédés par un objet Azure AD (mapping d'attack paths)
- [ ] Utiliser **Azure AD Module** (AzureAD / MSOnline)
- [ ] Utiliser **Az PowerShell**
- [ ] Utiliser **Az CLI**
- [ ] Énumérer les device identities (active devices, owners, state)
- [ ] Trouver les service principals abusables (app password, ownership, roles)
- [ ] Énumérer les ressources Azure : VMs, Storage Accounts, Key Vaults, Blobs
- [ ] Énumérer Automation Accounts, Deployment Templates, App Services, Function Apps
- [ ] Énumérer les role assignments pour mapper les attack paths
- [ ] Utiliser **ROADRecon** pour l'énumération
- [ ] Utiliser **AzureHound** pour l'énumération
- [ ] Différencier l'énumération Azure AD vs on-prem AD
- [ ] Utiliser l'**ARM REST API** pour l'énumération
- [ ] Utiliser la **MS Graph REST API** pour l'énumération

---

## V. Privilege Escalation

- [ ] Abuser des **Automation Accounts** (Runbooks, Run As accounts)
- [ ] Abuser des **Key Vaults** pour PrivEsc
- [ ] Abuser des **Storage Accounts** pour PrivEsc
- [ ] Abuser de la **Deployment History** (extraction de secrets)
- [ ] Escalader via les **Custom Roles** (Azure AD et Azure)
- [ ] Abuser de l'Ownership sur des ressources
- [ ] Abuser de l'ajout d'application password pour PrivEsc
- [ ] Abuser des role assignments (horizontal & vertical)
- [ ] Abuser de **Custom Script Extension** pour exécuter en SYSTEM sur VM
- [ ] Abuser de **RunCommand** sur Azure VMs
- [ ] Comprendre les **Dynamic Groups**
- [ ] Abuser de la membership rule des Dynamic Groups pour rejoindre un groupe
- [ ] Identifier les chemins de PrivEsc cross-subscription

---

## VI. Lateral Movement

- [ ] Abuser des **Hybrid Workers** via Runbooks pour passer du cloud à l'on-prem
- [ ] Faire du lateral movement GitHub → Azure tenant via GitHub Actions + Function App
- [ ] Exécuter une attaque **Pass-The-PRT** (replay du PRT cookie)
- [ ] Abuser **Intune** pour exécuter des commandes sur des machines on-prem
- [ ] Attaquer les **Application Proxy** apps (cloud → on-prem)
- [ ] Comprendre et abuser **Password Hash Sync (PHS)**
- [ ] Comprendre et abuser **Pass-Through Authentication (PTA)**
- [ ] Comprendre et abuser **Federation (AD FS)**
- [ ] Faire du lateral movement on-prem → cloud via Hybrid Identity
- [ ] Faire du lateral movement entre tenants

---

## VII. Persistence

- [ ] Identifier les opportunités de persistance liées à l'Hybrid Identity
- [ ] Abuser **AZUREADSSOACC** pour la persistance via Seamless SSO
- [ ] Comprendre la criticité du serveur **Azure AD Connect**
- [ ] Réaliser une persistance OS-level sur Azure AD Connect
- [ ] Exécuter l'attaque **Skeleton Key dans le cloud**
- [ ] Exécuter l'attaque **Golden SAML**
- [ ] Mettre en place la persistance via Storage Accounts
- [ ] Mettre en place la persistance via Applications & Service Principals
- [ ] Mettre en place la persistance via Consents et Permissions OAuth
- [ ] Mettre en place la persistance via Azure VMs et NSGs
- [ ] Créer des Custom Azure roles et Custom Azure AD roles pour persistance
- [ ] Backdoor un Service Principal (ajout de credentials)

---

## VIII. Data Mining

- [ ] Extraire des secrets depuis les **Key Vaults** via Managed Identity
- [ ] Mapper les attack paths en lisant les role assignments
- [ ] Extraire des secrets depuis la **Deployment History**
- [ ] Extraire les application passwords clear-text d'une workstation Azure compromise
- [ ] Extraire les tokens (refresh, access) d'une workstation compromise
- [ ] Extraire des secrets depuis le **Blob Storage**
- [ ] Lire les variables d'environnement / app settings dans Function Apps & App Services

---

## IX. Defenses (à comprendre côté défensif)

- [ ] Connaître la guidance Microsoft par catégorie de défense
- [ ] Maîtriser les **Conditional Access Policies (CAP)**
- [ ] Maîtriser **Privileged Identity Management (PIM)**
- [ ] Comprendre **Azure AD Identity Protection**
- [ ] Comprendre **Microsoft Defender for Cloud**
- [ ] Comprendre **Just-in-Time (JIT) access**
- [ ] Comprendre **Adaptive Network Hardening (ANH)**
- [ ] Comprendre **Adaptive Application Control**
- [ ] Comprendre **File Integrity Monitoring (FIM)**
- [ ] Comprendre **Continuous Access Evaluation (CAE)** et son impact sur le replay de tokens
- [ ] Connaître les différentes méthodes d'enforcement MFA dans Azure AD

---

## X. Defenses Bypass

- [ ] Pratiquer les bypass des **Conditional Access Policies** les plus courantes
- [ ] Pratiquer l'opsec pour éviter les détections d'**Azure AD Identity Protection**
- [ ] Bypass **JIT** pour accéder aux VMs
- [ ] Bypass **ANH** et **NSGs**
- [ ] Bypass **Azure Firewall**
- [ ] Bypass **Cloud Workload Protection**

### 🔐 Sous-section MFA Bypass (focus du lab manual)

- [ ] Comprendre les différentes méthodes d'enforcement MFA (per-user, CA, Security Defaults)
- [ ] **Evading Mandatory MFA Enforcement** (technique pointée dans le lab manual)
- [ ] Identifier les comptes/scénarios non couverts par MFA (legacy auth, service accounts)
- [ ] Bypass MFA via legacy authentication protocols (si encore activé)
- [ ] Bypass MFA via service principals / application authentication
- [ ] Bypass MFA via PRT / Pass-The-PRT (déjà MFA-claimed)
- [ ] Bypass MFA via Family of Client IDs (FOCI) et token swap
- [ ] Bypass MFA via device code phishing
- [ ] Comprendre l'impact du device compliance / Hybrid Azure AD Joined sur MFA
- [ ] Comprendre les exclusions de Conditional Access et leur abus

---

## 🛠️ Outils à maîtriser (transverse)

- [ ] **AADInternals**
- [ ] **ROADtools** / ROADRecon
- [ ] **AzureHound** + BloodHound
- [ ] **MicroBurst**
- [ ] **PowerZure**
- [ ] **Az PowerShell** + **Az CLI** + **AzureAD module**
- [ ] **TokenTactics / TokenTacticsV2**
- [ ] **GraphRunner**
- [ ] **Stormspotter**
- [ ] **MSOLSpray** (password spraying)
- [ ] **o365creeper** (user enumeration)

---

## 📝 Préparation examen (24h hands-on)

- [ ] S'entraîner à rédiger un rapport pro avec proof-of-completion
- [ ] Prendre des screenshots à chaque étape pendant le lab
- [ ] Préparer son cheat sheet personnel (commandes Az / Graph / outils)
- [ ] Refaire les 4 Kill Chains du lab + le CTF en autonomie
- [ ] Tester les attaques avec les 3 modèles d'Hybrid Identity (PHS / PTA / Federation)
- [ ] Vérifier l'accès au lab et au student VM avant le jour J
- [ ] Compromettre toutes les ressources mentionnées dans la page exam

https://www.youtube.com/watch?v=SK1zgqaAZ2E&list=PLyBFM3O1Pe7AB8WeKcieHmlewL6BZYWN6

https://raucousthrone3.com/certified-azure-red-team-expert-carte-review/