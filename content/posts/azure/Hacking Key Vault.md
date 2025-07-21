---
title: Key Vault hacking
date: 2025-06-30
draft: true
tags:
  - azure
  - Hacking
  - azure
  - key
  - vault
categories:
  - azure
summary: key vault
showToc: true
tocOpen: true
---

## Key Vault - Abuser une politique d’accès

Vous pouvez contrôler l’accès aux clés, aux certificats et aux secrets Key Vault en utilisant des stratégies d’accès Azure RBAC ou Key Vault.
Lors de l’utilisation du modèle d’autorisation de stratégie d’accès, un utilisateur ayant le rôle `Contributor`, `Key Vault Contributor` ou tout autre rôle incluant des autorisations `Microsoft.KeyVault/vaults/write` pour le plan de gestion de Key Vault peut s’accorder l’accès au plan de données en définissant une stratégie d’accès Key Vault.

- En raison des privilèges élevés accordés à l'utilisateur, ce dernier a pu afficher le secret du coffre-fort de clés.

![[Pasted image 20250630023407.png]]

En énumérant le coffre de clés, nous trouvons un secret. Cliquez sur la version du secret, puis sur « Afficher la valeur du secret » pour afficher le secret.

![[Pasted image 20250630023604.png]]