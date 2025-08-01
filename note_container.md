### L'isolation par défaut du réseau d'un conteneur
Pour rendre un ou plusieurs ports disponibles à un service extérieur à Docker il faut les publier.
Pour publier un port il faut utiliser l'option --publish ou -p.
Cela va ajouter une règle dans le pare-feu et va lier un port du conteneur à un port sur la machine hôte. 

La syntaxe est : -p PORT_HOTE:PORT_CONTENEUR.
Par exemple :  -p 8000:80

Permet de connecter le port TCP 80 du conteneur au port 8000 de la machine hôte.

# si nous lançons le conteneur en faisant

docker run -it -p 80:80 nom_container

 Nous pouvons maintenant afficher la page sur localhost.

## L'instruction EXPOSE

L'instruction EXPOSE permet d'informer Docker qu'un conteneur écoute sur les ports spécifiés lors de son exécution.

Par exemple : EXPOSE 80

Signifie que le conteneur écoute le port 80 en TCP

### Quelques rappels sur les commandes

#  Pour lancer notre application en mode détaché (pour reprendre la main dans le terminal) :
docker run -d --name appnode -p 80:80 myapp

#  Pour obtenir un terminal sur notre conteneur en cours d'exécution : 

docker exec -it appnode sh

#  Pour voir la consommation des ressources de nos conteneurs en cours d'exécution :

docker container stats




