---
title: "Key Vault"
date: 2025-06-30
draft: false
tags: ["azure", "security", "key vault"]
categories: ["azure"]
summary: "key vault"
showToc: true
tocOpen: true
---

## Qu’est-ce qu’Azure Key Vault ?

Azure Key Vault est un service cloud permettant de stocker les secrets et d’y accéder en toute sécurité.Il peut s’agir de:
- **Les secrets** : mots de passe, chaînes de connexion, jetons, etc.
- **Les clés de chiffrement** : pour chiffrer des données dans d'autres services.
- **Les certificats** : certificats SSL/TLS utilisés dans des services Azure. 
- **Les modules HSM (Hardware Security Modules)** : stocker les clés dans du matériel sécurisé.
son objectif  est **d'éviter d'intégrer des données directement dans le code source d'un programme ou d'un autre objet exécutable les fichiers de configuration.**

## cas d'tilisation de Azure Key Vault

La centralisation du stockage des secrets d’application dans Azure Key Vault vous permet de contrôler la distribution de ces secrets. Key Vault réduit considérablement les risques de fuite accidentelle des secrets. Lorsque les développeurs d’applications utilisent Key Vault, ils n’ont plus besoin de stocker des informations de sécurité dans leur application.


**Authentification sécurisée** : Une app web récupère un secret DB via Key Vault
Chiffrement de données : Azure SQL, Blob, ou Disk utilisent une clé stockée dans Key Vault
Rotation automatique : Gestion automatique des certificats SSL
Intégration DevSecOps : Azure DevOps ou GitHub récupèrent des secrets à l'exécution

## Architecture et composants

Le service Key Vault prend en charge deux types de conteneurs : les coffres et les pools HSM (modules de sécurité matériels) managés. Les coffres prennent en charge le stockage des clés logicielles et sauvegardées avec HSM, les secrets et les certificats. Les pools HSM managés prennent uniquement en charge les clés sauvegardées avec HSM.
les secrets et les certificats sont collectivement appelés « objets ».

###  Identificateurs d'objet

Les objets sont identifiés de manière unique dans Key Vault à l’aide d’un identificateur qui ne respecte pas la casse appelé identificateur d’objet. Il n’y a pas deux objets avec le même identificateur dans le système, quel que soit l’emplacement géographique.
L’identificateur est constitué d’un préfixe qui identifie le coffre de clés, du type d’objet, du nom d’objet fourni par l’utilisateur et d’une version d’objet. Les identificateurs qui n’incluent pas la version de l’objet sont appelés des « identificateurs de base ». Les identificateurs d’objet Key Vault sont également des URL valides.

Un identificateur d’objet a le format général suivant (selon le type de conteneur) :

- **Pour les coffres** : `https://{vault-name}.vault.azure.net/{object-type}/{object-name}/{object-version}`
- **Pour les pools Managed HSM** : `https://{hsm-name}.managedhsm.azure.net/{object-type}/{object-name}/{object-version}`

### Structure d’URL de requête

Les opérations de gestion des clés utilisent des verbes HTTP, notamment DELETE, GET, PATCH et PUT. Les opérations de chiffrement sur les objets clés existants utilisent HTTP POST.

Pour les clients qui ne peuvent pas prendre en charge des verbes HTTP spécifiques, Azure Key Vault autorise l’utilisation de HTTP POST avec l’en-tête `X-HTTP-REQUEST` pour spécifier le verbe prévu.

Pour utiliser des objets dans Azure Key Vault, voici des exemples d’URL :

- Pour CRÉER une clé appelée TESTKEY dans un coffre de clés, utilisez : `PUT /keys/TESTKEY?api-version=<api_version> HTTP/1.1`
- Pour IMPORTER une clé appelée IMPORTEDKEY dans un coffre de clés, utilisez - `POST /keys/IMPORTEDKEY/import?api-version=<api_version> HTTP/1.1`
- Pour OBTENIR un secret appelé MYSECRET dans un coffre de clés, utilisez - `GET /secrets/MYSECRET?api-version=<api_version> HTTP/1.1`
- Pour SIGNER un code de hachage utilisant une clé nommée TESTKEY dans Key Vault, utilisez : `POST /keys/TESTKEY/sign?api-version=<api_version> HTTP/1.1`
- L’autorité pour une demande adressée à un Key Vault est toujours la suivante :
    
    - Pour les coffres : `https://{keyvault-name}.vault.azure.net/`
    - Pour les modules HSM managés : `https://{HSM-name}.managedhsm.azure.net/` les clés sont toujours stockées sous le chemin /keys, tandis que les secrets sont toujours stockés sous le chemin /secrets.

#  Options d’authentification Key Vault

Lorsque vous créez un coffre de clés dans un abonnement Azure, il est automatiquement associé au locataire Microsoft Entra de l’abonnement.
 les applications peuvent accéder au coffre de clés de trois manières :
 **Application uniquement** : l’application représente un principal du service ou une identité managée.
 **Utilisateur uniquement** : l’utilisateur accède au coffre de clés à partir de n’importe quelle application inscrite dans le locataire.
 **Application-plus-utilisateur** (parfois appelé _identité composée)_ : l’utilisateur est tenu d’accéder au coffre de clés à partir d’une application spécifique _et_ l’application doit utiliser le flux OBO (Authentification On-Behalf-Of) pour emprunter l’identité de l’utilisateur.

#  modèle d’accès
L’accès à un coffre de clés est contrôlé par le biais de deux interfaces : le **plan de contrôle** et le **plan de données**.
Les deux plans utilisent Microsoft Entra ID pour l’authentification. Pour l’autorisation, le plan de contrôle utilise le contrôle d’accès en fonction du rôle Azure (Azure RBAC) et le plan de données utilise une stratégie d’accès Key Vault et Azure RBAC pour les opérations de plan de données Key Vault.

Key Vault prend en charge les stratégies d’accès conditionnel Microsoft Entra.
Vous pouvez contrôler l’accès aux clés, aux certificats et aux secrets Key Vault en utilisant des stratégies d’accès Azure RBAC ou Key Vault.
 
# créez un coffre de clés dans Azure Key Vault

 Azure Key Vault est un service cloud qui fonctionne comme un magasin des secrets sécurisé. Vous pouvez stocker des clés, des mots de passe, des certificats et d’autres secrets en toute sécurité.
1. Si vous ne disposez pas d’un compte Azure, créez-en un gratuitement avant de commencer.
2. Utilisez l’environnement Bash dans Azure Cloud Shell.  https://shell.azure.com/
Si vous préférez exécuter des commandes de référence CLI localement, installez Azure CLI. Si vous exécutez sur Windows ou macOS, envisagez d’exécuter Azure CLI dans un conteneur Docker.
	- Si vous utilisez une installation locale, connectez-vous à Azure CLI à l’aide de la commande **az login**.
	- Lorsque vous y êtes invité, installez l’extension Azure CLI lors de la première utilisation.
	- Exécutez **az version** pour rechercher la version et les bibliothèques dépendantes installées. Pour effectuer une mise à niveau vers la dernière version, exécutez az upgrade.
3. Créer un groupe de ressources
Un groupe de ressources est un conteneur logique dans lequel les ressources Azure sont déployées et gérées. Utilisez la commande **az group create** pour créer un groupe de ressources nommé _myResourceGroup_ à l’emplacement _eastus_.

```PowerShell
az group create --name "myResourceGroup" --location "EastUS"
```

![groupe_create](/images/20250630012714.png)

4.  Créer un coffre de clés
Utilisez la commande Azure CLI **az keyvault create** pour créer un coffre de clés dans le groupe de ressources de l’étape précédente. Vous devrez fournir certaines informations :
 Nom du coffre de clés : chaîne de 3 à 24 caractères qui ne peut contenir que des chiffres (0-9), des lettres (a-z, A-Z) et des traits d’union (-).
    
     Important
    
    Chaque coffre de clés doit avoir un nom unique. Remplacez <your-unique-keyvault-name> par le nom de votre coffre de clés dans les exemples suivants.
- Groupe de ressources nommé : **myResourceGroup**.
- Localisation : **EastUS**.

```Powershell
az keyvault create --name "<your-unique-keyvault-name>" --resource-group "myResourceGroup"
```

 5.  Donner à votre compte d’utilisateur des autorisations pour gérer les secrets dans Key Vault
 Pour obtenir des autorisations sur votre coffre de clés par le biais du **contrôle d’accès en fonction du rôle (RBAC )**, attribuez un rôle à votre « nom d’utilisateur principal » (UPN) à l’aide de la commande Azure CLI **az role assignment create**.
 
```powershell
az role assignment create --role "Key Vault Secrets Officer" --assignee "<upn>" --scope "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.KeyVault/vaults/<your-unique-keyvault-name>"
```

6.  Ajouter un secret au coffre de clés
Utilisez la commande Azure CLI [az keyvault secret set](https://learn.microsoft.com/fr-fr/cli/azure/keyvault/secret#az-keyvault-secret-set) suivante pour créer une clé secrète dans Key Vault

```powershell
az keyvault secret set --vault-name "<your-unique-keyvault-name>" --name "ExamplePassword" --value "hVFkk965BuUv"
```

7.  Récupérer un secret à partir de Key Vault
Vous pouvez maintenant référencer ce mot de passe que vous avez ajouté à Azure Key Vault à l’aide de son URI. Utilisez **`https://<your-unique-keyvault-name>.vault.azure.net/secrets/ExamplePassword`** pour obtenir la version actuelle.

Pour afficher la valeur contenue dans le secret sous forme de texte brut, utilisez la commande Azure CLI **az keyvault secret show** :

```powershell
az keyvault secret show --name "ExamplePassword" --vault-name "<your-unique-keyvault-name>" --query "value"
```




_Key Vault est conçu pour stocker des secrets de configuration pour des applications serveur._ Il n’est pas destiné au stockage des données appartenant aux utilisateurs de votre application.
Les données utilisateur doivent être stockées ailleurs, par exemple, dans une base de données Azure SQL avec chiffrement transparent des données (Transparent data encryption), ou dans un compte de stockage avec chiffrement du service de stockage (Storage Service Encryption). 

Ressources : 

https://learn.microsoft.com/fr-fr/cli/azure/keyvault?view=azure-cli-latest
https://learn.microsoft.com/fr-fr/training/modules/manage-secrets-with-azure-key-vault/?source=recommendations
https://learn.microsoft.com/fr-fr/azure/key-vault/general/security-features


