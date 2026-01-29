# SPF ( Sender Policy Framework)
Le Sender Policy Framework ( SPF ) sert à authentifier l'expéditeur d'un courriel. Grâce à un enregistrement SPF , les fournisseurs d'accès à Internet peuvent vérifier qu'un serveur de messagerie est autorisé à envoyer des courriels pour un domaine spécifique.

https://dmarcian.com/spf-syntax-table/
https://toolbox.googleapps.com/apps/messageheader/

# DKIM (Domainkeys Identified Mail )

DKIM ( DomainKeys Identified Mail) est utilisé pour l'authentification des e-mails envoyés. À l'instar de SPF , DKIM est une norme ouverte d'authentification des e-mails, utilisée pour l'alignement DMARC
https://dmarcian.com/dkim-selectors/

# DMARC ( authentification , le reporting et la conformité des messages basés sur le domaine  )

« DMARC , une norme open source, utilise un concept appelé alignement pour lier le résultat de deux autres normes open source,   SPF (une liste publiée de serveurs autorisés à envoyer des courriels au nom d'un domaine) et DKIM (un sceau de domaine inviolable associé à un courriel), au contenu d'un courriel. »

Cela signifie que DMARC vérifie que le domaine de l'expéditeur correspond aux domaines vérifiés par SPF et DKIM . En cas d'échec de cette vérification, DMARC indique au serveur du destinataire comment traiter le courriel selon la politique définie dans l'enregistrement.

# S/MIME  ( Extensions de messagerie internet sécurisées/multiusages)
 **Secure/Multipurpose Internet Mail Extensions** ) est un protocole standard d'envoi de messages chiffrés et signés numériquement. Il repose sur la cryptographie à clé publique, où la clé privée n'est jamais partagée et la clé publique peut être diffusée librement


# Sécurisé les emails 

## # passerelle de messagerie sécurisée (SEG)
https://www.cloudflare.com/learning/email-security/secure-email-gateway-seg/