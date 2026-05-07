
Mise en place : 
Configurer dans le docker file l'installation du projet et de sa configuration : 
Exemple avec Rocky Linux (système d'exploitation Linux "Entreprise" : stable & sécurisé), on utilise DNF (dandified YUM qui est le gestionnaire de paquets Rocky Linux).
Exemple dans DockerFile : 

![[Pasted image 20260119135628.png]]
Mettre à jour le Docker Compose : 
![[Pasted image 20260119140426.png]]
Par défaut un conteneur Docker est une bulle isolée. Il faut utiliser extra_hosts pour dire à Docker que host.docker.internal correspond à l'adresse IP de mon PC. Permet en gros à Xdebug d'envoyer les données à Ubuntu.

PHP_IDE_config : 
Imaginons que vous ayez trois projets Symfony ouverts en même temps dans PhpStorm. Quand PhpStorm reçoit un signal de debug sur le port 9003, il se demande : _"C'est pour quel projet ? Quel 'Server' et quels 'Path Mappings' dois-je utiliser pour afficher le code ?"_. 
PHP_IDE_CONFIG : C'est une variable d'environnement que PHP envoie à l'IDE lors de la connexion initiale. Elle contient un jeton d'identification. **Résultat :** Dès que la connexion arrive, PHP dit : _"Bonjour, je suis le serveur 'ipdocteurs'"_. PhpStorm répond : _"Ah, je te connais ! Pour toi, je dois transformer le chemin `/home/httpd/...` en `/home/bgrzadziel/...` sur mon écran"_.

Enfin configurer dans les settings le **mapping** qui fait le lien entre **local** (/home/bgrzadzaiel:...) et **conteneur** (/home/httpd). Le port de **débug** (9003 pr xDebug) et l'**état** du téléphone (mettre au vert).
* Dans vsCode il faut penser à configurer un fichier launch.json