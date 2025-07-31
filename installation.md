### Installation sur Ubuntu

## Il faut commencer par supprimer les anciens paquets de Docker pour éviter tout conflit s'il était installé préalablement 

sudo apt-get remove docker docker-engine docker.io containerd runc

## l faut ensuite mettre à jour votre liste de paquets disponibles pour APT :

sudo apt-get update

## Il faut installer certaines dépendances :

sudo apt-get install     ca-certificates     curl     gnupg

##  Ensuite, il faut ajouter la clé GPG de Docker : 

sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Cette clé permet la transmission des paquets de manière signés et chiffrés, garantissant ainsi leurs authenticité, intégrité et confidentialité. Autrement dit, personne ne pourra se faire passer pour Docker pour installer des paquets

##  Il faut ensuite ajouter le répertoire officiel de Docker : 
echo   "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu   "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" |   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

## Et enfin, il ne reste qu'à installer la dernière version :

sudo apt-get update &&sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

##  Si vous n'avez rien, lancez le démon Docker avec cette commande : 

systemctl start docker


