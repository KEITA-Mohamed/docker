## Lister les réseaux existants

docker network ls

## Inspecter les réseaux

docker network inspect

## Pour inspecter le réseau par défaut, faites :
docker network inspect bridge

#  Passons ensuite à la création de nouveaux conteneurs, en spécifiant cette fois-ci qu'ils sont connectés au réseau que nous venons de créer grâce à l'option --network. 

docker container run --network mynet --name alpine1 -d alpine ping google.fr

Ce premier conteneur va simplement envoyer des requêtes en continu à google.fr. 

# Démarrons maintenant un second conteneur :
docker container run --network mynet --name alpine2 -it alpine sh

# Essayons de contacter alpine1 :
ping alpine1

Génial ! Grâce à la résolution DNS automatique de Docker, le nom alpine1 est résolu automatiquement en une adresse IP et permet la communication avec l'autre conteneur. 

#  Par exemple, pour supprimer le réseau que nous venons de créer il faut faire : 
docker container stop alpine1 alpine2
docker network rm mynet

## Vous pouvez supprimer tous les réseaux d'un coup :
docker network prune




