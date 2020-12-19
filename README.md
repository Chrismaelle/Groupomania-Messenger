**_Environnement_**

Backend :
Ce projet est développé avec NodeJS, Express, Sequelize et Mysql2

Frontend :
Ce projet est développé avec VueJS

**_Prérequis_**

L'installation de NodeJS, Express, Sequelize et Mysql est requise sur votre ordinateur.

**Démarrage de l'application frontend**

Clônez le repo sur lequel vous vous trouvez.

À la racine du dossier frontend faites un npm install

Puis npm run serve

Vous rendre sur http://localhost:8080/

**Installation de la base de données**

Créez une base de données nommée "groupomania_messenger" dans votre base de données mysql. Command : CREATE DATABASE groupomania_messenger;

Pour finaliser la base de données, executez dans la racine du dossier backend : Command : "sequelize db:migrate --env=production"

Puis lancez le serveur avec "npm start --env=production"

Attention si vous souhaitez modifier l'utilisateur, pensez à le modifier dans le fichier config.json (developement)

**Démarrage de l'application backend**

À la racine du dossier backend, faites un npm install

Renomez le ficher .env.example en .env et complétez les champs DB_USER=root, DB_MDP=29pinpin, SECRET_TOKEN=JHGJHDF678SGD785D6G

Faites un node server ou nodemon server (node recquis pour cette aplication)

Vous pouvez utiliser l'application

**Infos**

L'utilisateur Sophie est admin. Voici ses informations pour se connecter : Email : test@test.com, MDP : 29pinpin
