# HelloDojo Marketing

Ce fichier permet de documenter (et logguer) les étapes que suivies
pour réaliser les demandes du [README.md](README.md).

## Mise en place
1. Voici les étapes que j'ai suivies pour installer le `serveur` et `importer` les `tables`.
1. J'ai commencer par crée une base de donnée grâce à `phpMyAdmin`,
1. puis j'ai importer les `dumps`,
1. ensuite j'ai choisi d'utiliser `MySQL Workbench` comme client de gestion de base de donnée.
1. pour finir je me suis connecté avec `l'identifiant` et le `mots de passe` choisit au préalable, en se connectant au `port` ouvert manuellement grâce au serveur crée par`UWAMP`

## MLD
![Mon MLD](https://i.imgur.com/XPV84Eu.png "Mon MLD généré avec l'option : database->reverse enginering")

## Informations à récolter
### Générales
1. La table `people` contient `NUMBER` personnes, ma requête est:  
   `SELECT COUNT(distinct id) 
   FROM people`

1. La table `people` contient `NUMBER` doublons, ma requête est:  
   `SELECT idnumber, COUNT(idnumber) 
   FROM people 
   GROUP BY idnumber 
   HAVING COUNT(idnumber) > 1;`

1. Cette requête permet de trouver les infos de la personne dont le nom de famille est "Warren" ?
   `SELECT * FROM people 
   WHERE lastname="Warren`