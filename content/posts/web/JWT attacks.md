JWT ( json web token )

![[Pasted image 20250707104217.png]]

Les JWT étant principalement utilisés dans les mécanismes d'authentification, de gestion de session et de contrôle d'accès

La spécification JWT est étendue par les spécifications JSON Web Signature (JWS) et JSON Web Encryption (JWE), qui définissent des méthodes concrètes d'implémentation des JWT.
L'un des types de jetons les plus courants est le jeton Web JSON (JWT), qui est transmis via l' `Authorization: Bearer`en-tête.
Les JWT sont des jetons autonomes permettant de transmettre des informations de session de manière sécurisée.

Vous trouverez ci-dessous les deux requêtes cURL permettant d'interagir avec l' API . Pour l'authentification, la requête cURL suivante peut être effectuée :

```bash
curl -H 'Content-Type: application/json' -X POST -d '{ "username" : "user", "password" : "passwordX" }' http://MACHINE_IP/api/v1.0/exampleX
```

```bash
curl -H 'Authorization: Bearer [JWT token]' http://MACHINE_IP/api/v1.0/example2?username=Y
```

### Structure de JWT

Un JWT se compose de trois composants, chacun codé en Base64Url et séparé par des points :
**Header** - L'en-tête indique généralement le type de jeton, qui est JWT, ainsi que l'algorithme de signature utilisé.
**Payload** la charge utile est le corps du jeton, qui contient les revendications. Une revendication est une information fournie pour une entité spécifique.
**Signature** la signature est la partie du jeton qui fournit une méthode pour vérifier son authenticité.

### Algorithmes de signature

Aucun 
signature symetrique : HS256,
signature asymetrique : RS256

#### Ne pas vérifier la signature
Le premier problème avec la validation de signature survient en l'absence de validation. Si le serveur ne vérifie pas la signature du JWT, il est possible de modifier les revendications du JWT selon vos préférences.

```
payload = jwt.decode(token, options={'verify_signature': False})
```

**La solution**  

Le JWT doit toujours être vérifié ou des facteurs d'authentification supplémentaires, tels que des certificats, doivent être utilisés pour les communications de serveur à serveur. Le JWT peut être vérifié en fournissant la clé secrète (ou publique), comme illustré dans l'exemple ci-dessous :

```
Payload = jwt.decode(token, self.secret, algorithms="HS256")
```


### Travailler avec des JWT dans Burp Suite

Vous pouvez utiliser Burp Inspector pour visualiser et décoder les JWT. Vous pouvez ensuite utiliser l'extension JWT Editor pour :

1. Générer des clés de signature cryptographiques.
2. Modifiez le JWT.
3. Résignez le jeton avec une signature valide qui correspond au JWT modifié.

https://jwt.io/
https://datatracker.ietf.org/doc/html/rfc7519
https://gchq.github.io/CyberChef/
https://docs.github.com/fr/enterprise-cloud@latest/apps/creating-github-apps/authenticating-with-a-github-app/generating-a-json-web-token-jwt-for-a-github-app
https://portswigger.net/burp/documentation/desktop/testing-workflow/session-management/jwts