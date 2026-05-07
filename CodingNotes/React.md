



**Comment créer une application React ?** 
React de base est juste une librairie, mais on peut en faire une application grâce à des frameworks qui se basent dessus.
La documentation officielle préconise d'utiliser un framework qui permettent d'utiliser à plein potentiels les composants de React.
On peut utiliser Next.js. Next.js est un framework construit autour de React qui simplifie plein de fonctionnalités qui ne sont pas incluses dans React de base et qui permet le développement FullStack. Il permet le rendu hybride : côté client et SSR, l'optimisation des performances (fractionnement automatique du code), permet de créer des composants qui s'exécutent uniquement coté serveur, renforce la sécurité en gardant la logique cote serveur, favorisent le SEO grâce au HTML immédiatement disponible.
Autre caractéristiques que tout framework doit contenir : support intégré de typescript, tests de qualité de code avec Jest et Cypress, internationalisation et support des langues, etc.
![[Pasted image 20260301102658.png]]
On peut aussi utiliser React router. Pour créer l'app faut lancer la commande npx create-react-router@latetest par ex.

**1. L'illusion du "tout côté client" (Le serveur statique)** Même une application React "côté client" (avec un backend PHP/Node séparé) utilise deux serveurs :

 **Le serveur d'API (Backend) :** Gère la base de données et renvoie la donnée brute (JSON).
 **Le serveur web "statique" (Frontend) :** (ex: Vercel, Nginx). Son unique rôle est de stocker tes fichiers frontend compilés (HTML vide, CSS, JS) et de les envoyer au navigateur de l'utilisateur.

**2. Pourquoi React renvoie du JS et pas du HTML ?**

-Les navigateurs ne comprennent pas le code `.tsx` (TypeScript + HTML mélangé).   
 Avant la mise en ligne, des outils (Vite, Webpack) **compilent** tous tes fichiers `.tsx` en un seul gros fichier JavaScript standard (appelé le _bundle_).
 Les bundlers : 
**Qu'est-ce qu'un Bundler ?** C'est un outil qui prend tous tes fichiers source (`.tsx`, `.css`, images) et les "empaquette" en un ou plusieurs fichiers JavaScript standards (le _bundle_) compréhensibles par le navigateur.    
**Les outils principaux :**    
**Webpack :** Le standard historique (celui derrière l'ancien `Create React App`). Très puissant, mais peut être lourd à configurer et lent à compiler sur de gros projets.        
**Vite (le standard actuel) :** Extrêmement rapide. En développement, il utilise les modules ES natifs du navigateur, ce qui permet un _Hot Module Replacement (HMR)_ quasi instantané (l'application se met à jour à l'écran en millisecondes quand tu sauvegardes ton code). C'est aujourd'hui l'outil recommandé pour initier un projet React pur.

-Contrairement aux sites classiques (ex: PHP) où le serveur construit et renvoie un fichier `.html` complet et prêt à l'emploi, le serveur statique de React ne renvoie qu'une coquille HTML vide (`<div id="root"></div>`) accompagnée de ce gros fichier JS.

**3. Pourquoi React pur est mauvais pour le SEO ?**

Quand le **crawler d'indexation** (ex: Googlebot) parcourt le site, le serveur statique lui renvoie le fichier `index.html` initial.   Le crawler voit le `<div id="root"></div>`, conclut qu'il n'y a pas de texte ni de contenu, et s'en va. Pour voir le contenu, il faudrait que le robot télécharge le JS et l'exécute pour qu'il "fabrique" les balises HTML. Google sait un peu le faire, mais c'est très lent et pénalise fortement le référencement naturel.    

**4. Pourquoi dit-on que React est une SPA (Single Page Application) ?** 

Une application React ne possède qu'**une seule vraie page HTML** (celle envoyée au tout début). La navigation vers d'autres pages n'est qu'une illusion créée par JavaScript.

**Comment les composants sont-ils chargés ? (Bundle vs Code Splitting)**

**Le comportement par défaut (Initial Load) :** Historiquement, lors de la première visite, le navigateur télécharge le fichier JS principal en entier (le _bundle_ global). Ce fichier contient la logique de **tous** les composants de l'application (Accueil, Contact, Profil...).
**Avantage :** Une fois téléchargé, la navigation est instantanée. Clic sur "Contact" : React **démonte** (unmount) le composant Accueil et **effectue le rendu** (render) du composant Contact dans le DOM, sans aucune nouvelle requête réseau vers le serveur statique.
**Inconvénient :**  Si l'application est très volumineuse, le poids du fichier JS devient énorme. Le temps de chargement initial (_Time to Interactive_) sera long, créant une mauvaise expérience utilisateur (écran blanc prolongé).

**L'optimisation indispensable : Le Code Splitting (Fractionnement du code)** Pour résoudre ce problème de lenteur initiale, on utilise le _Code Splitting_ (souvent implémenté avec `React.lazy` et `Suspense`).
**Le concept :** Au lieu de créer un seul énorme fichier JS, le Bundler (Vite, Webpack) va diviser l'application en de multiples petits fichiers appelés des **chunks**.

**En pratique** (Lazy Loading) : Le navigateur ne télécharge initialement qu'un tout petit fichier JS contenant le moteur React et le code strictement nécessaire pour afficher la page d'Accueil. Le code de la page "Contact" (un _chunk_ séparé) ne sera téléchargé à la volée **que** si l'utilisateur décide de cliquer sur le lien "Contact".

**5. La solution : Utiliser un Framework (ex: Next.js)** Pour pallier les défauts de React pur (SEO, lenteur initiale), la documentation officielle recommande un framework comme Next.js ou React Router (v7).
Next.js introduit le **SSR (Server-Side Rendering)** et les **RSC (React Server Components)**.    

Au lieu d'envoyer un HTML vide, le serveur Next.js exécute lui-même le code React et envoie un fichier HTML rempli de données au navigateur. C'est parfait pour le SEO et l'affichage instantané !

**1. Comment appelle-t-on ce type de technologie ?** On parle d'une **Architecture orientée composants** (Component-Based Architecture) et d'une approche **Déclarative**.

- **Déclaratif vs Impératif :** En React, on "déclare" à quoi l'interface doit ressembler en fonction de l'état des données (le _State_), et React se charge de mettre à jour le DOM tout seul. (À l'inverse du JavaScript "Vanilla" où l'on doit sélectionner manuellement les éléments avec `document.getElementById` pour les modifier, ce qui est une approche "impérative").
Vue.js, Angular, Svelte sont des frameworks component based architecture.
React est utilisé par Meta (créé par React), Netflix, Uber,... Surtout pour la réutilisabilité des composants adapté pour tous les types d'écrans. Uber aussi pour la gestion complexe d'états : react gère bien les changements d'états fréquents.

**2 - Quelle est la différence entre state et props ?** La principale différence réside dans le fait que les props sont des données immuables passées de parent à enfant pour la configuration initiale, tandis que le state est utilisé pour gérer les données internes d'un composant qui peuvent changer au fil du temps et qui déclencheront le rendu du composant lorsque modifiées. Les props sont destinées à la communication entre composants, tandis que le state est destiné à la gestion des données locales d'un composant.

**3 - Décrire le fonctionnement du hook "useEffect" ?**  Il permet de gérer les effets secondaires dans les composants fonctionnels. Les effets secondaires sont des actions qui se produisent en dehors du cycle de vie de rendu normal, comme l'interaction avec des API externes, la modification du DOM, la gestion des abonnements, etc. useEffect vous permet de spécifier des fonctions à exécuter après le rendu ou lors de la mise à jour du composant. Les dépendances peuvent être utilisées pour contrôler quand l'effet doit être réexécuté. La tableau de dépendance (deuxième argument du useEffect) indique quand le hook doit être utilisé. Un tableau vide signifie au chargement du composant, une valeur signifie à son changement, et rien mettre signifie à chaque modification du DOM (à éviter car ralentit bcp la page).
### C'est quoi la "gestion des abonnements" ?

Un abonnement (subscription), c'est quand ton composant se met sur écoute pour surveiller un événement qui se passe à l'extérieur.
- Écouter le scroll de la souris (pour faire apparaître un bouton "Retour en haut").    
- Écouter le redimensionnement de la fenêtre (`window.addEventListener('resize')`).
- Se connecter à un WebSocket (pour un chat en temps réel).

**💡 Le point bonus pour l'entretien (La fonction de nettoyage) :** Si on s'abonne à quelque chose, il faut absolument **se désabonner** quand le composant disparaît de l'écran (sinon on crée une "fuite de mémoire", le navigateur va ralentir). On fait ça en retournant une fonction à la fin du `useEffect`.

#### 1. Pourquoi le Service est-il asynchrone ?

En JavaScript, les opérations réseau (requêtes HTTP) sont des opérations d'**Entrée/Sortie (I/O) non-bloquantes**.

- **La Promesse (Promise) :** Le service renvoie un objet `Promise`. Il représente un état intermédiaire : l'opération est lancée, mais le résultat n'est pas encore disponible. Cela permet à la boucle d'événements (Event Loop) de continuer à traiter d'autres tâches (comme les animations ou les clics) sans geler l'interface.
    
- **Les Interfaces (TypeScript) :** On type le retour du service avec une `interface` (ex: `Promise<User[]>`). Cela définit un **contrat de données** entre le backend et le frontend. En entretien, expliquez que cela sécurise le code en garantissant que les propriétés manipulées existent réellement sur l'objet reçu après la désérialisation du JSON.
    

#### 2. L'implémentation au sein du `useEffect`

**Pourquoi ne pas déclarer le `useEffect` en `async` ?** La signature de la fonction passée à `useEffect` est stricte : elle doit retourner soit `undefined`, soit une fonction de nettoyage (cleanup function). Une fonction déclarée `async` retourne **systématiquement** une Promesse. Si vous passez une fonction `async` à `useEffect`, React recevra une Promesse au lieu d'une potentielle fonction de nettoyage, ce qui brise le contrat interne du hook et peut générer des comportements imprévisibles.

**L'utilisation de la fonction interne (Wrapper) :** Pour contourner cette restriction, on déclare une fonction asynchrone **à l'intérieur** du hook.

- **Le mot-clé `await` :** Il suspend l'exécution de cette fonction interne jusqu'à ce que la Promesse du service soit résolue (fulfilled) ou rejetée (rejected). Cela permet d'écrire un code asynchrone qui se lit de manière séquentielle (synchrone), facilitant la lecture et la maintenance.
    
- **L'exécution finale :** Déclarer la fonction (`const fetchData = async () => {...}`) ne fait que la stocker en mémoire. Il est impératif de l'appeler immédiatement après sa définition (`fetchData();`) pour déclencher l'exécution réelle de la logique.


#### 3. Structure de code standard (Pattern async/await)

```
useEffect(() => {
  // Définition de la fonction asynchrone interne
  const loadData = async () => {
    try {
      // await suspend l'exécution jusqu'à la résolution de la promesse
      const data: User[] = await userService.getUsers();
      setUsers(data);
    } catch (error) {
      // Gestion des rejets de la promesse
      handleError(error);
    }
  };

  loadData(); // Déclenchement impératif
}, []); // Dépendances vides pour un cycle de montage uniquement
```

#### 4. L'alternative : La chaîne de Promesses (`.then()`)

Il n'est pas obligatoire d'utiliser une fonction `async` interne. On peut traiter la Promesse via la méthode `.then()`.

```
useEffect(() => {
  userService.getUsers()
    .then((data) => {
      setUsers(data);
    })
    .catch((error) => {
      handleError(error);
    });
}, []);
```

**Comparaison pour l'entretien :**

- **`.then()` :** Plus proche du fonctionnement natif des Promesses, il évite de déclarer une fonction intermédiaire. Cependant, sur des enchaînements complexes (plusieurs appels dépendants), il peut mener à un "Callback Hell" ou à une structure de code moins lisible.
    
- **`async/await` :** Offre une syntaxe plus moderne et "plate". C'est l'approche privilégiée aujourd'hui car elle facilite la gestion des erreurs avec les blocs `try/catch`, rendant le flux de données plus explicite.

**6 - Quels packages avez-vous déjà utilisés et quelle est leur utilité ?**
**React Router :** Cette bibliothèque permet de gérer la navigation et les routes dans une
application React. Vous pouvez créer des routes pour différents composants et gérer la
navigation entre eux de manière déclarative.
Ce package comprend trois hooks : 
**useLocation** =  renvoie url actuel,
**useNavigate**= renvoie vers l'url indiqué, 
**useParams** = renvoie le parametre dans l'url précisé,
Et possède **BrowserRoute** qui encapsule **Routes** qui encapsule **Route** qui définit les routes du projet dans le fichier App.tsx. On passe à **Route** le composant et la route qui y mène.
Le header et le footer sont définis en dehors de l'élément **Routes**

**Redux**  : Redux est une bibliothèque de gestion d'état pour les applications React. Elle
permet de gérer efficacement l'état global de l'application, en particulier pour les applications
à grande échelle. Redux enveloppe toute l'application pour que chaque sous composant puisse y avoir accès. On import **Provider** et on englobe le fichier main.tsx avec en passant en parametre le store qu'on construit dans un autre fichier.
Les éléments sont stoqués dans la mémoire vive (ram) du navigateur.

**Le concept de Redux :** 
Redux vise à résoudre le problème de **Prop Drilling**. Dans une application React standart, les données circulent du haut vers le bas.
Si un composant tout en bas a besoin d'une info stockée tout en haut (comme "L'utilisateur est-il connecté ?"), vous devez passer cette info à travers tous les composants intermédiaires, même s'ils n'en ont pas besoin.

Redux résout cela en sortant l'état de l'arbre des composants pour le mettre dans un magasin centralisé (le Store). N'importe quel composant peut y accéder directement, peu importe où il se trouve.

Le concept d'action : C'est un objet qui décrit "ce qu'il s'est passé". Il a toujours un `type`.

- _Exemple :_ `const action = { type: "user/login", payload: "Thomas" }`
![[Pasted image 20260108093905.png]]Redux utilise useDispatch pour stocker les données et useSelector pour les récupérer depuis le store.

**Axios** : Axios est un client HTTP qui facilite les requêtes HTTP dans une application React. Il
est couramment utilisé pour interagir avec des API REST.
Il existe un outil **natif** intégré directement dans tous les navigateurs modernes : **`fetch()`**.
Beaucoup d'équipes décident de ne pas installer Axios pour ne pas alourdir le projet avec un package supplémentaire, et utilisent simplement `fetch`.
**La différence en bref :**
- **`fetch` (L'outil natif) :** Fait très bien le travail, mais il est un peu plus bavard. Par exemple, tu dois transformer manuellement la réponse en JSON à chaque fois (avec `response.json()`), et il ne considère pas une erreur 404 comme une vraie "erreur" réseau.
- **`Axios` (Le package externe) :** C'est le grand confort. Il transforme le JSON tout seul, gère mieux les erreurs, et surtout, il possède des "intercepteurs" (très pratiques pour ajouter automatiquement un token de sécurité à toutes tes requêtes vers ton API).

**i18next** : i18next est une bibliothèque de gestion des traductions pour React. Elle permet de
gérer facilement les chaînes de texte traduites dans votre application. _Petit point culture de dev :_ On l'appelle **i18n** car il y a exactement 18 lettres entre le "i" et le "n" du mot anglais "_internationalization_".

Au lieu d'écrire du texte en dur dans ton code, tu écris des "clés".
- **Dans ton code :** `<h1>{t('titre_accueil')}</h1>`
- **Dans un fichier `fr.json` :** `{"titre_accueil": "Bienvenue sur mon site"}`
- **Dans un fichier `en.json` :** `{"titre_accueil": "Welcome to my website"}`

C'est la référence absolue dans l'écosystème React (via le package `react-i18next`) pour traduire un site dynamiquement sans le recharger.

**UseIntl** : Un hook (natif de react) qui fait la même chose que 18next mais un peu différemment.

9 - Quel(s) outil(s) permet de gérer les différentes versions de node en local ?
nvm, volta
10 - Quel(s) outil(s) permet de gérer les packages d'une application React ?
npm, yarn (plus rapide que npm), pnpm (performant npm)