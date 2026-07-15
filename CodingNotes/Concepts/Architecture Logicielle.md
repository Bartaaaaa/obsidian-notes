
**L'architecture logicielle** désigne la structure fondamentale d'un système informatique : l'organisation des composants, leurs interactions et les principes qui guident leur conception. Elle se décide en fonction des contraintes techniques et métiers du projet.

L'architecture logicielle a un impact important sur la **maintenabilité** du projet, la **scalabilité** (un monolithe bien structuré peut évoluer vers des microservices si nécessaire), la **testabilité**, **performances**, **sécurité**, **coûts**...

**L'architecture monolithique** regroupe toutes les fonctionnalités dans une seule application déployable. Simple à déployer, peut devenir complexe à maintenir. C'était le choix historique sur les applications & sites mais aujourd'hui on utilise davantage une architecture en microservices.

**+** développement simple, déploiement simple (1 seul dossier), débogage facile, sécurité car un système fermé
**-** difficile à faire évoluer, à adapter
Ex: startups, projets de base, vieux projets
![[Pasted image 20260711155108.png]]

Le projet **DMM** est une architecture monolithique côté back Symfony qui expose une API. Le front React est en SPA. Le projet n'est pas en microservices car y'a pas plusieurs services back déployés séparément et avec chacun sa responsabilité et sa BDD. DMM le front est en monorepo  car il contient plusieurs applications (bo, fo) contrairement a Mires qui est une appli react simple.


```
mon-projet-ecommerce/
├── src/
│   ├── controllers/    # Gère TOUTES les requêtes (users, produits, factures)
│   ├── models/         # Les modèles de TOUTE la base de données
│   ├── services/       # La logique métier centralisée
│   └── utils/          # Des fonctions partagées par tout le monde
├── package.json        # TOUTES les dépendances du projet réunies ici
├── bdd_schema.sql      # Un seul schéma pour la seule et unique base de données
└── server.js           # Un seul point d'entrée qui lance toute l'application (ex: port 8080)
```
**Si un conteneur contient tout le code, c'est du monolithique**

**L'architecture en microservices** décompose l'application en services indépendants, chacun responsable d'une fonctionnalité indépendantes. Les services communiques via des [[Protocole d'API]]. Cette architecture offre une scalabilité et une indépendance maximale, au prix d'une complexité un peu plus grande.

**+** facile à faire évoluer, conçue pr l'automatisation CI/CD, opération indépendante entre les services
**-** tests plus complexes (quand un service a besoin d'un autre), sécurité car communication entre les services, latence, couts quand plus de services
Ex: commerces, plateformes de divertissements


```
(Souvent dans des dépôts Git séparés, ou un 'monorepo' bien compartimenté)
service-utilisateurs/
├── src/                # Code UNIQUEMENT lié aux utilisateurs
├── package.json        # Dépendances Node.js spécifiques
└── server.js           # Tourne dans son coin (ex: port 3001)

service-produits/
├── src/                # Code UNIQUEMENT lié au catalogue
├── requirements.txt    # Tiens, ce service est codé en Python ! (C'est possible)
└── main.py             # Tourne dans son coin (ex: port 3002)

api-gateway/            # Le chef d'orchestre à l'entrée
├── routes.js           # Route /users vers 3001, /products vers 3002
└── server.js           # Tourne sur le port 8080 pour les clients`
```
Les microservices peuvent très bien êtres codés en un langage différents. Pour l'instant je n'ai pas eu à travailler sur un projet utilisant des microservices.

**Clean Architecture** 

**Architecture hexagonale** 
Place le métier au centre (l'hexagone) et l'isole du monde extérieur via des ports (interfaces) et des adapters (implémentations concrètes : BDD, UI, API externes). + le cœur métier ne dépend ni de la BDD ni du framework, facile de changer un adapter (ex: MySQL vers MongoDB) sans toucher au métier, bonne testabilité car on peut mocker les ports. - complexité d'implémentation avec beaucoup d'interfaces et d'abstractions, peut sembler redondant sur un petit projet. 
**Ports** : Le port est une interface définie par le métier qui exprime un besoin. Il dit ce qu'on peut faire, sans définir comment.
```javascript
// Le port dit juste : "j'ai besoin de pouvoir sauvegarder et chercher un user"
class UserRepositoryPort {
  save(user) { throw new Error("non implémenté"); }
  findByEmail(email) { throw new Error("non implémenté"); }
}
```
Deux types de ports : 
Primaire : ce qui entre dans le métier (controller HTTP, commande)
Secondaire : Ce que le métier appelle vers l'extérieur (BDD, API Tierce, envoie d'email)
**Adaptateurs** : L'adaptateur dit comment le faire réellement, sur une techno précise.
```javascript
// L'adapter dit : "je le fais avec MongoDB, concrètement"
class MongoUserRepository extends UserRepositoryPort {
  async save(user) { await this.collection.insertOne(user); }
  async findByEmail(email) { return this.collection.findOne({ email }); }
}
```

Ex: systèmes avec beaucoup d'intégrations externes comme le paiement ou des API tierces.
```
src/
├── core/
│   ├── domain/
│   └── ports/            # Interfaces (ex: UserRepositoryPort)
├── adapters/
│   ├── primary/          # Entrée : controllers, CLI
│   └── secondary/        # Sortie : DB, API externes
```

![[Pasted image 20260714163343.png]]

Normalement en couches classiques, le sens de dépendance est :
```
Controller → Métier → BDD
```
En hexagonale, ce sens est inversé pour la BDD :
```
Controller → Métier ← BDD (via le port)
```


**Architecture événementielle** (event-driven) : Organise le système autour d'événements émis et consommés par différents composants. Adaptée pour les systèmes IOT et aux apps en temps réels.

**Single Page Application**
Le front charge une seule page HTML au départ, puis le routing et le rendu se font côté client en JS, sans rechargement de page à chaque navigation. + navigation fluide, expérience proche d'une app native, séparation claire front/back car le front consomme une API. - SEO plus difficile car le contenu est généré en JS, temps de chargement initial plus long car tout le JS est chargé au début. Ex: DMM front, Mires.

```
public/
├── index.html        # Un seul point d'entrée HTML
src/
├── components/
├── pages/             # Routing côté client (React Router...)
├── services/          # Appels API vers le back
└── App.jsx
```