
Moteur de recherche et d'analyse autonome, écrit en Java bâti sur la bilio apache Lucène. Il tourne comme un serveur distant, qu'on communique avec une API REST en JSON par HTTP.

Dans un projet Symfony, il y'a deux couches : 
- **ruflin/elastica** : biblio PHP bas niveau. Qui donne objets **Terms, BoolQuery, Range, Filter** qui construisent le JSON que comprend ES.
- **FOSElasticaBundle** : L'intégration de Symfony dessus. Il lit le fos_elastica.yaml qui gère la création d'index, et expose la commande : fos:elastica:populate

Bonne analogie : ES est à la recherche ce que Postgres est aux données relationnelles, et FOSElastica est le "Doctrine" de ES. Postgres tourne tt seul, Symfony lui parle via Doctrine. C'est la même chose, ES tourne tout seul, Symfony lui parle via Elastica/FOSElastica.


**Comment ça marche ?** 
ES stocke des documents JSON, qui vivent dans un index. Chaque index a un mapping : la définition des champs et leur type (~ le schéma de la base), c'est ce qui est dans le fos_elastica.yaml.

ES étant un serveur à part entière qui ne parle qu'en JSON par HTTP, il n'a pas accès a nos entités dans le code, c'est pour cela qu'on doit définir un fos_elastica.yaml. Il peut deviner des fois le type de champs qu'on lui envoie, mais cela est source d'erreurs. Avec le fichier définit clairement, ces erreurs disparaissent.

**Cela fonctionne en deux étapes :** 
**Le groupe elasticsearch = Quels champs** : C'est un filtre côté PHP, avant ES. Une propriété taguée {"elasticsearch"} est sérialisée et envoyée. Une propriétée non tagué ne part jamais : on évite de tout stocker. C'est le groupe qui fait ce job : on décide d'envoyer à ES uniquement ce qu'on désire afficher/récupérer.
 @Serializer\Groups({"Default", "elasticsearch"}) //On met Default car c'est le groupe par défaut utilisé quand on précise pas le \Groups. Sinon le champ disparaitrait de toute sérialisation faite avec le groupe Default.
 
**Le Mapping = Commen**t. Pour les champs qui sont arrivés, on dit le type : keyword, date, etc. C'est pas de la sélection, mais le typage des champs qui arrivent.
```
Objet PHP (TOUS les champs de l'entité)
   │
   │  ① JMS Serializer + groupe "elasticsearch"
   │     → décide QUELS champs partent vers ES
   ▼
JSON (un sous-ensemble de champs)
   │
   │  ② envoyé à ES par HTTP
   ▼
ES indexe selon le MAPPING (properties)
   → décide COMMENT chaque champ reçu est typé (text / keyword / date…)
   → et range le JSON reçu dans un champ spécial : _source
   ▼
Recherche → ES te renvoie _source (= ce qui a été envoyé)
```

**config/packages/fos_elastica.yaml** : déclare le client, les index, le mapping (champ -> type)
serializer: jms_serializer : Transformation des objets PHP en JSON envoyés à l'ES grâce à la sérialization.
clients.default.connections: différents noeuds d'un même cluster pour assurer de la disponibilité si l'un tombe (résilience & débit en lecture).
indexes : les tables qu'on va indexer sur ES
use_alias : true : permet de rendre l'indexation atomique, sans pertes

```
fos_elastica
├── serializer        → objets PHP ➜ JSON
├── clients           → connexions au cluster ES (les nœuds)
└── indexes
    └── <nom logique>
        ├── index_name   → nom réel dans ES (par env)
        ├── use_alias    → bascule sans coupure au populate
        ├── settings     → shards (définit par combien on découpe la bdd horizontalement : on tranche par paquets de lignes complètes, chaque shards a tous les champs, mais une partie des dossiers. Contrairement au découpage vertical : découpage par champs)) + replicas (nombre de copies de chaque shard posées sur d'autres noeuds : résilience + débit en lecture (requetes peuvent taper dans les repliques en parallèle))
        └── properties   → le mapping (champ → type)
```
    
        