Le contrôle d'accès aux ressources avec Azure RBAC consiste à attribuer des rôles Azure. Il s'agit d'un concept essentiel à comprendre : c'est ainsi que les autorisations sont appliquées. L'attribution d'un rôle se compose de trois éléments : le principal de sécurité, la définition du rôle et l'étendue.

Une définition de rôle est un ensemble d'autorisations. On l'appelle généralement simplement un rôle. Une définition de rôle répertorie les actions pouvant être effectuées, telles que la lecture, l'écriture et la suppression. Les rôles peuvent être de haut niveau, comme celui de propriétaire, ou spécifiques, comme celui de lecteur de machine virtuelle.
## Comment Azure RBAC détermine si un utilisateur a accès à une ressource

Voici les étapes générales utilisées par Azure RBAC pour déterminer si vous avez accès à une ressource. Ces étapes s'appliquent à Azure Resource Manager ou aux services de plan de données intégrés à Azure RBAC. Elles sont utiles pour résoudre un problème d'accès.

1. Un utilisateur (ou principal de service) acquiert un jeton pour Azure Resource Manager. Ce jeton inclut les appartenances aux groupes de l'utilisateur (y compris les appartenances aux groupes transitifs).
2. L’utilisateur effectue un appel d’API REST vers Azure Resource Manager avec le jeton attaché.
3. Azure Resource Manager récupère toutes les attributions de rôles et refuse les attributions qui s’appliquent à la ressource sur laquelle l’action est effectuée.
4. Si une affectation refusée s'applique, l'accès est bloqué. Sinon, l'évaluation se poursuit.
5. Azure Resource Manager restreint les attributions de rôles qui s’appliquent à cet utilisateur ou à son groupe et détermine les rôles dont dispose l’utilisateur pour cette ressource.
6. Azure Resource Manager détermine si l'action de l'appel d'API est incluse dans les rôles de l'utilisateur pour cette ressource. Si les rôles incluent `Actions`un caractère générique ( `*`), les autorisations effectives sont calculées en soustrayant le `NotActions`de la valeur autorisée `Actions`. De même, la même soustraction est effectuée pour toutes les actions de données.

- `Actions - NotActions = Effective management permissions`
- `DataActions - NotDataActions = Effective data permissions`

7. Si l'utilisateur ne dispose pas d'un rôle pour l'action correspondant à la portée demandée, l'accès n'est pas autorisé. Dans le cas contraire, toutes les conditions sont évaluées.
8. Si l'attribution de rôle comprend des conditions, celles-ci sont évaluées. Sinon, l'accès est autorisé.
9. Si les conditions sont remplies, l'accès est autorisé. Sinon, l'accès est refusé.