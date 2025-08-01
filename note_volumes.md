### Les volumes

 Les volumes sont créés et gérés entièrement par Docker. 
 Ils peuvent être créés en utilisant le CLI avec la commande : docker volume create

 ## Création des volumes
 docker volume create nom_volume

 ## Lister les volumes
 docker volume ls

 ## Inspecter des volumes
 docker volume inspect ID_NOM

 ## Supprimer un volume
 docker volume rm ID_NOM

 ## Supprimer tous les volumes
 docker volume prune

 ## Démarrer un conteneur avec un volume nommé
 docker container run -d --name nginx1 --mount source=data1,target=/app nginx

Nous lançons un conteneur en mode détaché avec pour nom nginx1.
Nous montons un volume qui a pour nom data1 et nous ciblons /app dans le conteneur.
Docker va créer un volume data1 automatiquement, le monter sur le dossier /app dans le conteneur. Comme /app n'existe pas il va également le créer automatiquement.
Si vous ne précisez pas le type lors de l'utilisation de --mount, Docker va utiliser par défaut type=volume. 

## Effectuer une sauvegarde des volumes d'un conteneur
 Pour sauvegarder tous les volumes utilisés par un conteneur en cours d'exécution, il suffit de créer un bind mount temporaire et de créer une sauvegarde du volume :

docker container run --rm --volumes-from conteneur1 --mount type=bind,src="$(pwd)",target=/backup alpine tar -cf /backup/backup.tar /data 

--volumes-from conteneur1 va monter tous les volumes montés dans le conteneur1 sur le nouveau conteneur alpine que nous lançons.

tar -cf /backup/backup.tar /data va prendre le volume /data monté sur notre nouveau conteneur, (et qui est le même que celui du conteneur1), et créer une archive dans /backup/backup.tar sur le conteneur.

 Or grâce au bind mount, le dossier backup est lié au répertoire de travail sur l'hôte. Après la suppression du conteneur (option --rm) les fichiers et dossiers situés dans /backup sur le conteneur sont donc conservés sur l'hôte. En l'occurrence backup.tar.

Pour tester ce qui a été archivé, vous pouvez faire sur l'hôte :  tar -tvf backup.tar

Si vous ne voulez pas utiliser --volume-from, vous pouvez également opter pour la forme plus longue : 
docker container run --rm --mount source=data,target=/data --mount type=bind,source="$(pwd)",target=/backup -it alpine tar -czf /backup/backup.tar.gz /data

## Effectuer une restauration d'un volume
Pour restaurer une archive compressée ou non compressée dans un nouveau volume, il suffit de faire :
docker run --mount type=volume,source=restore,target=/data --mount type=bind,source="$(pwd)",target=/backup -it alpine tar -xf /backup/backup.tar --strip-components 1 -C /data 





