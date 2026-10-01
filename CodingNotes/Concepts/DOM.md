Le DOM (Document Object Model) est une représentation en mémoire du document HTML sous forme d'arbres d'objets. C'est une API fournie (ensemble de méthodes pr le javascript) par le navigateur qui permet au Javascript de modifier l'HTML dynamiquement. Le DOM est construit à partir du HTML parsé.
Il relie les pages web aux scripts de programmation en représentant la structure du document.

Concrètement, le navigateur le génère au fur et à mesure qu'il lit (parse) le fichier HTML. Il ne "valide" pas le HTML : s'il y a des erreurs (balise pas fermée…) il les corrige tout seul comme il peut. A ne pas confondre avec le code source brut que : c'est une représentation dynamique et vivante que le navigateur utilise pour déterminer ce qu'il doit réellement afficher à l'écran.
Le DOM permet d'écouter les actions de l'utilisateur : javascript l'utilise pour savoir si un élément a été cliqué. Il permet aussi l'injenction de contenu en temps réelle : au lieu de modifier toute la page, il modifie juste l'élément nouveau, détecter avec JS si des inputs sont vides etc.
Pour un développeur, c'est l'outil qui permet à JavaScript de venir modifier le contenu, le style ou la structure de la page en temps réel après son chargement. On peut l'imaginer comme un arbre logique composé de nœuds et d'objets, où chaque balise HTML devient une branche ou une feuille que l'on peut attraper et manipuler. C'est ce pont indispensable qui transforme un document texte statique en une application web interactive.

**Le DOM est une Web API :** On parle parfois d'API pour le DOM. Au même titre que l'API _Fetch_ (pour faire des requêtes réseau), le DOM fait partie de la boîte à outils que le navigateur "prête" à JavaScript. API pour souligner que c'est un contrat standardisé dans lequel le HTML et le JS peuvent communiquer.
Sans cette interface programmée, ton code JavaScript resterait enfermé dans sa logique mathématique, incapable de toucher à la moindre couleur sur ton écran. 

![[Pasted image 20260510160529.png]]

**React** et **Vue** utilisent un **Virtual DOM**. (Attention, **Vite** n'a rien à voir : c'est un outil de build qui compile le code, pas un framework, il n'a pas de Virtual DOM.)
Le Virtual DOM est une copie légère du DOM en mémoire, sous forme de simples objets JS. Quand le state change :
1. React recrée un nouveau Virtual DOM
2. Il le compare à l'ancien (le **diffing**) pour trouver ce qui a changé
3. Il applique **uniquement** ces changements au vrai DOM
Pourquoi ? Parce que modifier le vrai DOM coûte cher (le navigateur doit recalculer la mise en page et redessiner). Le Virtual DOM permet d'écrire du code déclaratif ("voilà à quoi doit ressembler la page") sans toucher au DOM inutilement. Nuance : c'est pas plus rapide qu'une modif du DOM faite à la main parfaitement ciblée, c'est juste un moyen efficace de ne modifier que le nécessaire automatiquement. Certains frameworks comme Svelte s'en passent complètement.