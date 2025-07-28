# Architecture AV/EDR
### **Détection basée sur la signature**
La détection basée sur les signatures s’appuie sur des modèles connus de code malveillant (chaînes binaires, hachages ou séquences d’instructions spécifiques) pour identifier les menaces.

**Techniques d'évasion :**

- Réencodage ou cryptage du shellcode.
- Enveloppement de stub pour générer de nouveaux hachages.
- Utilisation de packers ou de crypteurs.

### **Détection heuristique**
La détection heuristique implique des vérifications statiques et dynamiques basées sur des règles sur le code, à la recherche d'attributs ou de comportements suspects.
**Comment ça marche :**

- Utilise des règles prédéfinies ou des vérifications de modèles de type YARA.
- Recherche des constructions suspectes :
    - Utilisation de l'API (par exemple, `VirtualAllocEx`, `WriteProcessMemory`).
    - Noms de section ou autorisations anormaux ( `.text`marqués comme RWX).
    - Entropie élevée suggérant un cryptage ou un emballage.

**Exemples :**

- Fichier PE avec une petite `.text`section et une grande `.data`section.
- Utilisation de `IsDebuggerPresent()`l'API.
- En-têtes de fichiers non standard ou métadonnées PE modifiées.

**Techniques d'évasion :**

- Rembourrage des charges utiles pour réduire l'entropie.
- Obfuscation des appels d'API ou retard de chargement.
- Diviser la logique malveillante en plusieurs étapes.

### **Détection comportementale**
La détection comportementale surveille le comportement d’exécution des processus et le corrèle avec des modèles malveillants connus.

### **Détection basée sur l'apprentissage automatique (ML)**

La détection ML implique la formation de modèles sur de grands ensembles de données de comportements bénins et malveillants pour identifier des anomalies ou des modèles de type malware.

## Composant principal d'un EDR
### **Capteur**
Le capteur est le composant logiciel léger installé sur le point de terminaison qui collecte la télémétrie et surveille le comportement en temps réel.
### **Kernel Driver**
Le pilote fonctionne dans **l'anneau 0** , offrant une intégration profonde avec le noyau du système d'exploitation pour surveiller les opérations de bas niveau.
### **Userland Components**
Ces composants fonctionnent en mode utilisateur et s'interfacent directement avec les API et les applications du système d'exploitation.
### **Backend (On-Premise or Hybrid)**
Le backend est l’infrastructure qui reçoit et traite la télémétrie de tous les points de terminaison de l’environnement.
### **Cloud Component**
Les solutions EDR modernes utilisent une infrastructure cloud pour centraliser l'intelligence, l'analyse et les mises à jour.
Pour contourner la surveillance des appels API/système :

#### **Stratégies d'évasion du Userland** :

- **Mappage manuel** : chargez les DLL manuellement pour éviter `LoadLibrary()`la journalisation IAT.
- **Appels système directs** : utilisez des outils tels que `SysWhispers`ou `Hell’s Gate`pour appeler directement les appels système.
- **Appels système indirects** : utilisez des gadgets ou des fonctions stub pour masquer la pile d'appels.

#### **Évasion au niveau du noyau (avancé)** :

- **Syscall Stomping** : écrasez les instructions d'appel système pour rediriger vers des appels système alternatifs.
- **Contournement ETW** : corrigez ou désactivez les consommateurs ETW comme `EtwEventWrite`.
- **Suppression des rappels** : désenregistrez les rappels du noyau (nécessite un pilote en mode noyau).
## Hooking
Le hooking consiste à intercepter ou à rediriger des appels de fonction ou des événements système. Il existe deux principaux niveaux de hooking selon son implémentation : **le mode utilisateur (Userland)** et **le mode noyau (Kernelland)** .

### **Présentation de l'architecture d'exécution de Windows**

Avant de plonger dans le hooking, il est essentiel de comprendre que Windows possède une architecture en couches :

- **Mode utilisateur** : où s'exécutent les applications standard. Accès limité aux ressources système.
- **Mode noyau** : le cœur du système d'exploitation (le noyau Windows) et les pilotes de périphériques. Son accès est illimité.
