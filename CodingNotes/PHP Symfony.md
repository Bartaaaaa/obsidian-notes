**2 - Quelle est la différence entre un Listener et un Subscriber ?**
La principale différence réside dans le fait qu'un Listener est spécifique à un événement particulier et doit être configuré individuellement pour chaque événement, tandis qu'un Subscriber peut écouter plusieurs événements et est configuré une seule fois pour tous ces événements. Le choix entre les deux dépend de l'architecture de votre application et du niveau d'organisation souhaité pour la gestion des événements.
**Exemple d'utilisations :** 
Subscriber : Une classe qui gère la logique d'un domaine complet sur **plusieurs** événements (ex: Un `UserSubscriber` qui écoute la connexion, la déconnexion et la modification de mot de passe). Un exceptionSubscriber qui formate le format de renvoie de réponse pour chaque réponse -> traduire des erreurs brut symfony  qui renvoient une page d'erreur  en un JSON.
Listener : Une classe qui ne fait qu'**une seule** chose très spécifique sur un seul événement (ex: Un `WelcomeEmailListener` déclenché uniquement à l'inscription).

**3 - A quoi sert Symfony Messenger ?**
Symfony Messenger est un composant de Symfony qui facilite la mise en œuvre de la
communication asynchrone au sein d'une application web. Il est conçu pour gérer les tâches
de fond, les messages entre différents services et les traitements asynchrones de manière
simple et efficace.

Symfony Messenger est un composant implémentant le pattern _Message Queuing_. Il permet de déléguer des traitements lourds ou bloquants à des processus exécutés en arrière-plan (asynchronisme), évitant ainsi de bloquer le thread de la requête HTTP principale de l'utilisateur.

**Architecture et flux d'exécution (Les composants clés)**

- **Le Message (DTO) :** Une classe PHP simple (Data Transfer Object) dont le seul rôle est de transporter les données sérialisables nécessaires à l'exécution de la tâche (ex: un ID utilisateur, un chemin de fichier). Il ne contient aucune logique métier.
- **Le MessageBus (Dispatcher) :** Le service central appelé par l'application (souvent depuis un Controller). Son rôle est de recevoir le Message et de l'acheminer (Routing) vers la bonne destination. Dans un contexte asynchrone, le Bus sérialise le Message et l'envoie vers un **Transport**. (Par ex appelé depuis le controlleur)
- **Le Transport (Message Broker / File d'attente) :** Le système de stockage temporaire. Il retient les messages en file d'attente jusqu'à ce qu'ils soient traités. Ce système peut être la base de données (Doctrine), Redis, RabbitMQ (AMQP), ou Amazon SQS.
    
- **Le Worker (Le processus de consommation) :** C'est un élément critique. Il s'agit d'un script en ligne de commande (`php bin/console messenger:consume`) qui **doit tourner en permanence** sur le serveur d'hébergement (souvent maintenu en vie par un gestionnaire de processus comme _Supervisor_ ou _Systemd_). Il scrute (poll) le Transport en continu pour voir si de nouveaux messages sont dans la file d'attente.
- **Le Handler (La logique métier) :** Une classe taguée avec l'attribut `#[AsMessageHandler]`. Lorsqu'un Worker récupère un Message dans le Transport, il le désérialise et le transmet au Handler correspondant. C'est ici que le code lourd (envoi de mail, traitement) est réellement exécuté.

**3. Le flux asynchrone étape par étape :**

1. L'application (Controller) instancie un Message et le passe au `MessageBus`.
2. Le Bus envoie le Message dans le Transport (la file d'attente). La requête HTTP se termine immédiatement et le serveur répond au client.
3. De son côté, le Worker (qui tourne en boucle infinie) détecte le nouveau Message dans le Transport.
4. Le Worker l'extrait et l'envoie au Handler.
5. Le Handler exécute la tâche complexe en arrière-plan.

**4. Cas d'utilisation (Use Cases)**

- Envois de courriels (interaction avec un serveur SMTP lent).
- Génération de fichiers lourds (exports Excel, PDF volumineux).
- Traitements multimédias (redimensionnement d'images, encodage vidéo).
- Appels à des API tierces (synchronisation avec un CRM/ERP, envois de Webhooks).

Les voter

**4 - Expliquer le principe des migrations de Doctrine**
Les migrations de Doctrine sont un outil puissant permettant de gérer l'évolution de la structure de la base de données dans le cadre d'une application PHP utilisant **Doctrine ORM**. Elles facilitent la création, la modification et la suppression de tables, de colonnes et d'index tout en préservant les données existantes.

Concrètement, Doctrine compare la version actuelle de la base de données avec la version attendue (définie par vos entités PHP), puis génère et exécute les requêtes SQL nécessaires pour synchroniser la base avec votre code.

Le cycle de vie d'une migration s'articule autour de trois actions clés :

- **Générer :** `php bin/console make:migration` _Compare vos entités avec la base de données et crée un nouveau fichier de migration contenant le code SQL de mise à jour._ (make:entity pour mettre à jour une table php (suivi d'une migrate))
- **Exécuter :** `php bin/console doctrine:migrations:migrate` _Applique à la base de données tous les fichiers de migration qui sont en attente._
- **Vérifier :** `php bin/console doctrine:migrations:status` _Affiche l'état actuel de vos migrations (exécutées, manquantes, etc.)._
**Réversibilité (`up` et `down`) :** Chaque fichier généré contient une méthode `up()` pour appliquer les modifications, et une méthode `down()` pour les annuler (rollback) en cas d'erreur.

**5 - Quelle est la commande pour installer un package ?**
composer require 
**6 - Avec quel framework peut-on réaliser des tests unitaires ?** 
PHPUnit 
**7 - Quel outil peut être utilisé pour débugger ?** 
XDebug, Zend Debugger 
**8 - A quoi sert Doctrine ?** 
Doctrine est une bibliothèque de mapping objet-relationnel (ORM) en PHP. Elle facilite la manipulation et la gestion des données dans une application PHP en permettant de travailler avec des bases de données de manière orientée objet.

**9 - A quoi sert le fichier "services.yaml" ?** 
Le fichier "services.yaml" est un fichier de configuration utilisé dans le framework Symfony pour définir et configurer les services de votre application. Les services sont des objets ou des composants réutilisables qui effectuent diverses tâches au sein de votre application, tels que la gestion de la base de données, le traitement des formulaires, la gestion de la sécurité, etc. Le fichier "services.yaml" permet de déclarer ces services et de spécifier comment ils doivent être instanciés et configurés.
Exemple : 
![[Pasted image 20260322114701.png]]
- **`_defaults` (L'automatisation)** : Active l'`autowire` (injection automatique des dépendances entre les classes) et l'`autoconfigure` (Symfony devine le rôle des classes, comme les Controllers ou les Listeners).
- **`bind` (Variables globales)** : Permet de lier des variables d'environnement (ex: clés JWT, chemins de dossiers) à des noms de variables précis (ex: `$jwtPrivateKeyPath`). Dès qu'un service demande cette variable dans son constructeur, Symfony l'injecte automatiquement.
- **`App\` (Déclaration des services)** : Indique à Symfony de transformer toutes les classes du dossier `src/` en services, en **excluant les Entités** (`Entity`), car ce sont de simples objets de données et non des outils.
- **Configurations manuelles spécifiques** : Utilisées quand la configuration automatique ne suffit pas.
    - **`CustomLoginLinkAuthenticator`** : On force manuellement l'injection de l'URL du front-end (`$fontendUrl`) et on lui applique un tag de sécurité.
    - **`EnumNormalizer`** : On lui attribue manuellement un tag avec une **priorité de 100** pour forcer Symfony à l'exécuter avant les autres normaliseurs.
**Définir des paramètres globaux (`parameters:`) :** Créer des variables constantes réutilisables dans toute ton application ou dans tes templates Twig (ex: l'email de l'administrateur, le nombre d'articles par page, le nom du site).

**10 - A quoi sert l'opérateur nullSafe ? Comment l'utiliser ?** 
L'opérateur nullsafe est une fonctionnalité introduite dans PHP 8.0 pour simplifier la manipulation de valeurs potentiellement nulles (null) de manière concise et sécurisée. Il permet d'appeler des méthodes et d'accéder à des propriétés sur des objets sans avoir à vérifier si l'objet lui-même est nul. Cela facilite la gestion des erreurs liées à des valeurs nulles. **$object?->methodName();**

**11 - Où sont stockés les logs et le cache ? Quelle est la commande pour vider le cache ?** 
Dans le dossier var, le fichier dev.log. 
La commande pour vider le cache : php bin/console cache:clear

**13 - Qu'est-ce qu'un attribut ?**
Dans PHP 8, les attributs sont une nouvelle fonctionnalité qui permet de déclarer de manière explicite des métadonnées ou des annotations pour les classes, les méthodes, les propriétés et d'autres éléments du code. L’attribut est considéré comme du code PHP contrairement aux annotations qui sont considérées comme string. Exemple avec Doctrine : 
#[ORM\Column(type: 'string', length: 255)]
    private string $nom;
	**Que se passe-t-il si on enlève un attribut Doctrine (ex: `#[ORM\Column]`) ?** Doctrine ignore totalement la propriété, qui redevient une simple variable PHP. Lors de la prochaine migration, Doctrine considérera qu'elle ne sert plus et générera du code pour **supprimer la colonne correspondante** dans la base de données. C'est donc essentiel pour Doctrine.
**Au-delà de Doctrine, les attributs sont utilisés partout dans Symfony. Voici 3 autres exemples essentiels :**

- **Le Routing (URL) :** `#[Route('/accueil', name: 'app_accueil')]` au-dessus d'une méthode de Controller pour lier une URL à une page.
- **La Validation (Formulaires) :** `#[Assert\NotBlank]` au-dessus d'une propriété pour interdire qu'un utilisateur laisse un champ vide.
- **La Sécurité (Droits d'accès) :** `#[IsGranted('ROLE_ADMIN')]` au-dessus d'une fonction pour en bloquer l'accès aux non-administrateurs.

**14 - Quels bundles tiers avez-vous utilisé (précisez leur utilité) ?**
Un bundle est un paquet de code structuré pour s'intégrer à l'architecture Symfony. Son rôle principal est de configurer et d'injecter des bibliothèques externes ou des fonctionnalités spécifiques directement dans le conteneur de services (DIC) de Symfony, généralement via des fichiers YAML. Le besoin fondamental d'un bundle est d'**automatiser l'intégration d'une librairie dans le DIC de Symfony**. Pour instancier les classes de la librairie avec les parametres par exemple.
Notes : **DIC** (Injection de dépendances dans Symfony). L'injection de dépendances est un mécanisme qui permet d'implémenter le principe de l'inversion de contrôle. L'idée est de créer dynamiquement (_injecter_) les dépendances d'une classe en utilisant une description (un fichier de configuration par exemple). Cette méthode va nous permettre de ne plus exprimer les dépendances entre les composants dans le code de manière statique, mais de les déterminer dynamiquement à l'exécution.
Passer de : (plus besoin d'instancier l'objet !).
![[Pasted image 20260328114927.png]]![[Pasted image 20260328114942.png]]
De plus on peut déclarer les arguments directement dans Services.yml pour centraliser les données déclarées (normalement on fait référence au fichier .env donc %env(resolve:adminEmail)%) :
![[Pasted image 20260328115647.png]]

**DoctrineBundle** : **Doctrine ORM** pour l'accès aux BDD relationnelles, mais existe aussi ODM (object document mapper) pr les bdd NoSQL.

**Elastica (FOSElasticaBundle) : Le moteur de recherche sous stéroïdes**
FOSElasticaBundle : Permet d'intégrer Elasticsearch à Symfony. Il est utilisé pour la recherche en texte intégral (**full-text search**) sur de gros volumes de données. Il pallie les limites du SQL en gérant nativement la pertinence, la tolérance aux erreurs (**fuzzy matching**) et les agrégations de données pour créer des filtres dynamiques (**facettes**).
 **LexikJWTAuthenticationBundle** : Utilisé pour sécuriser les API REST. Il permet de mettre en place une authentification _stateless_ (sans état) en générant et en validant des tokens JWT