# Compte rendu Yann Mongo DB semaine du 10 janvier

## Génèse
Nous cherchons a installer dans nos chaines de restaurants des bornes afin de passer commandes ou les clients devront remplir leurs informations afin de pouvoir finaliser une commande, ici on doit pouvoir récupérer des données clients dans le cadre de la restauration, cela nous permettra d'enregistrer de nouveaux utilisateurs dans notre base de données et potentiellement d'utiliser ces données dans l'application.
Le projet aura comme système de gestion de base de données MongoDB car il peut nous permettre de gérer des systèmes assez complexes de base de données et dans un cas comme celui là ou l'ont possède plusieurs miliers de clients, il sera bien plus pertinent qu'un mySql ou autre outil de gestion de base de données moins performant.
Le but final sera de renforcer le marketing de notre chaine de restaurant de plusieurs manières grace a MongoDB.

On commence par mocker des data clients via n'importe quel site de génération de mock data : 
![image](https://user-images.githubusercontent.com/45734971/148910172-400990a8-ccd8-47b8-97d8-44ca1242ec73.png)
(ce sera ensuite modifié afin d'y rajouter les dates de naissances des clients)

Une fois les données mockés on peut les insérer dans notre nouvelle base de données clients :
![image](https://user-images.githubusercontent.com/45734971/148912663-ff5c8ce4-564d-458f-bb20-b809826c65b9.png)

On se rend compte que les données mockés ne sont pas correct et que la latitude et la longitude ne peuvent pas être lue par le logiciel car probablement les latitudes et longitudes sont dans le mauvais ordre : 
![image](https://user-images.githubusercontent.com/45734971/148924830-382fb117-c2b7-48bb-94cf-c8c0d0add085.png)

## Géospatialisation des données 

Nous allons avoir besoin de spatialiser les données gps afin de pouvoir se renseigner sur nos clients géopgraphiquement, pour cela MongoDB mets a disposition les requêtes Géospatiale, pour fonctionner elles nécéssites des données géométriques (point, polygone ou multiline) qui permettent au final de calculer plusieurs choses, des distances, des radiants, des zone géographiques

Nous allons tenter d'utiliser les géodatas récupérées des utilisateurs sous forme longitude/latitude pour les lire avec charts. 

Connexion de MongoDB Charts a notre base de données clients 
![image](https://user-images.githubusercontent.com/45734971/148925447-80d7f630-fc77-49d5-a080-6fa41e998769.png)

Prise en main de charts :
![image](https://user-images.githubusercontent.com/45734971/148926316-536ef77e-1e2d-4441-80da-77d7e6deadbf.png)

Charts nous permet de créer des graphiques grâce aux données qu'on lui confie, par exemple grace aux données gps des clients, savoir ou le traffic est le plus présent, ou le moins présent. Dans le cadre d'un projet, cela permet de savoir par exemple où centrer le marketing de la chaine de restaurant, par exemple là ou le taux de clients est le plus bas on peut penser a peut etre ouvrir un restaurant afin de fideliser plus de clients ou dans le cadre d'une ville ou métropole, de savoir ou placer les pubs du restaurant afin que plus de clients entendent parler de l'établissement.

Liaison de nos données de coordonnées des clients lus sur charts : 
![image](https://user-images.githubusercontent.com/45734971/148927663-56f09425-8586-4042-9dc3-688f9563657f.png)

## Restaurants les plus visités

MongoDB peut nous permettre de récuperer de multiples renseignement sur nos clients. Par exemple, ici nous cherchons a savoir quels restaurants sont les plus visités par nos clients afin de pouvoir les comparer aux autres et trouver des solutions afin d'augmenter les visites dans les autres restaurants également.

```
db.visits.aggregate([{
  "$sortByCount": "$restaurant_id"
}])
```
Cette aggregation nous permet de récupérer le restaurant qui est le plus représenté dans la collection visits.

## Récompenser les clients inscrits 

On peut également par exemple récupérer la date d'aujourd'hui et comparer aux dates de naissance des clients afin de savoir quand est leur anniversaire et leur envoyer un sms ou un mail leur proposant un repas gratuit ou a moindre coût :

```
db.clients.find({
   "$expr": { 
       "$and": [
            { "$eq": [ { "$dayOfMonth": "$date_de_naissance" }, { "$dayOfMonth": new Date() } ] },
            { "$eq": [ { "$month"     : "$date_de_naissance" }, { "$month"     : new Date() } ] }
       ]
    }
});
```
Cela necessite une date de naissance sous format DateTime pour fonctionner, on compare le jour et le mois de la date de naissance au jour et mois d'aujourd'hui afin de nous sortir les utilisateurs ou cela coincide. 

## Créer des jeux récurrent

MongoDB permet de pouvoir récupérer aléatoirement un document d'un collection. Grâce a cette fonctionnalité, on pourrait faire gagner a un ou plusieurs clients des repas sur toute l'année par exemple, si il est inscrit dans notre collection client.

Cette requete permet de récupérer 1 client aléatoirement dans toute la collection clients :

```
db.clients.aggregate([{ $sample: { size: 1 } }])
```

si l'on souhaite plus de gagnants il suffit de rajouter plus que 1 a "size".

## Promotion pour les visiteurs récurrents

On souhaite, si un visiteur viens plusieurs fois, pouvoir le récompenser. On aimerais que toutes les 4 visites un visiteurs puis gagner 25% sur sa commande.
Donc si un visiteur enregistre 4 visites, il y aura droit. Donc lors d'une nouvelle visites d'un clients, on vérifiera a combien il en est afin de savoir si il doit être récompensé.

```
db.clients.aggregate({
   $lookup:
    {
        from: "visits",
        localField: "id",
        foreignField: "visitor_id",
        as: "visitor_visits"
    }
   $project: 
    {
      numberOfVisits: { $cond: {if: { $isArray: "$visitor_visits"}, then: {$size: "$visitor_visits"}, else: "Aucune"} 
    }
})
```
On recoit au final le nombre de visites du clients.

## Campagnes téléphoniques

On peut également se créer une liste de numéro de téléphone en cas de campagne téléphonique prévue par le marketing :

```
db.clients.find({}, { phone: 1 })
```

## Aggrégation

Les Aggrégation permette de mettre en commun, relier ou manipuler des données, créer des jointures par exemple : 
![image](https://user-images.githubusercontent.com/45734971/149180713-b6811f25-9789-409e-8d7a-ad05c81d5a15.png)

## Index

Un index permet de stocker une plus petite portion de données grace a un filtre pour pouvoir au future trouver plus rapidement la donnée que l'on recherche sans avoir a passer sur chaque document de la base.

## Vues

Les vues sont des collections indépendantes en lecture seule qui sont créées a partir de la base la plupart du tempps afin de sauvegarder une requête que l'on ne veut pas avoir a répéter tout le temps. On ne peut que la supprimer si l'on veut la modifier et il est possible d'indexer une vue également.

Indication de la documentation afin de créer une vue :
![image](https://user-images.githubusercontent.com/45734971/149181593-760a64eb-7c54-4332-ae78-32e0479d3693.png)

Pour récupérer une vue d'après la même doc: 
![image](https://user-images.githubusercontent.com/45734971/149181775-54a34914-ff58-4b7f-bcb7-f991def1b2ec.png)




## Installation Mongodb avec Atlas 

Création d'un compte Atlas 

Création du premier projet pour le cours :
![image](https://user-images.githubusercontent.com/45734971/148752303-782b05b0-3693-4186-a95b-5492d55d5942.png)

Création de la première database :
![image](https://user-images.githubusercontent.com/45734971/148752678-afb69d41-98b2-4a6b-a158-6ce2db87c7fe.png)

Donner l'acces a la database a nos adresses ip :
![image](https://user-images.githubusercontent.com/45734971/148753185-180fbb55-9c54-47e6-9165-1c684f297366.png)

Créer le première utilisateur root : 
![image](https://user-images.githubusercontent.com/45734971/148753504-e5ab88cf-3b02-476d-92d3-d1e0defd790f.png)

La database est créée :
![image](https://user-images.githubusercontent.com/45734971/148753424-760126cc-65ce-42de-bb2e-b51418ec290d.png)

Installation de Mongo Compass et import des données : 
![image](https://user-images.githubusercontent.com/45734971/148756761-00b41031-3148-4378-b423-6c89322b82fc.png)

## Questionnaire mongodb

1. De base tout les documents sont affichés si on ne filtre rien :
![image](https://user-images.githubusercontent.com/45734971/148758144-20f3c771-f840-49d9-b0e4-b69818d71680.png)

2. Afficher seulement 4 champs :
![image](https://user-images.githubusercontent.com/45734971/148758817-0348a4af-99db-4fce-a89a-80238ec5b996.png)

3. Afficher seulement 4 champs et en exclure 1 :
![image](https://user-images.githubusercontent.com/45734971/148758720-c364e607-5a7c-4a9d-a426-aece8d884a24.png)

4. Afficher zipcode qui est nesté dans address :
![image](https://user-images.githubusercontent.com/45734971/148759386-09b31796-f3cd-40f6-ba2f-2ff0a4e67584.png)

5. Afficher tout les restaurants du Bronx : 
![image](https://user-images.githubusercontent.com/45734971/148759539-69194f48-5af2-46b7-90bf-a78843117c91.png)

6. Afficher les 5 premiers restaurants du Bronx :
![image](https://user-images.githubusercontent.com/45734971/148760006-44c85665-98a0-48b3-bbde-ff2d9bae505e.png)

7. Afficher les 5 suivants du Bronx :
![image](https://user-images.githubusercontent.com/45734971/148760046-618efa2c-1cb1-4bd8-b06c-180820481e0c.png)

8. Afficher les restaurants avec un score supérieur a 90 :
![image](https://user-images.githubusercontent.com/45734971/148761020-77dab0e0-4f46-4c6a-ba3f-a1adf1616a38.png)










