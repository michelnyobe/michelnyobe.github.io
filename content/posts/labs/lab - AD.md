# Etapes
1. Configurer le controleur de Domaine (DC01)
nom du domaine : nyoma.local
2. Créer des objets utilisateur
departement / OU
- Direction
- IT
- Finance
- RH
- Marketing
- Sales
- Support
- Production
- 
|Nom complet|Username|Groupe|
|---|---|---|
|Michel Nyobe|mnyobe|GG_IT_Admin|
|Sarah Laurent|slaurent|GG_RH|
|Thomas Dubois|tdubois|GG_Finance|
|Julien Moreau|jmoreau|GG_IT_Support|
|Clara Petit|cpetit|GG_Marketing|
|Antoine Martin|amartin|GG_Sales|
|Laura Bernard|lbernard|GG_Sales|
|Kevin Robert|krobert|GG_Production|
|Sophie Leroy|sleroy|GG_RH|
|Hugo Fontaine|hfontaine|GG_IT_Admin|
|Emma Rousseau|erousseau|GG_Finance|
|Lucas Garnier|lgarnier|GG_IT_Support|
|Inès Chevalier|ichevalier|GG_Marketing|
|Nathan Girard|ngirard|GG_Production|
|Camille Bonnet|cbonnet|GG_Direction|
|Maxime Marchand|mmarchand|GG_Server_Admins|
|Léa Dupont|ldupont|GG_Backup_Operators|
|Enzo Mercier|emergier|GG_IT_Support|
|Chloé Faure|cfaure|GG_Marketing|
|Adam Noel|anoel|GG_Sales|


1 compte service faible :
svc-backup  
Mot de passe : Backup123  
Groupe : GG_Backup_Operators

utilisateur avec mot de passe faible :

jmoreau  
Mot de passe : Welcome1

1 compte Domain Admin :

adm-tech  
Groupe : Domain Admins


## creation des groupes 
Active Directory a deux formes de principaux de sécurité courants : les comptes d’utilisateur et les comptes d’ordinateur. Ces comptes représentent une entité physique qui est une personne ou un ordinateur.
https://learn.microsoft.com/fr-fr/windows-server/identity/ad-ds/manage/understand-security-groups

OU=Corp  
└── OU=Users  
├── OU=Direction  
│ └── Groupe : Direction  
├── OU=IT  
│ └── Groupe : IT  
├── OU=Finance  
│ └── Groupe : Finance  
├── OU=RH  
│ └── Groupe : RH  
├── OU=Marketing  
│ └── Groupe : Marketing  
└── OU=Support  
└── Groupe : Support


## Creation un object : utilisateur 
https://learn.microsoft.com/fr-fr/windows-server/identity/ad-ds/manage-user-accounts-in-windows-server#create-a-user-account

- Sarah Laurent :  T9v!Qm2@Lx7p  / slaurent
- Thomas Dubois : Rk4#Zt8&Yn3w / tdubois
- Julien Moreau : Mx7$Pd2!Kv9a /  jmoreau
-  Clara Petit : Hq3@Ls8^Tw5n /  cpetit
- Antoine Martin : Bv9!Er4#Xp2z /  amartin
- Laura Bernard : Nj6$Uy1@Gk8r
- Kevin Robert : Cp2^Wd7!Lm4q
- Sophie Leroy : Fs8@Hr3#Vz6t 
- Hugo Fontaine : Yk5!Bn9$Qp1x 
- Emma Rousseau : Td7#Mx2@Ls8v
- Lucas Garnier : Gp4!Zr6^Wk3n
- Inès Chevalier :  Lx9$Tv2@Hq7m 
- Nathan Girard : Vr3!Pk8#Ys4d
- Camille Bonnet : Qm6@Ln1$Xt9b
- Maxime Marchand : Dz8!Hr5^Kp2w
- Léa Dupont : Sw2#Yv7@Lm6t 
- Enzo Mercier : Uk9!Gp4$Rx3q
- Chloé Faure : Nm5@Zt8^Ld1v 
- Adam Noel  : Px7#Hr2!Kw9a



  
## UO

DC=nyoma,DC=local  
└── OU=Corp  
├── OU=Users  
│ ├── OU=IT  
│ ├── OU=Finance  
│ └── OU=RH  
├── OU=Servers  
├── OU=Admins  
└── OU=ServiceAccounts


##repartirion 

# Répartition logique des utilisateurs

Je les répartis proprement par département :

### 👑 Direction

- Sarah Laurent (slaurent)
    
- Thomas Dubois (tdubois)
    

### 💻 IT

- Julien Moreau (jmoreau)
    
- Hugo Fontaine
    
- Lucas Garnier
    
- Nathan Girard
    

### 💰 Finance

- Clara Petit
    
- Laura Bernard
    
- Camille Bonnet
    
- Maxime Marchand
    

### 👥 RH

- Sophie Leroy
    
- Inès Chevalier
    
- Léa Dupont
    

### 📢 Marketing

- Antoine Martin
    
- Emma Rousseau
    
- Chloé Faure
    

### 🛠 Support

- Kevin Robert
    
- Enzo Mercier
    
- Adam Noel


OU=Corp
 └── OU=Users
      ├── OU=Direction
      │    └── Group: Direction
      │         └── Sarah Laurent, Thomas Dubois
      ├── OU=IT
      │    └── Group: IT
      │         └── Julien, Hugo, Lucas, Nathan, Michel Nyobe
      ├── OU=Finance
      │    └── Group: Finance
      │         └── Clara, Laura, Camille, Maxime
      ├── OU=RH
      │    └── Group: RH
      │         └── Sophie, Inès, Léa
      ├── OU=Marketing
      │    └── Group: Marketing
      │         └── Antoine, Emma, Chloé
      └── OU=Support
           └── Group: Support
                └── Kevin, Enzo, Adam