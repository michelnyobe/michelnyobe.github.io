---
title: "kerberoasting ciblé"
date: 2026-03-06T10:00:00+02:00
draft: true
tags: ["pentest", "Active Directory", "PowerView", "reconnaissance", "CRTP", "red team"]
categories: ["Red Team", "pentester"]
summary: ""
showToc: true
tocOpen: true
---

Cet abus peut survenir lors du contrôle d'un objet doté d'un `GenericAll`, `GenericWrdddtechnique est appelée Kerberoasting ciblé.Cet abus peut survenir lors du contrôle d'un objet doté d'un GenericAll, GenericWriteou sur la cible. Un membre du groupe Opérateur de compteWriteProperty dispose généralement de ces autorisations.Validated-SPN[]

L'attaquant peut ajouter un SPN ( `ServicePrincipalName`) à ce compte. Une fois ce SPN obtenu, le compte devient vulnérable au [Kerberoasting](https://www.thehacker.recipes/ad/movement/kerberos/kerberoast) . Cette technique est appelée Kerberoasting ciblé.Cet abus peut survenir lors du contrôle d'un objet doté d'un `GenericAll`, `GenericWrite`ou sur la cible. Un membre du groupe [Opérateur de compte](https://www.thehacker.recipes/ad/movement/builtins/security-groups)`WriteProperty` dispose généralement de ces autorisations.`Validated-SPN`[](https://www.thehacker.recipes/ad/movement/builtins/security-groups)

L'attaquant peut ajouter un SPN ( `ServicePrincipalName`) à ce compte. Une fois ce SPN obtenu, le compte devient vulnérable au [Kerberoasting](https://www.thehacker.recipes/ad/movement/kerberos/kerberoast) . Cette technique est appelée Kerberoasting ciblé.