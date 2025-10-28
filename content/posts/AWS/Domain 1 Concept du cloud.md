le cloud computing est un modèle permettant un accès réseau omniprésent, pratique et à la demande à un pool partagé de ressources informatiques configurables (par exemple, réseaux, serveurs, stockage, applications et services) qui peuvent être rapidement provisionnées et libérées avec un minimum d'effort de gestion ou de fournisseur de services. 

Libre­-service à la demande _L’utilisateur gère les ressources mis à disposition par le fournisseur provider, et paie en fonction de l’utilisation de ces ressources, il s’agit d’un paiement à l’utilisation._

**Large accès au réseau (Broad network access)** _Déployer rapidement à l’échelle mondiale, si vous avez un client à l’étranger, il devrait y accéder avec une très faible latence._

 **Mise en commun des ressources (ressource pooling)** _En créant une infrastructure partagée avec d’autres clients vous pouvez réaliser des économies d’échelles plus importantes_

**Elasticité rapide (Rapid elasticity)** _L’élisticité représente la capacité du système à s’adapter aux demandes en approvisionnant et en désapprovisionnant des ressources de manière automatique._

 **Service mesuré (Measured service)**  
_Sécuriser le service par une détection immédiate d’une surcharge ou inversement, si l’une de ces conditions est vérifié, c’est à l’elasticité de prendre en charge cela._



## Les services Cloud AWS 

### EC2 (Elastic compute cloud ) 
EC2 est un service web qui fournit des ressources de calcul sécurisées et redimensionnables (machines virtuelles) dans le cloud AWS

### AWS Lambda 
AWS Lambda est un excellent exemple d’une nouvelle révolution dans le cloud computing appelée calcul sans serveur.
Lambda vous permet d'exécuter du code sans avoir à provisionner ni à gérer des serveurs.

### Elastic Beanstalk 
AWS Elastic Beanstalk est un service facile à utiliser pour déployer et faire évoluer des applications et services web développés avec des langages courants tels que Java, PHP et Python, pour n'en citer que quelques­uns.

### AWS Elastic Container Service ( ECS)
ECS d'Amazon est un service de gestion de conteneurs hautement évolutif et performant, compatible avec les conteneurs Docker. ECS vous permet d'exécuter efficacement des applications sur un cluster géré d'instances EC2.

### Simple storage service (S3)
AWS S3 est un stockage objet doté d'une interface de service web simple permettant de stocker et de récupérer n'importe quelle quantité de données, où que ce soit sur le web.

Elastic Block Store (EBS) EBS fournit des volumes de stockage de blocs persistants à utiliser avec les instances EC2 dans le cloud AWS

**S3 Glacier**  Amazon Glacier fait partie du service de stockage AWS S3. Il s'agit d'un service de stockage sécurisé, durable et extrêmement économique pour l'archivage et la sauvegarde à long terme des données.

Systeme de fichier elastic (EFS) AWS EFS fournit un stockage de fichiers simple et évolutif à utiliser avec les instances AWS EC2 dans le cloud AWS.

Cloud Prive Virtuel (VPC) AWS VPC vous permet de provisionner une section logiquement isolée du cloud AWS où vous pouvez lancer des ressources AWS dans un réseau virtuel que vous définissez.

**AWS Route 53** : AWS Route 53 est un service web DNS (Domain Name System) cloud hautement disponible et évolutif. Route 53 dirige efficacement les requêtes des utilisateurs vers l'infrastructure AWS (instances EC2, équilibreurs de charge Elastic Load Balancing ou buckets S3) et peut également servir à router les utilisateurs.

CloudFront : AWS CloudFront est un service mondial de réseau de diffusion de contenu (CDN). Il accélère la diffusion de vos sites web, API, contenus vidéo et autres ressources web.

**Passerelle API** : AWS API Gateway est un service entièrement géré qui simplifie la création, la publication, la maintenance, la surveillance et la sécurisation des API, quelle que soit leur taille.

Direct Connect  AWS Direct Connect est une solution qui rend Il est facile d'établir une connexion réseau dédiée entre vos locaux et AWS.

Service de base de données relationnelle (RDS) : AWS RDS simplifie la configuration, l'exploitation et la mise à l'échelle d'une base de données relationnelle dans le cloud.

DynamoDB : Amazon DynamoDB est un service de base de données NoSQL rapide et flexible pour toutes les applications nécessitant une latence constante de quelques millisecondes, quelle que soit l'échelle.

ElastiCache est un service web qui simplifie le déploiement, l'exploitation et la mise à l'échelle d'un cache en mémoire dans le cloud..

