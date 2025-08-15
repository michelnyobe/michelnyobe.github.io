# Introduction à la chasse aux menaces 

Les aspects clés de la chasse aux menaces comprennent :
- Une stratégie offensive proactivequi privilégie l’anticipation des menaces plutôt que la réaction, basée sur des hypothèses, les TTP des attaquants et les renseignements.
- Une réponse offensive reactivequi recherche sur le réseau des artefacts liés à un verifiedincident, sur la base de preuves et de renseignements.
- Une compréhension solide et pratique du paysage des menaces, des cybermenaces, des TTP adverses et de la chaîne de destruction cybernétique.
- Empathie cognitive avec l’agresseur, favorisant la compréhension de l’état d’esprit de l’adversaire.
- Une connaissance approfondie de l'environnement informatique de l'organisation, de la topologie du réseau, des actifs numériques et de l'activité normale.
- Utilisation de données haute fidélité et d’analyses tactiques, et exploitation d’outils et de plateformes avancés de recherche de menaces.

# Structure d'une équipe de chasse aux menaces
La constitution d'une équipe de chasse aux menaces est un processus stratégique et minutieusement planifié qui requiert des compétences, une expertise et des perspectives diversifiées. Il est crucial que chaque membre de l'équipe possède un ensemble de compétences uniques qui, combinées, offrent une approche globale et complète pour identifier, atténuer et éliminer les menaces.

La composition idéale d'une équipe de chasse aux menaces comprend généralement les rôles suivants :
- Threat HunterAu sein de l'équipe, les chasseurs de menaces sont des professionnels de la cybersécurité possédant une connaissance approfondie du paysage des menaces, des tactiques, techniques et procédures (TTP) des cyberadversaires et des méthodologies sophistiquées de détection des menaces.
- Threat Intelligence AnalystCes personnes sont chargées de collecter et d'analyser des données provenant de diverses sources, notamment des renseignements provenant de sources ouvertes, du dark web, des rapports sectoriels et des flux de menaces. 
- Incident RespondersLorsque les chasseurs de menaces identifient des menaces potentielles, les intervenants en cas d'incident interviennent pour gérer la situation. 
- Forensics Experts:Il s'agit des membres de l'équipe qui analysent en profondeur les détails techniques d'un incident. 
- Data Analysts/Scientists:Ils jouent un rôle essentiel dans l’examen de grands ensembles de données, en utilisant des modèles statistiques, des algorithmes d’apprentissage automatique et des techniques d’exploration de données pour découvrir des modèles, des corrélations et des tendances qui peuvent conduire à des informations exploitables pour les chasseurs de menaces.
- Security Engineers/ArchitectsLes ingénieurs en sécurité sont responsables de la conception globale de l'infrastructure de sécurité de l'organisation.
- Network Security AnalystCes professionnels sont spécialisés dans le comportement des réseaux et les schémas de trafic.
- SOC Manager:Le responsable du centre des opérations de sécurité (SOC) supervise les opérations de l'équipe de recherche de menaces, assurant une coordination harmonieuse entre les membres de l'équipe et une communication efficace avec le reste de l'organisation.
- 
# La relation entre l'évaluation des risques et la chasse aux menaces

L'évaluation des risques, aspect essentiel de la cybersécurité, permet une compréhension globale des vulnérabilités potentielles et des vecteurs de menaces au sein d'une organisation. Dans le cadre de la traque des menaces, l'évaluation des risques constitue un outil essentiel, nous permettant de prioriser nos activités et de concentrer nos efforts sur les domaines les plus impactants.

L'évaluation des risques implique un processus systématique d'identification et d'évaluation des risques en fonction des sources de menaces potentielles, des vulnérabilités existantes et de l'impact potentiel de leur exploitation. Elle comprend une série d'étapes, notamment l'identification des actifs, des menaces et des vulnérabilités, la détermination des risques et, enfin, la formulation d'une stratégie d'atténuation des risques.

Dans le processus de recherche des menaces, les informations recueillies à partir d’une évaluation approfondie des risques peuvent guider nos activités de plusieurs manières :

Prioritizing Hunting EffortsEn identifiant les actifs les plus critiques (souvent appelés « joyaux de la couronne ») et les risques associés, nous pouvons prioriser nos efforts de recherche de menaces dans ces domaines. Ces actifs peuvent inclure des référentiels de données sensibles, des applications critiques ou des infrastructures réseau clés.

Understanding Threat LandscapeL'étape d'identification des menaces de l'évaluation des risques nous permet de mieux comprendre le paysage des menaces, notamment les tactiques, techniques et procédures (TTP) utilisées par les acteurs potentiels. Cette compréhension nous aide à développer nos hypothèses de recherche, essentielles à une chasse proactive aux menaces.

Highlighting VulnerabilitiesL'évaluation des risques permet de mettre en évidence les vulnérabilités de nos systèmes, applications et processus. Connaître ces faiblesses nous permet de rechercher des indicateurs d'exploitation dans ces domaines. Par exemple, si nous savons qu'une application particulière présente une vulnérabilité permettant une élévation de privilèges, nous pouvons rechercher des anomalies dans les niveaux de privilèges des utilisateurs.

Informing the Use of Threat IntelligenceLa veille sur les menaces est souvent utilisée dans la chasse aux menaces pour identifier les schémas de comportement malveillant. L'évaluation des risques contribue à éclairer l'application de la veille sur les menaces en identifiant les acteurs les plus probables et leurs méthodes d'attaque préférées.

Refining Incident Response PlansL'évaluation des risques joue également un rôle essentiel dans l'amélioration des plans de réponse aux incidents (IR). Comprendre les risques potentiels nous aide à anticiper et à planifier les violations potentielles, garantissant ainsi une réponse rapide et efficace.

Enhancing Cybersecurity Controls:Enfin, les stratégies d’atténuation des risques dérivées de l’évaluation des risques peuvent directement contribuer à l’amélioration des contrôles et des défenses de cybersécurité existants, renforçant ainsi davantage la posture de sécurité de l’organisation.

# Le processus de chasse aux menaces
Vous trouverez ci-dessous une brève description du processus de recherche de menaces :
- Setting the Stage: La phase initiale est entièrement consacrée à la planification et à la préparation. Elle comprend la définition d'objectifs clairs, fondés sur une compréhension approfondie du paysage des menaces, des besoins critiques de notre entreprise et de nos analyses de renseignements sur les menaces.Setting the Stage: La phase initiale est entièrement consacrée à la planification et à la préparation. Elle comprend la définition d'objectifs clairs, fondés sur une compréhension approfondie du paysage des menaces, des besoins critiques de notre entreprise et de nos analyses de renseignements sur les menaces.
- Formulating HypothesesL'étape suivante consiste à formuler des prédictions éclairées qui guideront notre traque des menaces. Ces hypothèses peuvent provenir de sources diverses, comme les récentes informations sur les menaces, les actualités du secteur, les alertes des outils de sécurité, voire notre intuition professionnelle. 
- 


Introduction to Threat Hunting & Hunting With Elastic  https://academy.hackthebox.com/module/214/section/2280
