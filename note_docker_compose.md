### lancez docker-compose
docker compose up

##  Si vous voulez uniquement les conteneurs lancés par Docker Compose vous pouvez faire : 
docker compose ps

Vous pouvez également lui passer l'option -a pour voir également les conteneurs lancés par docker compose run. 

##  Lancer une commande sur un service avec docker compose run
docker compose run myalpine

Vous pouvez remplacer la commande par défaut de l'image en passant une autre commande en argument : 
docker compose run myalpine ls

##  Stopper les conteneurs lancés avec Docker compose
docker compose down

##  Pour supprimer également les volumes créés par Docker Compose, anonymes et nommés, il suffit de spécifier l'option -v : 
docker compose down -v

# build
l'option de configuration build permet de définir le chemin du contexte de build. Il n'y a rien besoin d'autre si le Dockerfile et dans le même dossier. 

Si le Dockerfile a un autre nom et / ou si le contexte de build est dans un autre dossier, il faut utiliser la configuration plus détaillée, par exemple : 

build:
 context: ./dossier
 dockerfile: Dockerfile.dev

##  Construire les images avec docker compose build

La commande docker compose build permet de construire toutes les images spécifiées dans le fichier de configuration (dans les configurations build) et de les taguer pour une future utilisation. 

## Publier des ports
Vous pouvez définir quels ports doivent être publiés pour un service dans le fichier de configuration docker-compose.yml.
Pour ce faire, il suffit d'utiliser ports. Le format est toujours HOTE:CONTENEUR puis par défaut TCP. Vous pouvez préciser UDP avec /upd : 

ports:
  - "80:80"
  - "8080:3000/udp"

## Créer des volumes anonymes
 Pour créer un volume anonyme il faut simplement déclarer un volume de type volume et de spécifier uniquement une target (ou destination / dst) et pas de source. Dans ce cas Docker Compose créera automatiquement un volume anonyme : 

services:
a:
  image: alpine
  command: ['ls']
b:
  build:
    context: ./backend
    dockerfile: Dockerfile
    args:
      - FOLDER=test
    labels:
      - email=jean@gmail.com
  volumes:
    - type: bind
      source: ./data
      target: /app/data
    - type: volume
      target: /app/data2

## Créer des volumes nommés

 Pour créer un volume nommé, il faut déclarer dans une configuration au même niveau que services une clé volumes : ce sont les volumes nommés qui seront automatiquement créés par Docker Compose. 

 volumes:
    - type: bind
      source: ./data
      target: /app/data
    - type: volume
      target: /app/data2
    - type: volume
      source: data3
      target: /app/data3
volumes:
  data3:
 Ici data3 est un volume nommé créé et qui est monté dans le service b. 

## Utiliser des volumes nommés déjà créés

 Si vous ne voulez pas que Docker Compose crée un nouveau volume nommé mais utilise un existant (que vous avez créé en dehors de Docker Compose), il faut préciser external: true dans la configuration, de cette manière : 

 volumes:
    - type: bind
      source: ./data
      target: /app/data
    - type: volume
      target: /app/data2
    - type: volume
      source: data3
      target: /app/data3
volumes:
  data3:
    external: true

 Ici data3 ne sera pas créé par Docker Compose. Il utilisera le volume déjà créé. Si aucun volume avec ce nom n'existe, vous aurez une erreur. 

## Utiliser des variables d'environnement

 Vous pouvez utiliser des variables d'environnement dans votre Dockerfile ou votre fichier docker-compose.yml, par exemple pour définir une version d'image : 

 backend:
  image: "node-app:${NODE_APP_VERSION}"

##  Définir la valeur des variables d'environnement avec environment
backend:
  environment:
    - NODE_APP_VERSION=2.2.3

##  Utiliser un fichier externe avec env_file et .env

Par défaut, Docker Compose va lire les variables d'environnement dans le fichier .env situé au même niveau que le docker-compose.yml.
Si vous voulez spécifier un fichier différent il faut utiliser la clé de configuration env_file. Par exemple : 

backend:
  env_file:
    - config/env.dev


