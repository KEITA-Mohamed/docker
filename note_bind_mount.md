### Les bind mounts

#  Créez un conteneur avec un bind mount sur un dossier : 
docker container run -it --name alpine1 --mount type=bind,source="$(pwd)",target=/data alpine sh

Notez que $(pwd) est une substitution de commande qui va être remplacée par la valeur de la variable d'environnement pwd, qui contient le chemin absolu du répertoire courant. (Pour en savoir plus voyez le cours Linux). 

Nous montons donc le dossier data de la machine hôte que nous avons créé dans le dossier /data sur le conteneur.
Notez que ce dossier peut avoir le nom que vous voulez, il n'a pas à correspondre au nom sur la machine hôte.

Sur Windows avec Git Bash uniquement, il se peut que le montage ne fonctionne pas en mettant /data. Il faut mettre //data (car Git Bash le transforme sinon en C:\\data). 

### mettre en place une liaison entre les fichiers de notre application  et notre conteneur grâce à un bind mount. 
#  Nous démarrons un conteneur avec la nouvelle version de l'image, sans oublier de publier le port et avec le --mount : 
docker run -p 80:80 --mount type=bind,source="$(pwd)/src",target=/app/src myapp

le Dockerfile pour cette exemple :
FROM node:alpine
WORKDIR /app
COPY ./package.json .
RUN npm install
COPY . .
ENV PATH=$PATH:/app/node_modules/.bin
CMD [ "nodemon", "-L", "src/app.js" ]