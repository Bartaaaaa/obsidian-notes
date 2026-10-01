
Un PC est composé de : 

**CPU** : Faut le voir comme le cerveau de tout appareil informatique. Il extrait des instructions de la mémoire, exécute des tâches et renvoie les résultats en mémoire. Il gère toutes les tâches informatiques nécessaires au fonctionnement du système d'exploitation et des aplications. Un processeur peut exécuter plusieurs tâches en simultané en utilisant des threads. Plus il a de coeurs, plus il peut exécuter de tâches en parallèle. C'est aussi lui qui échange les infos entre les différents composants du PC : disque dur, mémoire vive, gpu.
**GPU** : Circuit électronique pour accélérer l'affichage et le traitement d'images sur différents appareils. Avant tout était exécuté sur le CPU, cependant avec l'essor des jeux vidéos et des logiciels vidéos/images, 
Une carte graphique dédiée possède sa propre mémoire vive (VRAM) (un GPU intégré au processeur, lui, utilise la RAM du PC).
La vraie différence avec le CPU c'est **le type de calcul** :
- Le **CPU** a peu de cœurs (8, 16…) mais très puissants et polyvalents : il est très fort pr enchaîner des tâches variées et compliquées les unes après les autres (logique, conditions, OS…).
- Le **GPU** a des **milliers de petits cœurs** simples : chacun est moins malin qu'un cœur de CPU, mais ils font **tous le même calcul en même temps** sur des données différentes (calcul parallèle).
Ex : une image 4K = 8 millions de pixels, et chaque pixel demande à peu près le même calcul → parfait pr le GPU. Pareil pr le machine learning, qui est surtout des multiplications de matrices énormes.
Donc c'est pas un "CPU boosté" : un GPU serait nul pr faire tourner l'OS, et un CPU serait lent pr un jeu en 4K. Image : le CPU c'est quelques chefs cuisiniers, le GPU c'est une armée de commis qui épluchent tous des patates en même temps.
**Carte** **mère** : Colonne vertébrale de l'ordinateur. Elle permet à tous les différents composants du système de communiquer entre eux. Elle détermine aussi les fonctionnalités prises en charge par le PC tel que les options de connectivités : ports USB, type de WIFI, nombre d'emplacement de SSD etc
**Ram** : La mémoire vive (RAM) est essentielle pour permettre à l'ordinateur d'accéder et de stocker des quantités importantes de données. Sa force réside dans sa rapidité (beaucoup plus rapide qu'un SSD), mais elle stocke **moins** (16-32 Go contre 1-2 To pr un SSD) et elle est **volatile** : tout est effacé quand on éteint le PC. Le PC y met les programmes et fichiers en cours d'utilisation pr y accéder vite. Plus elle est grande, plus l'ordinateur peut garder de programmes ouverts en même temps, sans devoir aller relire le SSD (beaucoup plus lent) tout le temps.
**SSD** : Stocker les éléments présents sur l'ordinateur (OS, jeux, fichiers, vidéos) de manière **permanente** : les données restent même PC éteint. Plus lent que la RAM mais beaucoup plus grand.
**Alimentation** : Fournit le courant à l'ensemble des composants de l'ordinateur.
**Ventilation** : Refroidit les composants dans l'ordinateur. Pour le CPU c'est combo pathe thermique + ventirad/water cooling


**BIOS (Basic input/ output system)** :
C'est le microprogramme qui se charge avant le système d'exploitation. Sur les PC récents on parle plutôt d'**UEFI**, qui est le remplaçant moderne du BIOS (interface graphique, support des gros disques, démarrage sécurisé), mais tout le monde continue de dire "BIOS". Le BIOS effectue des processus de démarrage qui vérifient les composants du système et charge le système d'exploitation. Si qqchose est défectueux, il peut s'arrêter sans produire d'affichage vidéo. On peut y accéder au démarrage en répétant la même touche. On peut y paramétrer par exemple l'overclocking du **CPU et de la RAM** (profils XMP/EXPO), activer des paramètres de bas niveau (virtualisation…), définir un nouveau disque de démarrage etc. (L'overclocking du GPU, lui, se fait depuis Windows avec un logiciel comme MSI Afterburner.)

