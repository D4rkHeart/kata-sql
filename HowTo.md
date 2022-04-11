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

1. La table `people` est triée par nom de famille, ma requête est:  
   `SELECT * FROM people 
   ORDER BY lastname`

1. Les 5 premières entrées de la table `people` sont:  
   `SELECT * FROM people 
   LIMIT 5 `

1. Je trouve toutes les personnes dont le nom ou le prénom contient `ojo`, ma requête est:  
   `SELECT * FROM people 
   WHERE firstname OR lastname 
   LIKE '%ojo%'`

1. Les 5 personnes plus agées sont obtenus avec cette requête:  
   `SELECT * FROM people 
   ORDER BY birthdate 
   ASC LIMIT 5 `

1. Les 5 personnes plus jeunes sont obtenus avec cette requête:  
   `SELECT * FROM people 
   ORDER BY birthdate 
   DESC LIMIT 5 `

1. La requête suivante permet de trouver l'age en année de chaque personne:  
   `SELECT FORMAT(DATEDIFF(now(),birthdate)/365,0) 
   FROM people`

1. La moyenne d'age est `NUMBER`, ma requête est:  
   `SELECT FORMAT(AVG(DATEDIFF(now(),birthdate)/365),0) 
   AS "Age moyen" FROM people`

1. Le plus long prénom est `TEXT`, ma requête est:  
   `SELECT firstname FROM people 
   ORDER BY LENGTH(firstname) DESC LIMIT 1`

1. Le plus long nom de famille est `TEXT`, ma requête est: 
   `SELECT lastname FROM people 
   ORDER BY LENGTH(lastname) DESC LIMIT 1 `

1. La plus longue paire nom + prénom est `TEXT`, ma requête est:  
   `SELECT lastname,firstname FROM people 
   ORDER BY LENGTH(`lastname`) DESC LIMIT 1`
### Invitations
1. Pour lister tous le membres de plus de 18 ans:  
   `SELECT DATEDIFF(now(),birthdate)/365 AS Majeur FROM people 
   HAVING Majeur>=18`
  a. et de moins de 60 ans:  
     	`SELECT DATEDIFF(now(),birthdate)/365 AS Age FROM people HAVING Age>60`
  a. qui ont une addresse email valide:  
	`SELECT email FROM people 
	WHERE email LIKE '%_@__%.__%'`

1. Pour ajoutez une colonne `age` dans le résultat de la requête:  
   `SELECT DATEDIFF(now(),birthdate)/365 
   AS Age`

1. Pour générer une liste contenant `Prénom Nom <email@provider.com>`;  
   `SELECT firstname, lastname, email FROM people 
   WHERE email LIKE '%_@__%.__%' `

1. Avec cette requête:  
   `SELECT idcountry,COUNT(*) FROM countries_people 
   WHERE idcountry=756`  
   je peux estimer que `NUMBER` personnes habitent en Suisse.

### Jointure
1. Avec cette requête:  
   `SELECT idperson,idcountry,COUNT(idperson) as NbPersonne 
   FROM countries_people 
   LEFT JOIN people ON people.id = countries_people.idperson 
   LEFT JOIN countries ON countries.id = countries_people.idcountry 
   WHERE countries.name_fr="Suisse"`  
      je sais que `NUMBER` personnes habitent en Suisse.

1. Avec cette requête:  
   `SELECT idperson,idcountry,COUNT(idperson) as NbPersonne 
   FROM countries_people 
   LEFT JOIN people ON people.id = countries_people.idperson 
   LEFT JOIN countries ON countries.id = countries_people.idcountry 
   WHERE NOT countries.name_fr="Suisse"`  
      je sais que `NUMBER` personnes n'habitent pas en Suisse.

1. Avec cette requête:  
   `SELECT firstname as "prénom",lastname as "Nom",name_fr as "pays" 
   FROM countries_people 
   LEFT JOIN people ON people.id = countries_people.idperson 
   LEFT JOIN countries ON countries.id = countries_people.idcountry 
   WHERE name_fr="France" or "Allemagne" or "Italie" or "Autriche"`  
      je liste (nom & prénom) des membres habitants de France, Allemagne, Italie, Autriche 
      et Lischenchtein.

1. Cette requête:  
   `SELECT name_fr AS "Pays",COUNT(*) AS "NbPersonne" 
   FROM countries_people 
   LEFT JOIN people ON people.id = countries_people.idperson 
   LEFT JOIN countries ON countries.id = countries_people.idcountry 
   WHERE name_fr is not null 
   GROUP BY name_fr`  
      permet de compter combien il y a de personnes par pays.

1. Cette requête:  
   `SELECT name_fr AS "Pays" FROM countries_people 
   LEFT JOIN people ON people.id = countries_people.idperson 
   LEFT JOIN countries ON countries.id = countries_people.idcountry 
   WHERE idperson is null`  
      liste les pays qui ne possèdent pas de personnes.

1. En exécutant cette requête:  
   `SELECT firstname AS "Prénom",COUNT(name_fr) AS "NbPays" 
   FROM countries_people 
   JOIN people ON people.id = countries_people.idperson 
   JOIN countries ON countries.id = countries_people.idcountry 
   GROUP BY firstname HAVING "NbPays"<1`  
      je sais que `NAME`, `NAME` et `NAME` sont liés à plusieurs pays.
