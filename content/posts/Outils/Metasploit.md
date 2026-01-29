## Mfsconsole

Pour commencer à utiliser Metasploit Framework, il suffit de saisir la commande msfconsoledans le terminal de votre choix.
On peut aussi utiliser l' -qoption qui ne permet pas d'afficher la bannière.

```
msfconsole -q
```

Pour mieux visualiser toutes les commandes disponibles, nous pouvons les saisir help. 
## Architecture de Metasploit 
### Modules 
#### les types de modules 
- Auxiliary : Fonctionnalités de scan, de fuzzing, d'analyse du trafic réseau et d'administration. Offre une assistance et des fonctionnalités supplémentaires.
- Encoders  : S'assurer que les charges utiles arrivent intactes à destination.
- Exploits : Définis comme des modules exploitant une vulnérabilité permettant la livraison de la charge utile.
- NOPs : (Aucun code d'opération) Veillez à ce que la taille de la charge utile reste constante d'une tentative d'exploitation à l'autre.
- payloads : Le code s'exécute à distance et renvoie une connexion (ou un shell) à la machine de l'attaquant.
- Plugins : Des scripts supplémentaires peuvent être intégrés à une évaluation et coexister avec elle.
- Post : Large éventail de modules pour recueillir des informations, approfondir l'analyse, etc.
Nous pouvons également affiner notre recherche et la limiter à une seule catégorie de services. Par exemple, pour la CVE, nous pourrions spécifier l'année cve:<year>, la plateforme Windows platform:<os>, le type de module recherché type:<auxiliary/exploit/post>, le niveau de fiabilité rank:<rank>et le nom de la recherche <pattern>. Les résultats ne contiendraient alors que les services correspondant à tous ces critères.


```
search type:exploit platform:windows cve:2021 rank:excellent microsoft
```

###  Cibles

Il s'agit d'identifiants uniques du système d'exploitation, extraits des versions spécifiques de ces systèmes, qui permettent d'adapter le module d'exploitation sélectionné à cette version particulière. 

### Payloads 
Dans Metasploit, un Payloadmodule A aide le module d'exploitation à (généralement) obtenir un accès shell pour l'attaquant.
Le framework Metasploit propose trois types de modules de charge utile : Singles, Stagers et Stages.
#### charge simple 

utile contient l'exploit et l'intégralité du shellcode pour la tâche sélectionnée. Les charges utiles intégrées sont, par conception, plus stables que les autres car elles regroupent tous les éléments nécessaires. Cependant, certains exploits ne supportent pas la taille importante de ces charges utiles, qui peut devenir considérable.

### Encoder 

https://www.rapid7.com/blog/post/2015/03/25/stageless-meterpreter-payloads/

```
 sudo msfdb init
 
```
