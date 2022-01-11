# Compte rendu Yann Mongo DB semaine du 10 janvier

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

## Début du projet TP MONGODB 

On commence par réfléchir a quel type de projet pourrait correspondre le sujet de la semaine, ici on doit pouvoir récupérer des données clients dans le cadre de la restauration, il nous faut donc un projet context nous permettant d'enregistrer de nouveaux utilisateurs dans notre base de données.
Le projet aura comme système de gestion de base de données MongoDB car il peut nous permettre de gérer des systèmes assez complexes de base de données et dans un cas comme celui là ou l'ont possède plusieurs miliers de clients/rows, il sera bien plus pertinent qu'un mySql ou autre outil de gestion de base de données moins performant.
On commence par mocker des data clients via n'importe quel site de génération de mock data : 
![image](https://user-images.githubusercontent.com/45734971/148910172-400990a8-ccd8-47b8-97d8-44ca1242ec73.png)

Une fois les données mockés on peut les insérer dans notre nouvelle base de données clients :
![image](https://user-images.githubusercontent.com/45734971/148910347-f27f43e8-049c-497d-b393-b03bb3cd8023.png)
