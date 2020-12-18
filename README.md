        Démarrage de l'application frontend

Clônez le repo sur lequel vous vous trouvez.

À la racine du dossier frontend faites un npm start. (Sous OSX vérifiez que vous avez vscode ou CLT Apple)

Puis npm run serve

Vous rendre sur http://localhost:8080/

        Installation de la base de données

Créez une base de données nommée "groupomania_messenger" dans votre base de données mysql. Command : CREATE DATABASE groupomania_messenger;

Pour finaliser la base de données, executez dans la racine du dossier backend : Command : "sequelize db:migrate --env=production"

Puis lancez le serveur avec "npm start --env=production"

Attention si vous souhaitez modifier l'utilisateur, pensez à le modifier dans le fichier config.json (developement)

        Démarrage de l'application backend

À la racine du dossier backend, faites un node server ou nodemon server (node recquis pour cette aplication)

Vous pouvez utiliser l'application.

        Infos

L'utilisateur Sophie est admin. Voici ses informations pour se connecter : Email : test@test.com, MDP : 29pinpin
