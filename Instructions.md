### L'instruction FROM

Tous les Dockerfile doivent utiliser une instruction FROM comme première instruction.

L'instruction FROM permet d'indiquer quelle est l'image parente depuis laquelle vous allez effectuer les modifications spécifiées dans vos instructions.

Autrement dit, il s'agit de l'image de base ou d'origine à partir de laquelle vous allez construire votre image. 

# La syntaxe des arguments passés à FROM est :
FROM image@digest

ou 

FROM image:tag

ou

FROM image


### L'instruction RUN

L'instruction RUN permet d'exécuter toutes les commandes spécifiées dans une nouvelle couche (layer) au-dessus de la dernière image intermédiaire puis de sauvegarder le résultat.

 Il existe deux syntaxes pour l'instruction RUN, la forme shell et la forme exec. 

 ##  La forme exec de RUN

 La forme exec utilise un tableau JSON de mots qui seront ensuite interprétés en une instruction. 
 Elle permet d'exécuter des commandes avec des exécutables sans utiliser nécessairement un shell. 
 Il est obligatoire d'utiliser des guillemets doubles et non des guillemets simples. 

 # La syntaxe est :
 RUN ["executable", "param1", "param2"]
 RUN ["/bin/bash", "-c", "echo Bonjour !"]

 ##  La forme shell de RUN

 La forme shell permet d'exécuter une ou plusieurs commandes par un shell qui est par défaut sh. 

 # RUN et le cache

  Extrêmement important : les instructions RUN ne sont pas invalidées automatiquement à chaque build. 
  Par exemple, si vous avez l'instruction suivante dans un Dockerfile : 

  RUN apt update && apt dist-upgrade -y

   Les commandes shell ne seront exécutées que lors du premier build, ensuite ce sera le cache qui sera utilisé aux prochains builds.

    Pour empêcher l'utilisation du cache il faut utiliser l'option --no-cache lors du build : 

    docker build --no-cache

### L'instruction ADD

L'instruction ADD permet de copier des fichiers, des dossiers ou des fichiers distants en utilisant des URL et de les ajouter au système de fichiers de l'image. 

ADD permet d'automatiquement décompresser des archives locales passées en source avant de les copier dans la destination choisie (à noter que cela ne fonctionne pas pour les URL). 

# La syntaxe est :

ADD SOURCE... DESTINATION

 Notez que pour que Docker considère que la destination est un dossier il faut obligatoirement qu'elle finisse par /

 # Si nous passons un dossier :

 ADD back/ /app/

  Tout le contenu du dossier, y compris les sous-dossiers, seront copiés dans /app/. Le dossier ne sera pas copié, uniquement son contenu. 

  ## Les patterns

  # Par exemple :

  ADD package* /app/

Copiera tous les fichiers commençant par package dans le dossier /app/ du conteneur. 

### L'instruction COPY

L'instruction COPY permet de copier des fichiers et des dossiers et de les ajouter au système de fichiers de l'image. 

##  Les règles de syntaxe sont les mêmes que pour ADD. Les différences entre COPY et ADD sont les suivantes : 

COPY contrairement à ADD, n'accepte pas d'URL comme source. 
COPY contrairement à ADD, ne va pas automatiquement décompresser des archives locales passées comme source. 

### L'instruction WORKDIR

L'instruction WORKDIR permet de modifier le répertoire de travail pour toutes les instructions RUN, CMD, ENTRYPOINT, COPY et ADD. 

 Vous pouvez utiliser plusieurs WORKDIR dans un Dockerfile, dans ce cas chaque WORKDIR s'appliquera pour les instructions jusqu'au prochain WORKDIR. 


### L'instruction CMD

L'instruction CMD permet de fournir la commande exécutée par défaut pour les conteneurs lancés depuis l'image. 
 Il ne peut y avoir qu'une seule instruction CMD par Dockerfile

 Il existe trois formes pour l'instruction CMD : la forme exec, la forme shell et la forme en utilisation conjointe avec ENTRYPOINT (que nous verrons après). 

#  Comme pour RUN, la forme exec est :

CMD ["exécutable","param1","param2"]
Dans ce cas l'exécutable n'est pas lancé par un shell.

# La forme shell est :
CMD commande param1 param2

La forme recommandée par Docker est la forme exec.

### L'instruction ENTRYPOINT

L'instruction ENTRYPOINT permet de configurer un conteneur qui sera lancé comme un exécutable.
Autrement dit, elle permet de configurer la commande qui sera toujours exécutée lorsque vous lancerez un conteneur.

 L'instruction ENTRYPOINT a aussi une forme shell et exec. Cependant, comme la forme shell est déconseillée, nous ne montrerons que la forme exec :

 ENTRYPOINT ["exécutable", "param1", "param2"]

## L'instruction ARG

L'instruction ARG permet de définir des variables qui seront utilisables par l'utilisateur lançant les conteneurs.
Autrement dit, ARG permet de fournir des valeurs lors du build en les passant comme valeurs de variables définies dans le Dockerfile.

Par exemple, si nous avons :
ARG env

L'utilisateur peut ensuite faire lors du lancement :
docker build --build-arg env=prod

 Notez qu'il est possible de définir des valeurs par défaut dans le Dockerfile : ARG env=dev

 Attention ! Il ne faut pas utiliser ARG pour passer des secrets (clés, mots de passe, etc.). Ce n'est pas la manière sécurisée de le faire, que nous verrons plus tard.

### L'instruction ENV

L'instruction ENV permet de définir des variables d'environnement.

La syntaxe est la suivante : ENV CLE="Une valeur"

Vous pouvez utiliser plusieurs instructions ENV :
ENV CLE1="Une valeur1"
ENV CLE2="Une valeur2"

 Ou une seule en déclarant les variables d'environnement en les séparant par des espaces : ENV CLE1="Une valeur1" CLE2="Une valeur2"

La différence avec ARG est que les variables d'environnement sont persistées dans l'image après le build. À l'inverse les ARG ne sont pas persistés. 

### L'instruction LABEL

L'instruction LABEL permet d'ajouter des métadonnées à une image.  C'est-à-dire des informations sur le contenu, l'auteur, la version, etc. de l'image. 

Vous pouvez mettre autant de labels que souhaités, vous pouvez les écrire sur plusieurs lignes, et/ou dans plusieurs instructions LABEL : 

LABEL version="2.3.1"
LABEL auteur="jean@gmail.com"

Ou dans une seule instruction :
LABEL version="2.3.1"       auteur="jean@gmail.com"

Aucune des deux formes n'a d'impact sur la taille ou le nombre de couches (layers), utilisez l'une ou l'autre forme suivante votre préférence.

### Inspecter une image

La commande pour inspecter une image est docker image inspect.
Elle permet d'obtenir toute la configuration de Docker pour sa création. Elle permet notamment d'obtenir tous les hashs des couches de l'image.

Elle permet également d'accéder aux variables d'environnement qui ont été définies.


###  La commande docker container commit
La commande docker container commit permet de créer une image à partir des modifications effectués dans un conteneur.


###  La commande docker container logs
La commande docker container logs permet d'afficher les logs d'un conteneur.

Une option intéressante est --follow ou -f afin de suivre les logs d'un conteneur pendant son exécution. C'est très pratique pour déboguer.

Une autre option est --timestamps ou -t qui permet d'obtenir un timestamp pour chaque ligne de log.

### La commande docker image tag

La commande docker image tag permet de créer un tag pour une image.
En effet, une même image peut avoir plusieurs tags et lorsque vous construisez vos images vous pouvez vouloir garder des versions antérieures. 

La syntaxe est : docker tag SOURCE:TAG CIBLE:TAG

Nous verrons que par convention, les tags des images sont souvent repository/nom:version.

###  La commande docker image history

La commande docker image history permet de lister les couches utilisées pour la construction d'une image. Elle fournit des informations détaillées sur la façon dont l'image a été créée, notamment les instructions exécutées à chaque étape.

Si l'image n'a pas été build localement vous n'aurez pas les images intermédiaires mais seulement les couches. 