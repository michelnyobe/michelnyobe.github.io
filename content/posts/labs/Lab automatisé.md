**Infrastructure as Code (IaC)** — le cœur de l'automatisation. **Terraform** (ou OpenTofu) pour provisionner/détruire, **Ansible** pour configurer les machines, **Packer** pour fabriquer des images-templates prêtes à cloner.
- **Orchestration & développement backend** — c'est ce qui transforme le « bouton » en actions. Une API + une file d'attente de tâches (Celery, BullMQ, ou **Temporal** pour les workflows longs), qui déclenche un `terraform apply`, enregistre une échéance, et planifie le `terraform destroy`. C'est là que vit la logique « TTL 48 h ».
**Réseau & sécurité réseau** — segmentation stricte, un VPN par utilisateur (**WireGuard** ou OpenVPN), et surtout l'**isolation multi-tenant** : un étudiant ne doit jamais pouvoir atteindre le lab d'un autre ni votre infrastructure.

**Active Directory & Windows** — savoir construire des AD volontairement vulnérables (mauvaises ACL, Kerberoasting, délégations, chemins d'attaque BloodHound).

**Sécurité offensive (le contenu)** — concevoir les scénarios, vulnérabilités web (OWASP), chaînes d'exploitation, et poser les « flags » à capturer.

