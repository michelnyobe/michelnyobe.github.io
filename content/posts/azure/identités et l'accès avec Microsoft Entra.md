## Objectifs d'apprentissage

Dans ce module, vous :

- Créer, configurer et gérer les utilisateurs
- Créer, configurer et gérer des groupes
- Gérer les licences
- Explorez les attributs de sécurité personnalisés et le provisionnement automatique
## Afficher les utilisateurs
Pour afficher les utilisateurs Microsoft Entra, sélectionnez l' entrée **« Utilisateurs » sous** **« Identité** », puis ouvrez la vue **« Tous les utilisateurs »** . Prenez quelques instants pour accéder au portail et consulter vos utilisateurs. Observez la colonne **« Type d'utilisateur »** pour afficher les membres et les invités.
En règle générale, Microsoft Entra ID définit les utilisateurs de trois manières :

- **Identités cloud** : ces utilisateurs existent uniquement dans Microsoft Entra ID. Il peut s'agir de comptes administrateurs ou d'utilisateurs que vous gérez vous-même. Leur source est **Microsoft Entra ID** ou **un annuaire Microsoft Entra externe** si l'utilisateur est défini dans une autre instance Microsoft Entra mais a besoin d'accéder aux ressources d'abonnement contrôlées par cet annuaire. Lorsque ces comptes sont supprimés de l'annuaire principal, ils sont supprimés.
- **Identités synchronisées avec l'annuaire** : ces utilisateurs sont présents dans un annuaire Active Directory local. Une synchronisation via **Microsoft Entra Connect** permet d'intégrer ces utilisateurs à Azure. Leur source est **Windows Server AD** .
- **Utilisateurs invités** : ces utilisateurs existent en dehors d'Azure. Il peut s'agir de comptes d'autres fournisseurs cloud ou de comptes Microsoft, comme un compte Xbox LIVE. Leur source est **« Utilisateur invité »** . Ce type de compte est utile lorsque des fournisseurs ou sous-traitants externes ont besoin d'accéder à vos ressources Azure. Une fois leur intervention terminée, vous pouvez supprimer le compte et tous leurs accès.
## Créer un nouvel utilisateur dans Microsoft Entra ID
1. Accédez au menu Identité dans le [centre d’administration Microsoft Entra](https://entra.microsoft.com/) .
2. Dans la navigation de gauche, sélectionnez **Utilisateurs** , puis **Tous les utilisateurs.**
3. Dans la page Utilisateurs, dans le menu, sélectionnez + **Nouvel utilisateur** et **Créer un nouvel utilisateur** .
## Créer un groupe de sécurité dans Microsoft Entra ID
1. Accédez à l’écran du centre d’administration Microsoft Entra.
2. Dans la navigation de gauche, sous **Identité** , sélectionnez **Groupes** puis **Tous les groupes** .
3. Dans l’écran Groupes, dans le menu, sélectionnez **Nouveau groupe** .
## Attribuer une licence à un groupe

1. Dans la liste **Tous les groupes** , sélectionnez **Marketing** .
2. Dans la fenêtre Marketing, sous **Gérer** , sélectionnez **Licences** .
3. Notez qu'aucune licence n'est actuellement attribuée à ce groupe.
4. Ouvrez un nouvel onglet dans votre navigateur.
5. Accédez au centre d’administration Microsoft 365 à l’ [adresse http://admin.microsoft.com](https://admin.microsoft.com/) .
6. Sélectionnez **Facturation** dans le menu de gauche.
7. Sélectionnez **Licences** .
8. Dans la liste des licences dont vous disposez, sélectionnez-en une.
9. Ensuite, vous sélectionnez **Groupes** dans la liste en haut de l’écran.  
10. Sur la page Groupes, sélectionnez + **Attribuer une licence** .  
11. Recherchez et sélectionnez le groupe **Marketing** que nous avons créé précédemment.
12. Sélectionnez le bouton **Attribuer** en bas de la boîte de dialogue.
13. Vous devriez recevoir un message indiquant que les licences ont été attribuées avec succès.
# Créer, configurer et gérer des groupes
Un groupe Microsoft Entra permet d'organiser les utilisateurs, ce qui simplifie la gestion des autorisations. L'utilisation de groupes permet au propriétaire de la ressource (ou du répertoire Microsoft Entra) d'attribuer un ensemble d'autorisations d'accès à tous les membres du groupe, au lieu de devoir attribuer les droits un par un.
Microsoft Entra ID vous permet de définir deux types de groupes différents.

- **Les groupes de sécurité** sont les plus courants et permettent de gérer l'accès des membres et des ordinateurs aux ressources partagées d'un groupe d'utilisateurs. Par exemple, vous pouvez créer un groupe de sécurité pour une stratégie de sécurité spécifique. Ainsi, vous pouvez attribuer un ensemble d'autorisations à tous les membres simultanément, au lieu de les attribuer individuellement à chaque membre. Cette option nécessite un administrateur Microsoft Entra.
- **Groupes Microsoft 365** : offrez des opportunités de collaboration en donnant aux membres l'accès à une boîte aux lettres, un calendrier, des fichiers, un site SharePoint partagés, etc. Cette option vous permet également d'autoriser des personnes extérieures à votre organisation à accéder au groupe. Cette option est disponible pour les utilisateurs comme pour les administrateurs.
## Créer un groupe Microsoft 365 dans Microsoft Entra ID

1. Accédez à la page Identité du centre d’administration Microsoft Entra dans le [centre d’administration Microsoft Entra](https://entra.microsoft.com/) .
2. Dans la navigation de gauche, sous sélectionnez **Groupes** .
3. Dans la lame Groupes, dans le menu, sélectionnez **Nouveau groupe** .
4. Créez un groupe en utilisant les informations suivantes :
## Modifier l'attribution de licence de groupe

1. Ouvrez [https://entra.microsoft.com](https://entra.microsoft.com/) pour accéder au centre d’administration Microsoft Entra.
2. Dans la navigation de gauche, ouvrez **Groupes** .
3. Sélectionnez **Tous les groupes** , puis sélectionnez l’un des groupes disponibles.
4. Dans la navigation de gauche, sous **Gérer** , sélectionnez **Licences** .

Vous verrez la liste des licences actuellement attribuées. Vous devrez utiliser le Centre d'administration Microsoft 365 pour effectuer les mises à jour.

5. Passez en revue les tâches en cours, puis, dans le menu, sélectionnez **+ Tâches** .
6. Ouvrez [https://admin.microsoft.com](https://admin.microsoft.com/) pour ouvrir le centre d’administration Microsoft 365.
7. Sélectionnez **Facturation** . Sélectionnez ensuite **Licences** .
8. Sélectionnez une licence disponible dans la liste.
9. Sélectionnez **Groupes** dans le menu en haut de la page.
10. Sélectionnez l’ option **+ Attribuer des licences .**
11. Sélectionnez le groupe que vous consultiez précédemment dans Microsoft Entra. Sélectionnez ensuite le bouton **« Attribuer »** en bas de la page.
12. Sur la page « Licences » du groupe, vérifiez la modification. Vous devriez la voir dans le centre d'administration Microsoft Entra et dans le centre d'administration Microsoft 365.
## Mettre à jour les attributions de licences utilisateur

1. Accédez au centre d’administration Microsoft Entra.
2. Dans la navigation de gauche, sous **Identité** , sélectionnez **Utilisateurs** .
3. Dans l’écran Utilisateurs, sélectionnez **Dominique Koch** .
4. Dans la navigation de gauche, sélectionnez **Licences.**
5. Dans le panneau Mettre à jour les attributions de licence, cochez la case correspondant à une ou plusieurs licences.
## Qu'est-ce qu'un attribut de sécurité personnalisé ?

Les attributs de sécurité personnalisés de Microsoft Entra ID sont des attributs métier (paires clé-valeur) que vous pouvez définir et affecter aux objets Microsoft Entra. Ces attributs permettent de stocker des informations, de catégoriser des objets ou d'appliquer un contrôle d'accès précis à des ressources Azure spécifiques.