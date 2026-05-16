
Mettre en place un projet requiert souvent les mêmes étapes d'installation. Quelque soit les technologies utilisés, on retrouve souvent les mêmes fichiers.

**Pour les projet en javascript, on retrouve :** 
#### `package.json`
Il contient les métadonnées du projet et la liste des dépendances.
**`name`** : le nom du projet, doit être en minuscules et sans espaces. C'est l'identifiant du projet sur le registre npm si jamais il est publié.
**`version`** : le numéro de version du projet au format sémantique `MAJOR.MINOR.PATCH`. On incrémente MAJOR pour un changement cassant, MINOR pour une nouvelle fonctionnalité rétrocompatible, PATCH pour un correctif. Ou on met juste la version du projet
**`private`** : si mis à **true**, empêche la publication accidentelle du projet sur le registre npm. C'est une sécurité utile pour les applications web qui ne sont pas destinées à être utilisées comme librairie par d'autres projets.
**`type`** : définit le système de modules utilisé dans le projet. `"module"` active les ES Modules modernes (`import/export`), tandis que `"commonjs"` utilise l'ancien système Node.js (`require`). Si le champ est absent, Node.js retombe sur CommonJS par défaut. C'est simplement pour les imports/exports.
**`main`** / **`module`** / **`exports`** : indiquent le point d'entrée du package, c'est-à-dire quel fichier est chargé quand quelqu'un fait `import monPackage`. Ces champs sont surtout pertinents pour les librairies publiées sur npm, pas pour une application web classique.
**`engines`** : permet de déclarer explicitement les versions de Node.js compatibles avec le projet, par exemple `"node": ">=18"`. C'est une indication pour les autres développeurs et certains outils de CI qui peuvent refuser d'installer si la version ne correspond pas.

**Ses groupes :**
**`scripts`** : raccourcis de commandes qu'on exécute avec `npm run <nom>`, `yarn <nom>` ou `pnpm <nom>` afin d'éviter de réécrire la commande complète. C'est aussi utile pour standardiser les commandes entre développeurs : tout le monde tape la même chose quelle que soit la configuration de sa machine.
**`dependencies`** : dépendances nécessaires en production, c'est-à-dire les packages dont l'application a besoin pour fonctionner chez l'utilisateur final — React, Vue, une librairie de gestion de dates, etc. Ces packages sont inclus dans le bundle final livré au navigateur.
**`devDependencies`** : dépendances utiles uniquement pendant le développement. TypeScript, Vite, ESLint, les frameworks de test comme Vitest… tout ce qui sert à construire, vérifier ou tester le code mais qui n'a aucune raison d'atterrir chez l'utilisateur. Cette séparation allège le bundle final et rend les intentions explicites.
`package-lock.json, yarnlockfile.json`Il contient les versions résolues de toutes les dépendances, leurs sous-dépendances et leurs emplacements d'installation précis. Il constitue un instantané de l'arborescence des dépendances, garantissant ainsi des installations cohérentes.

`Pnpm-lock.yaml` : Le fichier équivalent à package-lock.json mais pour l'utilisation de pnpm.
#### `pnpm-workspace.yaml`
Utilisé dans les **monorepos** : un seul dépôt Git qui contient plusieurs projets ou packages distincts, par exemple un frontend, un backend et une librairie de composants partagée. Ce fichier indique à pnpm où se trouvent ces sous-projets :
Cela permet d'installer toutes les dépendances de tous les sous-projets en une seule commande depuis la racine.
Le second usage, indépendant du premier, est la gestion des **permissions de compilation**. Certains packages ont besoin de compiler du code natif (C++ par exemple) au moment de leur installation via un script automatique. pnpm bloque ces scripts par défaut depuis sa version 9 pour des raisons de sécurité. Le fichier sert alors à lister lesquels sont autorisés ou non. Mettre `false` signifie que tu refuses l'exécution de leur script de compilation — pnpm téléchargera à la place des binaires pré-compilés mis à disposition par les auteurs, ce qui fonctionne très bien dans la grande majorité des cas. En général pnpm génère ce fichier automatiquement, y'aura pas trop à le configurer soit même.

#### `.nvmrc` 
Utiliser la version node.js du projet. Tous les projets utilisant du javascript tournent sur node.js. Chaque projet a sa version spécifique. Node.js permet de faire tourner le projet.

#### `tsconfig.json`
La présence de ce fichier signale que le projet utilise TypeScript. Il configure le compilateur TypeScript (`tsc`) : comment il doit lire, vérifier et transpiler le code.

#### `tsconfig.app.json` et `tsconfig.node.json`
TypeScript a besoin de savoir dans quel environnement ton code va tourner pour vérifier que tu n'utilises pas des outils qui n'existent pas là où ton code s'exécute.

Dans un projet TypeScript, deux environnements coexistent : ton application React tourne dans le **navigateur**, qui donne accès à `window`, `document`, `fetch`… tandis que les fichiers de configuration du projet tournent dans **Node.js** sur ta machine, qui lui donne accès à `process`, `__dirname`… mais pas à `document` ou `window`.

Sans cette séparation, TypeScript mélangerait les deux et ne te préviendrait pas si tu utilisais `document` dans un fichier de configuration, alors que ça planterait à l'exécution puisque Node.js ne connaît pas ce concept.

`tsconfig.app.json` dit donc à TypeScript "ce code tourne dans le navigateur" et `tsconfig.node.json` lui dit "ce code tourne dans Node.js". Le `tsconfig.json` racine référence les deux via `"references"` et sert de point d'entrée unifié pour ton éditeur de code. référence les deux via le champ `"references"` et sert de point d'entrée unifié. Ces fichiers sont généras automatiquement quand on fait pnpm create vite en général, tu n'auras pas à y toucher. D'ailleurs ces fichiers contiennent un élément "**include**" qui permet d'indiquer ce qu'ils concernent : Pour tsconfig.app.json ça sera souvent "src", alors que pour node ça sera "vite.config.ts". Au autre champ "lib" qui précise ce qui peut être utilisé (genre le DOM pour app).


#### `vite.config.ts`
Vite est à la fois un serveur de développement et un outil de build.
En développement, Vite démarre un serveur local très rapide. Sa particularité est de ne pas bundler tout le code au démarrage : il sert les fichiers à la demande en utilisant les ES Modules natifs du navigateur, ce qui rend le démarrage quasi-instantané même sur de gros projets. Il intègre aussi le Hot Module Replacement (**HMR**) qui met à jour uniquement le composant modifié dans le navigateur sans recharger toute la page.
En production, Vite compile et optimise l'ensemble du code : il transpile TypeScript, bundle tous les fichiers en un nombre réduit de fichiers optimisés, minifie le code pour réduire sa taille, et effectue du tree-shaking pour supprimer automatiquement tout le code importé mais jamais utilisé.
Dans `vite.config.ts` on configure les plugins (React, Vue…), les alias de chemins qui doivent correspondre à ceux de `tsconfig.json`, le port du serveur de développement, et les options du build de production.


#### Dossier `node_modules/`
Contient tous les packages installés — dépendances directes et leurs propres sous-dépendances.

##### `config/nginx.conf`
Fichier de configuration de Nginx. On y définit par exemple sur quel port écouter, vers quel service rediriger les requêtes, comment gérer le HTTPS, quels headers ajouter aux réponses, ou comment réécrire certaines URLs. Il est généralement placé dans le dossier du serveur back-end ou à la racine du projet selon l'organisation choisie.


**[[Docker]] :** 
Docker comporte ses propres fichiers. Vu que c'est un service a part entière complexe, il est détaillé dans a propre ficher.


**Pour les projets en PHP Symfony :** 
##### `composer.json`
L'équivalent de `package.json` mais pour PHP. **Composer** est le gestionnaire de dépendances de PHP. Le fichier liste les dépendances du projet (Symfony lui-même, des bundles, des librairies tierces), les scripts personnalisés, et les métadonnées du projet. Le fichier de lock associé est `composer.lock`.
##### `.up`
Souvent un raccourci de commande pour lancer un docker compose up avec certains paramètres.

**Pour les projets en Python :** 
##### `requirements.txt` ou `pyproject.toml`
Fichier qui liste les dépendances Python du projet, avec leurs versions. C'est l'équivalent rudimentaire de `package.json`. On installe tout avec `pip install -r requirements.txt`. Sur les projets plus modernes, ce fichier tend à être remplacé par `pyproject.toml` qui est plus complet.
##### `venv.sh`
C'est  un script personnalisé qui crée et active un **virtual environment** Python. Un venv est un environnement Python isolé pour le projet : ses dépendances sont installées dedans plutôt qu'au niveau global de ta machine, ce qui évite les conflits entre projets qui auraient besoin de versions différentes d'une même librairie. 

**Qu'on retrouve partout :** 
##### `.gitlab-ci.yml` ou `.github/workflows/*.yml` ou `Jenkinsfile`
Ces fichiers définissent la **CI/CD** du projet (Continuous Integration / Continuous Deployment). La CI est un ensemble d'actions automatiques qui se déclenchent à chaque push ou pull request : lancer les tests, vérifier le linting, construire l'application, déployer en production… Le format dépend de l'outil utilisé : `.gitlab-ci.yml` pour GitLab, des fichiers YAML dans `.github/workflows/` pour GitHub Actions, un `Jenkinsfile` pour Jenkins.

##### `.gitignore`
Liste les fichiers et dossiers que Git doit ignorer, c'est-à-dire ne jamais committer dans le dépôt. 
##### `.dockerignore`
Liste les fichiers et dossiers que Docker doit ignorer **lors de la construction de l'image**. Quand Docker construit une image, il commence par copier l'intégralité du dossier du projet dans son contexte de build. Sans `.dockerignore`, il copierait `node_modules/`, le dossier `.git/`, les fichiers de logs, etc. Cela rendrait le build beaucoup plus lent (potentiellement des centaines de Mo à transférer) et alourdirait inutilement l'image finale. Le contenu ressemble souvent au `.gitignore` mais avec des choix adaptés au contexte Docker.


