# Planifier et implémenter la sécurité pour les réseaux virtuels
# Qu’est-ce qu’un réseau virtuel Azure ?
Le réseau virtuel Azure est un service qui fournit le bloc de construction fondamental de votre réseau privé dans Azure.
### Communiquer entre les ressources Azure
Les ressources Azure communiquent en toute sécurité entre elles de l’une des manières suivantes :

- Réseau virtuel : vous pouvez déployer des machines virtuelles et d’autres types de ressources Azure dans un réseau virtuel. Des exemples de ressources incluent les environnements App Service, Azure Kubernetes Service (AKS) et Microsoft Azure Virtual Machine Scale Sets
### Communiquer avec les ressources locales
- Réseau privé virtuel de point à site (VPN) : établi entre un réseau virtuel et un seul ordinateur de votre réseau.Une connexion par passerelle VPN point à site (P2S) vous permet de créer une connexion sécurisée à votre réseau virtuel à partir d’un ordinateur de client individuel.
- VPN de site à site : établi entre votre appareil VPN local et une passerelle VPN Azure déployée dans un réseau virtuel.
- Azure ExpressRoute : établi entre votre réseau et Azure, via un partenaire ExpressRoute. Cette connexion est privée.
### Filtrer le trafic réseau
Vous pouvez filtrer le trafic réseau entre les sous-réseaux en utilisant l’une ou l’autre des options suivantes :
- Groupes de sécurité réseau : les groupes de sécurité réseau et les groupes de sécurité d’application peuvent contenir plusieurs règles de sécurité entrantes et sortantes.
- Appliances virtuelles réseau : une appliance virtuelle réseau est une machine virtuelle qui exécute une fonction réseau, telle qu’un pare-feu ou une optimisation WAN.
# Azure Virtual Network Manager
Azure Virtual Network Manager est un service de gestion qui vous permet de regrouper, de configurer, de déployer et de gérer des réseaux virtuels globalement sur l’ensemble des abonnements
# Planifier et implémenter des groupes de sécurité réseau (NSG) et des groupes de sécurité d’application (ASG)
## Groupes de sécurité réseau (NSG)
Les règles de sécurité sont évaluées et appliquées en fonction des informations de cinq tuples (**1. source, 2. port source, 3. destination, 4. port de destination et 5. protocole**)

# Planifier et implémenter des itinéraires User-Defined (UDR)
Vous pouvez spécifier les types de sauts suivants lors de la création d’un itinéraire défini par l’utilisateur :
- Appliance virtuelle : une appliance virtuelle est une machine virtuelle qui exécute généralement une application réseau, telle qu’un pare-feu.
- Passerelle de réseau virtuel : spécifiez quand vous souhaitez que le trafic destiné aux préfixes d’adresses spécifiques acheminés vers une passerelle de réseau virtuel.
- Aucun : spécifiez quand vous souhaitez supprimer le trafic vers un préfixe d’adresse, plutôt que de transférer le trafic vers une destination.
- - Réseau virtuel : spécifiez l’option de réseau virtuel lorsque vous souhaitez remplacer le routage par défaut au sein d’un réseau virtuel.
- Internet : spécifiez l’option Internet lorsque vous souhaitez acheminer explicitement le trafic destiné à un préfixe d’adresse vers Internet, ou si vous souhaitez que le trafic destiné aux services Azure avec des adresses IP publiques conservées dans le réseau principal Azure.
# Planifier et implémenter un réseau étendu virtuel, y compris un hub virtuel sécurisé
Une instance Virtual WAN vous connecte à vos ressources dans Azure via une connexion VPN IPsec/IKE (IKEv1 et IKEv2). Ce type de connexion requiert un périphérique VPN local disposant d’une adresse IP publique exposée en externe.

# Chiffrement Azure
Cette unité fournit une vue d’ensemble de l’utilisation du chiffrement dans Microsoft Azure. Il couvre les principales zones de chiffrement, notamment le chiffrement au repos, le chiffrement en vol et la gestion des clés avec Azure Key Vault.

## Chiffrement des données au repos
Les données au repos incluent des informations qui résident dans un stockage persistant sur un support physique, dans n’importe quel format numérique. Il peut s’agir de fichiers sur un support magnétique ou optique, de données archivées et de sauvegardes de données.
### Chiffrement côté serveur
- Clés gérées par le service : offre une combinaison de contrôle et de commodité avec une faible surcharge.
- Clés gérées par le client : vous permettent de contrôler les clés, y compris la prise en charge de la fonctionnalité BYOK (Apportez vos propres clés), ou vous permettent de générer de nouvelles clés.
- Clés gérées par le service dans le matériel contrôlé par le client : vous permet de gérer des clés dans votre référentiel propriétaire, en dehors du contrôle Microsoft. Cette caractéristique est appelée HYOK (Host Your Own Key). Toutefois, la configuration est complexe et la plupart des services Azure ne prennent pas en charge ce modèle

# Qu’est-ce que Liaison privée Azure
Azure Private Link vous permet d’accéder aux services Azure PaaS (par exemple Stockage Azure et SQL Database) ainsi qu’aux services de partenaires ou de clients hébergés par Azure sur un [point de terminaison privé](https://learn.microsoft.com/fr-fr/azure/private-link/private-endpoint-overview) dans votre réseau virtuel.
# Superviser la sécurité réseau en utilisant Network Watcher
Azure Network Watcher fournit une suite d’outils permettant de surveiller, diagnostiquer, afficher les métriques et activer ou désactiver les journaux d’activité pour les ressources Azure IaaS (Infrastructure-as-a-Service).
Network Watcher se compose de trois principaux ensembles d’outils et de fonctionnalités :

- Supervision  
    
- Outils de diagnostic réseau
- Trafic
# des points de terminaison de service de réseau virtuel
Les points de terminaison de service de réseau virtuel (VNet) offrent une connectivité directe et sécurisée aux services Azure via un itinéraire optimisé sur le réseau principal Azure.
## Principaux avantages
- Sécurité renforcée pour vos ressources de service Azure : les espaces d'adressage privés du réseau virtuel peuvent se chevaucher.
- Routage optimal pour le trafic du service Azure à partir de votre réseau virtuel
- Configuration simple et gestion simplifiée : vous n'avez plus besoin d'adresses IP publiques réservées dans vos réseaux virtuels pour sécuriser les ressources Azure via un pare-feu IP.