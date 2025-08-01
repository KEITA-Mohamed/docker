### Le fichier .dockerignore

Il faut que ce fichier soit dans le dossier racine du contexte. La plupart du temps au même niveau que le Dockerfile. 
Ce fichier permet de spécifier les fichiers et les dossiers qui doivent être ignorés par Docker. Ils ne seront pas envoyés au démon et Docker n'en aura aucune connaissance. 

Cela permet d'enlever des fichiers volumineux (par exemple node_modules) ou sensibles (des clés, des secrets , etc.) pour les instructions ADD et COPY. 

# La syntaxe est la suivante :

'#' permet d'insérer un commentaire.

*/temp* permet d'exclure les fichiers et les dossiers qui commencent par temp dans les dossiers enfants du répertoire racine du contexte. Par exemple, /app/temp_fichier, /temp/temporaire/, etc. seront exclus. Par contre /app/dossier/temp ne le sera pas.

*/*/temp* permet d'exclure les fichiers et les dossiers qui commencent par temp dans les dossiers qui sont à deux niveaux du répertoire racine du contexte. Donc /dossierA/dossierB/temp sera exclus mais pas app/temp.

fichier? permet d'exclure tous les dossiers et les fichiers dans le dossier racine du contexte qui commence par fichier et sont suivis d'un caractère alphanumérique. Par exemple /fichierA ou fichier1 seront exclus.

** signifie n'importe quel niveau de dossier. Donc par exemple **/*.txt permet d'exclure tous les fichiers terminant par .txt dans n'importe quel dossier du contexte. 

! signifie exception. Cela permet par exemple de ne pas exclure un fichier qui serait normalement exclus suivant les règles précédentes : 
**/*.txt
!README.txt

Ici tous les fichiers .txt sont exclus par la première règle. Mais la deuxième ligne permet d'ajouter une exception pour le fichier README.txt qui sera alors inclus dans le contexte. 

L'ordre des règles est très important dans le .dockerignore. C'est toujours la dernière règle qui l'emporte pour chaque match. Donc, ceci ne fonctionnerait pas :

!README.txt
**/*.txt