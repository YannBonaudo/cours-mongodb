# Compte rendu Yann Mongo DB semaine du 10 janvier

## Génèse
On commence par réfléchir a quel type de projet pourrait correspondre le sujet de la semaine, ici on doit pouvoir récupérer des données clients dans le cadre de la restauration, il nous faut donc un projet context nous permettant d'enregistrer de nouveaux utilisateurs dans notre base de données et potentiellement d'utiliser ces données dans l'application.
Le projet aura comme système de gestion de base de données MongoDB car il peut nous permettre de gérer des systèmes assez complexes de base de données et dans un cas comme celui là ou l'ont possède plusieurs miliers de clients/rows, il sera bien plus pertinent qu'un mySql ou autre outil de gestion de base de données moins performant.

On commence par mocker des data clients via n'importe quel site de génération de mock data : 
![image](https://user-images.githubusercontent.com/45734971/148910172-400990a8-ccd8-47b8-97d8-44ca1242ec73.png)

Une fois les données mockés on peut les insérer dans notre nouvelle base de données clients :
![image](https://user-images.githubusercontent.com/45734971/148912663-ff5c8ce4-564d-458f-bb20-b809826c65b9.png)

On se rend compte que les données mockés ne sont pas correct et que la latitude et la longitude ne peuvent pas être lue par le logiciel car probablement les latitudes et longitudes sont dans le mauvais ordre : 
![image](https://user-images.githubusercontent.com/45734971/148924830-382fb117-c2b7-48bb-94cf-c8c0d0add085.png)

## Géospatialisation des données 

Nous allons avoir besoin de spatialiser les données gps, pour cela MongoDB mets a disposition les requêtes Géospatiale, pour fonctionner elles nécéssites des données géométriques (point, polygone ou multiline) qui permettent au final de calculer plusieurs choses, des distances, des radiants, des zone géographiques

Nous allons tenter d'utiliser les géodatas récupérées des utilisateurs sous forme longitude/latitude& pour les lire avec charts. 

Connexion de MongoDB Charts a notre base de données clients 
![image](https://user-images.githubusercontent.com/45734971/148925447-80d7f630-fc77-49d5-a080-6fa41e998769.png)

Prise en main de charts :
![image](https://user-images.githubusercontent.com/45734971/148926316-536ef77e-1e2d-4441-80da-77d7e6deadbf.png)

Charts nous permet de créer des graphiques grâce aux données qu'on lui confie, par exemple grace aux données gps des clients, savoir ou le traffic est le plus présent, ou le moins présent. Dans le cadre d'un projet, cela permet de savoir par exemple où centrer le marketing de la chaine de restaurant, par exemple là ou le taux de clients est le plus bas on peut penser a peut etre ouvrir un restaurant afin de fideliser plus de clients ou dans le cadre d'une ville ou métropole, de savoir ou placer les pubs du restaurant afin que plus de clients entendent parler de l'établissement.

Liaison de nos données de coordonnées des clients lus sur charts : 
![image](https://user-images.githubusercontent.com/45734971/148927663-56f09425-8586-4042-9dc3-688f9563657f.png)

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

## Début du questionnaire

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










