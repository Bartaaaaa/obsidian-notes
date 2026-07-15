Le DOM (Document Object Model) est une représentation en mémoire du document HTML sous forme d'arbres d'objets. C'est une API fournie (ensemble de méthodes pr le javascript) par le navigateur qui permet au Javascript de modifier l'HTML dynamiquement. Le DOM est construit à partir du HTML parsé.
Il relie les pages web aux scripts de programmation en représentant la structure du document.

Concrètement, le navigateur le génère à partir du fichier HTML dès que celui-ci est chargé et validé. A ne pas confondre avec le code source brut que : c'est une représentation dynamique et vivante que le navigateur utilise pour déterminer ce qu'il doit réellement afficher à l'écran.
Le DOM permet d'écouter les actions de l'utilisateur : javascript l'utilise pour savoir si un élément a été cliqué. Il permet aussi l'injenction de contenu en temps réelle : au lieu de modifier toute la page, il modifie juste l'élément nouveau, détecter avec JS si des inputs sont vides etc.
Pour un développeur, c'est l'outil qui permet à JavaScript de venir modifier le contenu, le style ou la structure de la page en temps réel après son chargement. On peut l'imaginer comme un arbre logique composé de nœuds et d'objets, où chaque balise HTML devient une branche ou une feuille que l'on peut attraper et manipuler. C'est ce pont indispensable qui transforme un document texte statique en une application web interactive.

**Le DOM est une Web API :** On parle parfois d'API pour le DOM. Au même titre que l'API _Fetch_ (pour faire des requêtes réseau), le DOM fait partie de la boîte à outils que le navigateur "prête" à JavaScript. API pour souligner que c'est un contrat standardisé dans lequel le HTML et le JS peuvent communiquer.
Sans cette interface programmée, ton code JavaScript resterait enfermé dans sa logique mathématique, incapable de toucher à la moindre couleur sur ton écran. 

![[Pasted image 20260510160529.png]]

**Vite** et **React** , ces frameworks utilisent souvent un **Virtual DOM**. C'est une copie légère du DOM en mémoire qui permet de calculer les changements très rapidement avant de les appliquer au "vrai" DOM, ce qui améliore grandement les performances.