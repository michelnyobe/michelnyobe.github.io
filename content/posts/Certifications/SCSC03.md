# Roadmap de préparation — AWS Certified Security – Specialty (SCS-C03)

**Pour :** Michel **Date de départ :** 29 avril 2026 **Cible :** réussir l'examen **SCS-C03** (lancé décembre 2025) — 750/1000 minimum, 170 min, ~65 questions

> 📌 **Nouveautés clés C02 → C03**
> 
> - **IAM passe de 16 % à 20 %** — c'est désormais **le domaine le plus lourd** (avant c'était Infrastructure Security).
> - **Detection et Incident Response sont maintenant deux domaines distincts** (avant : un seul à 14 %).
> - **Nouveaux formats de questions** : **ordering** (mettre des étapes dans l'ordre) et **matching** (associer des éléments).
> - **Nouveaux sujets 2026** : logging aligné **OCSF** (Open Cybersecurity Schema Framework) avec **Security Lake**, **guardrails pour l'IA générative** (Bedrock Guardrails, sécurité Bedrock).

---

## 1. Domaines & pondérations (SCS-C03)

|#|Domaine|Pondération|
|---|---|---|
|1|**Detection**|16 %|
|2|**Incident Response**|14 %|
|3|**Infrastructure Security**|18 %|
|4|**Identity and Access Management**|**20 %** ⭐|
|5|**Data Protection**|18 %|
|6|**Security Foundations and Governance**|14 %|

➡️ **IAM + Data Protection + Infrastructure Security = 56 %** : c'est votre cœur de cible. ➡️ **Detection + Incident Response = 30 %** : ne les négligez surtout pas, ils sont maintenant séparés et pèsent plus qu'avant.

---

## 2. Pré-requis recommandés

- **Solutions Architect Associate (SAA-C03)** ou **SysOps Administrator** validé, ou expérience équivalente.
- Bases solides : VPC, EC2, IAM, S3, CloudTrail, CloudWatch.
- 3-5 ans d'expérience IT, dont 2 ans en sécurité AWS recommandés par AWS.
- **Compte AWS** avec budget mensuel ~20-30 USD pour les labs (alarmes de coût configurées !).
- Outils : **AWS CLI v2**, **CloudShell**, **VS Code** + extensions AWS, **CDK** ou **Terraform**.

---

## 3. Plan en 12 semaines (≈ 8–10 h/semaine)

### Phase 1 — Fondations & setup (Semaines 1-2)

- [ ] Lire l'**Exam Guide SCS-C03** officiel en entier (PDF AWS).
- [ ] Créer un compte AWS dédié (séparé de la prod), activer **MFA root**, créer un IAM admin et désactiver l'usage du root.
- [ ] Configurer **AWS Budgets** + alertes de coût (10/20/30 USD).
- [ ] Installer AWS CLI v2, configurer profils.
- [ ] Suivre le **AWS Skill Builder Learning Plan** « Exam Prep – AWS Certified Security – Specialty (SCS-C03) ».
- [ ] Lire **AWS Well-Architected Security Pillar** + **AWS Security Reference Architecture (AWS SRA)**.
- [ ] Réviser SAA-C03 si rouille (VPC, EC2, S3, IAM, RDS).
- [ ] Démarrer un **glossaire** : KMS, CMK, SCP, OCSF, Bedrock Guardrails, etc.

**Livrable :** environnement de lab opérationnel + glossaire.

---

### Phase 2 — IAM & Identity (Semaines 3-4) — **20 %, le plus lourd**

> ⚠️ Domaine désormais N°1. Investissez du temps de qualité ici.

**Théorie :**

- IAM users, groups, roles, policies (managed vs inline, customer vs AWS).
- **Policy evaluation logic** : Deny explicite, Allow, conditions, ordre d'évaluation cross-account.
- **Permissions boundaries** vs **SCPs** vs **session policies** (différences précises !).
- **STS** : `AssumeRole`, `AssumeRoleWithSAML`, `AssumeRoleWithWebIdentity`, federation.
- **IAM Identity Center** (ex-AWS SSO) : permission sets, multi-account access, IdP externes.
- **AWS Organizations + SCPs** : héritage, allow-list vs deny-list, RCPs (Resource Control Policies, nouveauté).
- **Cognito** : User Pools vs Identity Pools (Federated Identities).
- **IAM Access Analyzer** : findings cross-account, **unused access analyzer**, custom policy checks.
- **Service-Linked Roles**, **Instance Profiles**.
- **IAM Roles Anywhere** : workloads on-prem avec certificats X.509.
- **AWS Verified Access** (Zero Trust pour apps internes, alternative VPN).
- **AWS Verified Permissions** (Cedar, autorisation fine pour vos apps).
- **ABAC** (Attribute-Based Access Control) avec tags.

**Labs :**

- [ ] Écrire une **IAM policy** avec conditions `aws:SourceIp`, `aws:MultiFactorAuthPresent`, `aws:RequestedRegion`, `aws:PrincipalTag`.
- [ ] Mettre une **Permissions Boundary** sur un user et tester la limitation.
- [ ] Créer une Organization avec OUs + SCP qui interdit `ec2:RunInstances` hors `eu-west-1`.
- [ ] Configurer une **RCP** (Resource Control Policy) au niveau OU pour interdire l'accès public S3.
- [ ] Configurer **IAM Identity Center** avec un IdP externe.
- [ ] Activer **IAM Access Analyzer (unused access)** et lire les findings.
- [ ] Déployer **Verified Access** pour publier une app interne sans VPN.

**Pièges :** différencier **Permissions Boundary** vs **SCP** vs **RCP**, ordre d'évaluation policies cross-account, Cedar / Verified Permissions.

---

### Phase 3 — Infrastructure Security (Semaines 5-6) — 18 %

**Théorie — Réseau :**

- **VPC** : subnets, route tables, IGW, NAT GW, peering, **Transit Gateway**, **Cloud WAN**.
- **Security Groups** (stateful) vs **NACLs** (stateless).
- **VPC Endpoints** : Gateway (S3, DynamoDB) vs Interface (PrivateLink), endpoint policies.
- **AWS Network Firewall** : règles Suricata managées, **TLS inspection** (nouveauté).
- **Route 53 Resolver DNS Firewall** : block-list domaines malveillants.
- **VPC Flow Logs**, **VPC Traffic Mirroring** pour analyse de paquets.

**Théorie — Edge / App :**

- **AWS WAF v2** : règles managées, Bot Control, **OWASP Top 10** (très demandé), rate-based, **CAPTCHA / Challenge** actions.
- **AWS Shield** Standard vs Advanced, **Shield Response Team**, cost protection.
- **CloudFront** + **Origin Access Control (OAC)** (remplace OAI), signed URLs/cookies.
- **ACM** + intégration ALB/CloudFront/API Gateway, **ACM Private CA**.
- **API Gateway** : resource policies, IAM auth, Cognito auth, Lambda authorizers.

**Théorie — Compute :**

- EC2 hardening : **Systems Manager** (Patch, Session Manager, Run Command).
- **IMDSv2** (token-based) — **obligatoire** sur les nouveaux comptes, à imposer via SCP.
- **Inspector v2** : EC2, ECR, Lambda + **agentless EC2 scan**.
- Containers : **ECR scanning enhanced** (avec Inspector), **EKS Pod Identity** (remplace IRSA progressivement), network policies.

**Théorie — IA générative (NOUVEAU C03) :**

- **Amazon Bedrock Guardrails** : filtres de contenu, denied topics, sensitive info filters, contextual grounding checks.
- Sécurité **Bedrock** : VPC endpoints, KMS pour modèles fine-tunés, IAM pour invoke.
- Logs Bedrock invocation → CloudWatch / S3.

**Labs :**

- [ ] Architecture 3-tier avec SG, NACL, VPC Endpoints S3.
- [ ] **AWS WAF** sur ALB avec Core Rule Set + Bot Control + CAPTCHA.
- [ ] **Network Firewall** dans un VPC inspection avec TLS inspection.
- [ ] Imposer **IMDSv2** via SCP et tester.
- [ ] **Inspector v2** scan agentless + lire findings.
- [ ] **Bedrock Guardrail** : créer une guardrail avec denied topics + tester sur un modèle.

---

### Phase 4 — Data Protection (Semaines 7-8) — 18 %

**Théorie — KMS & secrets :**

- **AWS KMS** : Customer Managed Keys, AWS Managed, AWS Owned.
- **Key policies** vs **IAM** vs **grants** (logique d'évaluation).
- Symmetric / asymmetric / **HMAC** keys, **multi-Region keys**, **External Key Stores (XKS)**.
- Cross-account KMS, automatic key rotation (1 an par défaut).
- **Envelope encryption**.
- **CloudHSM** : FIPS 140-2 niveau 3, cas d'usage.
- **Secrets Manager** vs **SSM Parameter Store** : rotation, coût, scoping.
- **ACM** public, **ACM Private CA**, renouvellement auto.

**Théorie — Stockage :**

- **S3** : SSE-S3, SSE-KMS, **DSSE-KMS** (double layer), SSE-C, **S3 Bucket Keys**.
- **S3 Block Public Access** (4 settings — à connaître par cœur).
- **S3 Object Ownership**, **Bucket Policies**, **Access Points**, **Multi-Region Access Points**.
- **S3 Object Lock** (Compliance vs Governance), **MFA Delete**.
- **EBS** encryption par défaut, snapshots cross-account chiffrés.
- **EFS / FSx** encryption at rest et in transit.
- **DynamoDB** encryption (default, AWS owned, AWS managed, CMK).
- **RDS / Aurora** : encryption at rest (KMS), TLS in transit, **IAM DB authentication**, audit logs, RDS Proxy.

**Théorie — Découverte & classification :**

- **Amazon Macie** : découverte de PII dans S3, jobs, **custom data identifiers**, allow lists.

**Labs :**

- [ ] **CMK avec rotation** + **key policy cross-account**.
- [ ] Bucket S3 avec SSE-KMS + Bucket Policy `aws:SecureTransport` + Block Public Access.
- [ ] **S3 Object Lock** mode Compliance + tester l'impossibilité de suppression.
- [ ] **Macie** sur un bucket avec données PII factices.
- [ ] **Secrets Manager** avec rotation Lambda automatique pour un mot de passe RDS.
- [ ] **DSSE-KMS** sur un bucket et observer la différence dans les events CloudTrail.

**Pièges :** SSE-S3 vs SSE-KMS vs DSSE-KMS vs SSE-C, deny dans key policies peut bloquer admins, **MFA Delete** seulement via root + CLI.

---

### Phase 5 — Detection + Incident Response (Semaines 9-10) — **30 %** combiné

> ⚠️ Ces deux domaines sont maintenant séparés (16 % + 14 %). Beaucoup de questions au total.

**Théorie — Detection (16 %) :**

- **CloudTrail** : management/data/Insights events, **CloudTrail Lake** (queries SQL sur les logs).
- Trail multi-Region, **Organization trail**, log file integrity validation.
- **CloudWatch Logs** : Log Groups, retention, metric filters, subscription filters.
- **VPC Flow Logs**, **Route 53 query logs**.
- **AWS Config** : rules managed/custom, **conformance packs**, **proactive evaluation** (nouveauté).
- **GuardDuty** : findings types (Recon, UnauthorizedAccess, Backdoor, CryptoCurrency, Trojan), **Malware Protection**, **EKS audit logs**, **RDS Protection**, **S3 Protection**, **Lambda Protection**, **Runtime Monitoring** (eBPF).
- **Security Hub** : standards (FSBP, CIS, PCI, NIST), agrégation cross-account, custom insights.
- **Detective** : graph d'enquête, finding groups.
- **Inspector v2** : CVE, SBOM export.
- **Trusted Advisor** : security checks (selon support level).
- **NOUVEAU C03 — Amazon Security Lake** : data lake centralisé, **format OCSF** (Open Cybersecurity Schema Framework), query via Athena, partage avec partenaires.

**Théorie — Incident Response (14 %) :**

- **EventBridge rules** → SNS / Lambda / SSM Automation pour remédiation auto.
- **SSM Incident Manager** : response plans, runbooks, contacts, escalation chains, post-incident analysis.
- **Forensics EC2** : isolation (SG quarantine), snapshot EBS, memory capture via SSM.
- **Compromised IAM credentials** runbook : disable access keys, force MFA reset, rotation.
- **Compromised S3** : versioning + Object Lock pour restauration.
- **Step Functions** pour workflows de réponse complexes.
- **AWS Customer Incident Response Team (CIRT)**, AWS Abuse.

**Labs :**

- [ ] **GuardDuty + Security Hub + Macie** dans une Organization, compte délégué d'administration.
- [ ] **EventBridge rule** : finding GuardDuty (UnauthorizedAccess) → Lambda qui isole l'EC2 dans un SG « quarantine ».
- [ ] **AWS Config** + conformance pack CIS + remédiation auto pour S3 public.
- [ ] **Security Lake** : activer, ingérer CloudTrail + VPC Flow Logs en OCSF, requêter avec Athena.
- [ ] **SSM Incident Manager** : créer un response plan avec runbook automatique de capture forensique.
- [ ] **CloudTrail Lake** : query SQL pour trouver les `RootLogin` des 30 derniers jours.

---

### Phase 6 — Foundations, Governance & Practice (Semaines 11-12) — 14 % + révisions

**Théorie — Governance :**

- **AWS Organizations** : OUs, SCPs (deny-list), **RCPs** (Resource Control Policies, nouveau), Tag policies.
- **AWS Control Tower** : landing zone, guardrails (preventive vs detective), Account Factory.
- **AWS Config aggregator**, **Audit Manager** (frameworks SOC2, HIPAA, GDPR), **Artifact** (rapports).
- **AWS RAM** (Resource Access Manager) : partage cross-account.
- **Service Catalog** + SCPs pour standardiser les déploiements.
- Tag strategies (compliance, cost, ABAC).
- **AWS Backup** + cross-account, cross-Region, vault lock.

**Practice exams — viser ≥ 80 % avant le J :**

- [ ] **Tutorials Dojo (Jon Bonso) — SCS-C03 set** : référence numéro 1.
- [ ] **AWS Skill Builder Official Practice Question Set** (SCS-C03).
- [ ] **AWS Official Practice Exam SCS-C03** (Pearson VUE, ~50 USD).
- [ ] **Whizlabs**, **ExamPro** : compléments.
- [ ] **S'entraîner aux nouveaux formats** : ordering questions et matching questions (Tutorials Dojo en propose).
- [ ] Pour chaque erreur : note dans le glossaire, refaire le lab AWS associé.
- [ ] **Réserver l'examen Pearson VUE** ~2 semaines avant la date cible.
- [ ] La veille : repos, lecture du glossaire, pas de bachotage tardif.

---

## 4. Ressources de référence

**Officielles (gratuites)**

- **Exam Guide SCS-C03** : https://docs.aws.amazon.com/aws-certification/latest/security-specialty-03/security-specialty-03.html
- **Exam Guide PDF** : https://d1.awsstatic.com/onedam/marketing-channels/website/aws/en_US/certification/approved/pdfs/docs-security-spec/AWS-Certified-Security-Specialty_Exam-Guide-03.pdf
- **AWS Skill Builder Learning Plan SCS-C03** (modules + practice questions).
- **AWS Well-Architected Security Pillar** whitepaper.
- **AWS Security Reference Architecture (AWS SRA)**.
- **AWS re:Inforce 2025 / re:Invent 2025** sessions YouTube (security track).

**Cours payants (excellent ROI)**

- **Adrian Cantrill** — _AWS Certified Security – Specialty_ (très complet, attendre la mise à jour SCS-C03 si pas déjà publiée).
- **Stephane Maarek** (Udemy) — synthétique, version SCS-C03.
- **Tutorials Dojo Cheat Sheets**.

**Practice tests**

- **Tutorials Dojo (Jon Bonso) — SCS-C03** : référence absolue.
- **AWS Official Practice Exam SCS-C03**.
- **Whizlabs SCS-C03**, **ExamPro**.

**Hands-on**

- **AWS Workshops** (workshops.aws) — section Security.
- **CloudGoat** (Rhino Security) — CTF attaque/défense.
- **AWS Jam** (re:Inforce, re:Invent replays).

---

## 5. Conseils stratégiques spécifiques SCS-C03

1. **IAM = priorité absolue** : 20 %. Maîtrisez policies, conditions, evaluation logic, **Permissions Boundary vs SCP vs RCP**, IAM Identity Center, Roles Anywhere, Verified Access.
2. **OCSF + Security Lake** : sujet nouveau et **chouchou** des examinateurs en 2026.
3. **Bedrock Guardrails** : à connaître pour les nouvelles questions sur sécurité IA.
4. **Nouveaux formats** : entraînez-vous aux questions **ordering** (étapes dans l'ordre — souvent runbooks d'incident) et **matching** (associer service ↔ rôle ↔ usage).
5. **GuardDuty Runtime Monitoring** : nouveau, à savoir reconnaître.
6. **170 minutes pour ~65 questions ≈ 2,5 min/question** : marquez et passez les questions floues.
7. **Lire la question avant le scénario** sur les questions longues — gagne du temps.
8. **Pas de calcul** : surtout des choix d'architecture et de logique de policy.

---

## 6. Suivi des phases

Phases 1 à 6 mises à jour dans votre liste de tâches Cowork (#12 à #17 — désormais étiquetées SCS-C03).

Bon courage ! 🛡️

# Roadmap de préparation — AWS Certified Security – Specialty (SCS-C03)

**Pour :** Michel **Date de départ :** 29 avril 2026 **Cible :** réussir l'examen **SCS-C03** (lancé décembre 2025) — 750/1000 minimum, 170 min, ~65 questions

> 📌 **Nouveautés clés C02 → C03**
> 
> - **IAM passe de 16 % à 20 %** — c'est désormais **le domaine le plus lourd** (avant c'était Infrastructure Security).
> - **Detection et Incident Response sont maintenant deux domaines distincts** (avant : un seul à 14 %).
> - **Nouveaux formats de questions** : **ordering** (mettre des étapes dans l'ordre) et **matching** (associer des éléments).
> - **Nouveaux sujets 2026** : logging aligné **OCSF** (Open Cybersecurity Schema Framework) avec **Security Lake**, **guardrails pour l'IA générative** (Bedrock Guardrails, sécurité Bedrock).

---

## 1. Domaines & pondérations (SCS-C03)

|#|Domaine|Pondération|
|---|---|---|
|1|**Detection**|16 %|
|2|**Incident Response**|14 %|
|3|**Infrastructure Security**|18 %|
|4|**Identity and Access Management**|**20 %** ⭐|
|5|**Data Protection**|18 %|
|6|**Security Foundations and Governance**|14 %|

➡️ **IAM + Data Protection + Infrastructure Security = 56 %** : c'est votre cœur de cible. ➡️ **Detection + Incident Response = 30 %** : ne les négligez surtout pas, ils sont maintenant séparés et pèsent plus qu'avant.

---

## 2. Pré-requis recommandés

- **Solutions Architect Associate (SAA-C03)** ou **SysOps Administrator** validé, ou expérience équivalente.
- Bases solides : VPC, EC2, IAM, S3, CloudTrail, CloudWatch.
- 3-5 ans d'expérience IT, dont 2 ans en sécurité AWS recommandés par AWS.
- **Compte AWS** avec budget mensuel ~20-30 USD pour les labs (alarmes de coût configurées !).
- Outils : **AWS CLI v2**, **CloudShell**, **VS Code** + extensions AWS, **CDK** ou **Terraform**.

---

## 3. Plan en 12 semaines (≈ 8–10 h/semaine)

### Phase 1 — Fondations & setup (Semaines 1-2)

- [ ] Lire l'**Exam Guide SCS-C03** officiel en entier (PDF AWS).
- [ ] Créer un compte AWS dédié (séparé de la prod), activer **MFA root**, créer un IAM admin et désactiver l'usage du root.
- [ ] Configurer **AWS Budgets** + alertes de coût (10/20/30 USD).
- [ ] Installer AWS CLI v2, configurer profils.
- [ ] Suivre le **AWS Skill Builder Learning Plan** « Exam Prep – AWS Certified Security – Specialty (SCS-C03) ».
- [ ] Lire **AWS Well-Architected Security Pillar** + **AWS Security Reference Architecture (AWS SRA)**.
- [ ] Réviser SAA-C03 si rouille (VPC, EC2, S3, IAM, RDS).
- [ ] Démarrer un **glossaire** : KMS, CMK, SCP, OCSF, Bedrock Guardrails, etc.

**Livrable :** environnement de lab opérationnel + glossaire.

---

### Phase 2 — IAM & Identity (Semaines 3-4) — **20 %, le plus lourd**

> ⚠️ Domaine désormais N°1. Investissez du temps de qualité ici.

**Théorie :**

- IAM users, groups, roles, policies (managed vs inline, customer vs AWS).
- **Policy evaluation logic** : Deny explicite, Allow, conditions, ordre d'évaluation cross-account.
- **Permissions boundaries** vs **SCPs** vs **session policies** (différences précises !).
- **STS** : `AssumeRole`, `AssumeRoleWithSAML`, `AssumeRoleWithWebIdentity`, federation.
- **IAM Identity Center** (ex-AWS SSO) : permission sets, multi-account access, IdP externes.
- **AWS Organizations + SCPs** : héritage, allow-list vs deny-list, RCPs (Resource Control Policies, nouveauté).
- **Cognito** : User Pools vs Identity Pools (Federated Identities).
- **IAM Access Analyzer** : findings cross-account, **unused access analyzer**, custom policy checks.
- **Service-Linked Roles**, **Instance Profiles**.
- **IAM Roles Anywhere** : workloads on-prem avec certificats X.509.
- **AWS Verified Access** (Zero Trust pour apps internes, alternative VPN).
- **AWS Verified Permissions** (Cedar, autorisation fine pour vos apps).
- **ABAC** (Attribute-Based Access Control) avec tags.

**Labs :**

- [ ] Écrire une **IAM policy** avec conditions `aws:SourceIp`, `aws:MultiFactorAuthPresent`, `aws:RequestedRegion`, `aws:PrincipalTag`.
- [ ] Mettre une **Permissions Boundary** sur un user et tester la limitation.
- [ ] Créer une Organization avec OUs + SCP qui interdit `ec2:RunInstances` hors `eu-west-1`.
- [ ] Configurer une **RCP** (Resource Control Policy) au niveau OU pour interdire l'accès public S3.
- [ ] Configurer **IAM Identity Center** avec un IdP externe.
- [ ] Activer **IAM Access Analyzer (unused access)** et lire les findings.
- [ ] Déployer **Verified Access** pour publier une app interne sans VPN.

**Pièges :** différencier **Permissions Boundary** vs **SCP** vs **RCP**, ordre d'évaluation policies cross-account, Cedar / Verified Permissions.

---

### Phase 3 — Infrastructure Security (Semaines 5-6) — 18 %

**Théorie — Réseau :**

- **VPC** : subnets, route tables, IGW, NAT GW, peering, **Transit Gateway**, **Cloud WAN**.
- **Security Groups** (stateful) vs **NACLs** (stateless).
- **VPC Endpoints** : Gateway (S3, DynamoDB) vs Interface (PrivateLink), endpoint policies.
- **AWS Network Firewall** : règles Suricata managées, **TLS inspection** (nouveauté).
- **Route 53 Resolver DNS Firewall** : block-list domaines malveillants.
- **VPC Flow Logs**, **VPC Traffic Mirroring** pour analyse de paquets.

**Théorie — Edge / App :**

- **AWS WAF v2** : règles managées, Bot Control, **OWASP Top 10** (très demandé), rate-based, **CAPTCHA / Challenge** actions.
- **AWS Shield** Standard vs Advanced, **Shield Response Team**, cost protection.
- **CloudFront** + **Origin Access Control (OAC)** (remplace OAI), signed URLs/cookies.
- **ACM** + intégration ALB/CloudFront/API Gateway, **ACM Private CA**.
- **API Gateway** : resource policies, IAM auth, Cognito auth, Lambda authorizers.

**Théorie — Compute :**

- EC2 hardening : **Systems Manager** (Patch, Session Manager, Run Command).
- **IMDSv2** (token-based) — **obligatoire** sur les nouveaux comptes, à imposer via SCP.
- **Inspector v2** : EC2, ECR, Lambda + **agentless EC2 scan**.
- Containers : **ECR scanning enhanced** (avec Inspector), **EKS Pod Identity** (remplace IRSA progressivement), network policies.

**Théorie — IA générative (NOUVEAU C03) :**

- **Amazon Bedrock Guardrails** : filtres de contenu, denied topics, sensitive info filters, contextual grounding checks.
- Sécurité **Bedrock** : VPC endpoints, KMS pour modèles fine-tunés, IAM pour invoke.
- Logs Bedrock invocation → CloudWatch / S3.

**Labs :**

- [ ] Architecture 3-tier avec SG, NACL, VPC Endpoints S3.
- [ ] **AWS WAF** sur ALB avec Core Rule Set + Bot Control + CAPTCHA.
- [ ] **Network Firewall** dans un VPC inspection avec TLS inspection.
- [ ] Imposer **IMDSv2** via SCP et tester.
- [ ] **Inspector v2** scan agentless + lire findings.
- [ ] **Bedrock Guardrail** : créer une guardrail avec denied topics + tester sur un modèle.

---

### Phase 4 — Data Protection (Semaines 7-8) — 18 %

**Théorie — KMS & secrets :**

- **AWS KMS** : Customer Managed Keys, AWS Managed, AWS Owned.
- **Key policies** vs **IAM** vs **grants** (logique d'évaluation).
- Symmetric / asymmetric / **HMAC** keys, **multi-Region keys**, **External Key Stores (XKS)**.
- Cross-account KMS, automatic key rotation (1 an par défaut).
- **Envelope encryption**.
- **CloudHSM** : FIPS 140-2 niveau 3, cas d'usage.
- **Secrets Manager** vs **SSM Parameter Store** : rotation, coût, scoping.
- **ACM** public, **ACM Private CA**, renouvellement auto.

**Théorie — Stockage :**

- **S3** : SSE-S3, SSE-KMS, **DSSE-KMS** (double layer), SSE-C, **S3 Bucket Keys**.
- **S3 Block Public Access** (4 settings — à connaître par cœur).
- **S3 Object Ownership**, **Bucket Policies**, **Access Points**, **Multi-Region Access Points**.
- **S3 Object Lock** (Compliance vs Governance), **MFA Delete**.
- **EBS** encryption par défaut, snapshots cross-account chiffrés.
- **EFS / FSx** encryption at rest et in transit.
- **DynamoDB** encryption (default, AWS owned, AWS managed, CMK).
- **RDS / Aurora** : encryption at rest (KMS), TLS in transit, **IAM DB authentication**, audit logs, RDS Proxy.

**Théorie — Découverte & classification :**

- **Amazon Macie** : découverte de PII dans S3, jobs, **custom data identifiers**, allow lists.

**Labs :**

- [ ] **CMK avec rotation** + **key policy cross-account**.
- [ ] Bucket S3 avec SSE-KMS + Bucket Policy `aws:SecureTransport` + Block Public Access.
- [ ] **S3 Object Lock** mode Compliance + tester l'impossibilité de suppression.
- [ ] **Macie** sur un bucket avec données PII factices.
- [ ] **Secrets Manager** avec rotation Lambda automatique pour un mot de passe RDS.
- [ ] **DSSE-KMS** sur un bucket et observer la différence dans les events CloudTrail.

**Pièges :** SSE-S3 vs SSE-KMS vs DSSE-KMS vs SSE-C, deny dans key policies peut bloquer admins, **MFA Delete** seulement via root + CLI.

---

### Phase 5 — Detection + Incident Response (Semaines 9-10) — **30 %** combiné

> ⚠️ Ces deux domaines sont maintenant séparés (16 % + 14 %). Beaucoup de questions au total.

**Théorie — Detection (16 %) :**

- **CloudTrail** : management/data/Insights events, **CloudTrail Lake** (queries SQL sur les logs).
- Trail multi-Region, **Organization trail**, log file integrity validation.
- **CloudWatch Logs** : Log Groups, retention, metric filters, subscription filters.
- **VPC Flow Logs**, **Route 53 query logs**.
- **AWS Config** : rules managed/custom, **conformance packs**, **proactive evaluation** (nouveauté).
- **GuardDuty** : findings types (Recon, UnauthorizedAccess, Backdoor, CryptoCurrency, Trojan), **Malware Protection**, **EKS audit logs**, **RDS Protection**, **S3 Protection**, **Lambda Protection**, **Runtime Monitoring** (eBPF).
- **Security Hub** : standards (FSBP, CIS, PCI, NIST), agrégation cross-account, custom insights.
- **Detective** : graph d'enquête, finding groups.
- **Inspector v2** : CVE, SBOM export.
- **Trusted Advisor** : security checks (selon support level).
- **NOUVEAU C03 — Amazon Security Lake** : data lake centralisé, **format OCSF** (Open Cybersecurity Schema Framework), query via Athena, partage avec partenaires.

**Théorie — Incident Response (14 %) :**

- **EventBridge rules** → SNS / Lambda / SSM Automation pour remédiation auto.
- **SSM Incident Manager** : response plans, runbooks, contacts, escalation chains, post-incident analysis.
- **Forensics EC2** : isolation (SG quarantine), snapshot EBS, memory capture via SSM.
- **Compromised IAM credentials** runbook : disable access keys, force MFA reset, rotation.
- **Compromised S3** : versioning + Object Lock pour restauration.
- **Step Functions** pour workflows de réponse complexes.
- **AWS Customer Incident Response Team (CIRT)**, AWS Abuse.

**Labs :**

- [ ] **GuardDuty + Security Hub + Macie** dans une Organization, compte délégué d'administration.
- [ ] **EventBridge rule** : finding GuardDuty (UnauthorizedAccess) → Lambda qui isole l'EC2 dans un SG « quarantine ».
- [ ] **AWS Config** + conformance pack CIS + remédiation auto pour S3 public.
- [ ] **Security Lake** : activer, ingérer CloudTrail + VPC Flow Logs en OCSF, requêter avec Athena.
- [ ] **SSM Incident Manager** : créer un response plan avec runbook automatique de capture forensique.
- [ ] **CloudTrail Lake** : query SQL pour trouver les `RootLogin` des 30 derniers jours.

---

### Phase 6 — Foundations, Governance & Practice (Semaines 11-12) — 14 % + révisions

**Théorie — Governance :**

- **AWS Organizations** : OUs, SCPs (deny-list), **RCPs** (Resource Control Policies, nouveau), Tag policies.
- **AWS Control Tower** : landing zone, guardrails (preventive vs detective), Account Factory.
- **AWS Config aggregator**, **Audit Manager** (frameworks SOC2, HIPAA, GDPR), **Artifact** (rapports).
- **AWS RAM** (Resource Access Manager) : partage cross-account.
- **Service Catalog** + SCPs pour standardiser les déploiements.
- Tag strategies (compliance, cost, ABAC).
- **AWS Backup** + cross-account, cross-Region, vault lock.

**Practice exams — viser ≥ 80 % avant le J :**

- [ ] **Tutorials Dojo (Jon Bonso) — SCS-C03 set** : référence numéro 1.
- [ ] **AWS Skill Builder Official Practice Question Set** (SCS-C03).
- [ ] **AWS Official Practice Exam SCS-C03** (Pearson VUE, ~50 USD).
- [ ] **Whizlabs**, **ExamPro** : compléments.
- [ ] **S'entraîner aux nouveaux formats** : ordering questions et matching questions (Tutorials Dojo en propose).
- [ ] Pour chaque erreur : note dans le glossaire, refaire le lab AWS associé.
- [ ] **Réserver l'examen Pearson VUE** ~2 semaines avant la date cible.
- [ ] La veille : repos, lecture du glossaire, pas de bachotage tardif.

---

## 4. Ressources de référence

**Officielles (gratuites)**

- **Exam Guide SCS-C03** : https://docs.aws.amazon.com/aws-certification/latest/security-specialty-03/security-specialty-03.html
- **Exam Guide PDF** : https://d1.awsstatic.com/onedam/marketing-channels/website/aws/en_US/certification/approved/pdfs/docs-security-spec/AWS-Certified-Security-Specialty_Exam-Guide-03.pdf
- **AWS Skill Builder Learning Plan SCS-C03** (modules + practice questions).
- **AWS Well-Architected Security Pillar** whitepaper.
- **AWS Security Reference Architecture (AWS SRA)**.
- **AWS re:Inforce 2025 / re:Invent 2025** sessions YouTube (security track).

**Cours payants (excellent ROI)**

- **Adrian Cantrill** — _AWS Certified Security – Specialty_ (très complet, attendre la mise à jour SCS-C03 si pas déjà publiée).
- **Stephane Maarek** (Udemy) — synthétique, version SCS-C03.
- **Tutorials Dojo Cheat Sheets**.

**Practice tests**

- **Tutorials Dojo (Jon Bonso) — SCS-C03** : référence absolue.
- **AWS Official Practice Exam SCS-C03**.
- **Whizlabs SCS-C03**, **ExamPro**.

**Hands-on**

- **AWS Workshops** (workshops.aws) — section Security.
- **CloudGoat** (Rhino Security) — CTF attaque/défense.
- **AWS Jam** (re:Inforce, re:Invent replays).

---

## 5. Conseils stratégiques spécifiques SCS-C03

1. **IAM = priorité absolue** : 20 %. Maîtrisez policies, conditions, evaluation logic, **Permissions Boundary vs SCP vs RCP**, IAM Identity Center, Roles Anywhere, Verified Access.
2. **OCSF + Security Lake** : sujet nouveau et **chouchou** des examinateurs en 2026.
3. **Bedrock Guardrails** : à connaître pour les nouvelles questions sur sécurité IA.
4. **Nouveaux formats** : entraînez-vous aux questions **ordering** (étapes dans l'ordre — souvent runbooks d'incident) et **matching** (associer service ↔ rôle ↔ usage).
5. **GuardDuty Runtime Monitoring** : nouveau, à savoir reconnaître.
6. **170 minutes pour ~65 questions ≈ 2,5 min/question** : marquez et passez les questions floues.
7. **Lire la question avant le scénario** sur les questions longues — gagne du temps.
8. **Pas de calcul** : surtout des choix d'architecture et de logique de policy.

---

## 6. Suivi des phases

Phases 1 à 6 mises à jour dans votre liste de tâches Cowork (#12 à #17 — désormais étiquetées SCS-C03).

Bon courage ! 🛡️