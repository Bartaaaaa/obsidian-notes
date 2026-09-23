**ELK** est une chaine de traitement de données. : Logstash pour recevoir les données, les transformer, enrichir, filtrer : lourd car tourne sur une JVM, ElasticSearch : le moteur de recherche, une base NoSQL orientée Json pr la recherche ultra rapide. Kibana, l'interface de monitoring pr créer des dashboards et intérroger ElasticSearch sans coder.
# ElasticSearch

ElasticSearch est un moteur de recherche et d'analyse distribué et open source conçu pour gérer des grands volumes de données stockées en NOSQL (souvent JSON) et offrir des capacités de recherches quasiment en temps réel.  La recherche y est très performante car il sait où il a mis les objets. On peut y récupérer les données, mais auss**i les informations sur les données** : comme le nombre de fois qu'un enregistrement correspond à certaines conditions. Il stocke les données sous format JSON. Peut aussi etre utilisé comme outil de monitoring.
ElasticSearch est construit sur [[Apache Lucene]] et il fait partie de la suite Elastic Stack qui comprend [[Kibana]] pour la visualisation des données et [[Logstash]] pour le traitement des données.
Moteur de recherche et d'analyse autonome, écrit en Java bâti sur la bilio apache Lucène. Il tourne comme un serveur distant, qu'on communique avec une API REST en **JSON** par **HTTP**.

**Comment ça marche ? **
- **Indexation*** : Les données sont ingérées dans Elasticsearch via le processus d’indexation. Lors de [l’indexation](https://www.geeksforgeeks.org/dbms/indexing-in-databases-set-1/) , les documents sont analysés, tokenisés et stockés dans des index inversés, ce qui permet des opérations de recherche rapides et efficaces.  
- **Requêtes** : Les utilisateurs interagissent avec Elasticsearch par le biais de requêtes, qui peuvent être de simples recherches par mots-clés ou des agrégations complexes. Elasticsearch utilise un **DSL (langage spécifique au domaine)*** pour exprimer différents types de requêtes, allant des recherches plein texte de base aux agrégations et filtres avancés.
-  **Partionnement** & **Recherche distribuée** : Elasticsearch utilise le partitionnement pour répartir les données sur plusieurs nœuds d'un cluster, améliorant ainsi les performances et l'évolutivité. Chaque partition est un fragment d'index autonome, permettant à Elasticsearch de paralléliser les opérations de recherche et d'indexation.
- **Replication :** Pour garantir la redondance des données et la tolérance aux pannes. Chaque partition peut comporter une ou plusieurs répliques, qui servent de sauvegardes en cas de défaillance d'un nœud ou de perte de données.

ES stocke des documents JSON, qui vivent dans un index. Chaque index a un mapping : la définition des champs et leur type (~ le schéma de la base), c'est ce qui est dans le fos_elastica.yaml.
Dans un projet Symfony, il y'a deux couches : 
- **ruflin/elastica** : biblio PHP bas niveau. Qui donne objets **Terms, BoolQuery, Range, Filter** qui construisent le JSON que comprend ES.
- **FOSElasticaBundle** : L'intégration de Symfony dessus. Il lit le fos_elastica.yaml qui gère la création d'index, et expose la commande : fos:elastica:populate

**Bonne analogie** : ES est à la recherche ce que Postgres est aux données relationnelles, et FOSElastica est le "Doctrine" de ES. Postgres tourne tt seul, Symfony lui parle via Doctrine. C'est la même chose, ES tourne tout seul, Symfony lui parle via Elastica/FOSElastica.

ES étant un serveur à part entière qui ne parle qu'en JSON par HTTP, il n'a pas accès a nos entités dans le code, c'est pour cela qu'on doit définir un **fos_elastica**.**yaml**. Il peut deviner des fois le type de champs qu'on lui envoie, mais cela est source d'erreurs. Avec le fichier définit clairement, ces erreurs disparaissent.

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
**Découpage horizontal :** 
Avantage : Scalabilité infinie, performances des requêtes ciblées, disponibilité (si un noeud tombe, le reste est dispo).
Inconvénients : Requete couteuses si on sait pas ou chercher la donnée, complexité de répartition (bien répartir les shards)
**Découpage vertical :** 
Avantage : Optimisation I/O : si on a un champ textuel immense, mais que la majorité des requetes cherchent date/titre -> on stock pas le gros texte en mémoire, meilleure mise en cache (les champs fréquemment utilisés restent en cache)
Inconvénients : Cout des jointures, scalabilité limitée (meme si un shard a 3 colonnes, il ne peut gérer 100K lignes)

A savoir : Pour modifier le nombre de shards en court de route, on peut pas augmenter comme ça, il faut créer un nouvel indexe et réindexer.

# **Logstash**
Logstash s’insère dans la stack Elastic (ELK/Elastic Stack) aux côtés d’Elasticsearch, Kibana et Beats. Il joue le rôle de **moteur ETL** (Extract, Transform, Load), en centralisant et normalisant les données avant leur indexation dans Elasticsearch
. Il s'accompagne d'un ensemble de modules permettant d'ingérer des multitudes de formats de logs courants, notamment les BDD, fichiers de
logs, files de messages, api REST,....

**Concrètement ?** 
Logstash est un moteur de collecte et de traitement des données via plug-in. Il est doté de nombreux plug-ins qui permettent de configurer facilement l'outil pour collecter, traiter et transférer les données dans un grand nombre d'architectures variées.

Lorsque vous configurez le fichier, il est utile de considérer Logstash comme un pipeline qui prend les données à une extrémité, les traite d’une manière ou d’une autre et les envoie à leur destination (dans ce cas, la destination est Elasticsearch). Un pipeline Logstash comporte deux éléments obligatoires, `input` (l’entrée) et `output` (la sortie), et un élément optionnel, `filter` (nettoyer, anonymiser des données...). Les plugins d’entrée consomment les données d’une source, les plugins de filtrage traitent les données, et les plugins de sortie écrivent les données vers une destination.
![[Pasted image 20251218181928.png]]
Faut lui configurer un fichier logstash.conf dans lequel on va lui définir un json avec les données sur lesquelles il doit taper (l'URI), la collection en question, l'host (9200) ...

Mini exemple de pipeline logstash : 
```conf
input {
  mongodb {
    uri => "mongodb://localhost:27017"
    database => "logs_db"
    collection => "logs"
  }
}

filter {
  mutate {
    add_field => { "source" => "mongodb" }
  }
}

output {
  elasticsearch {
    hosts => ["http://localhost:9200"]
    index => "mongodb-logs"
  }
}
```


# Kibana
Kibana est un outil de visualisation des données indexées à Elasticsearch. Il permet de rendre les flux de données énormes et complexes plus rapides et plus facilement compréhensibles grâces à une représentation graphique. Il permet de créer des dashboards, graphiques, filtres et listes pour visualiser les données, tableaux de bords intéractifs.
![[Pasted image 20251218181218.png]]

En gros visualiser sa base de données afin d'en faire des analyses et actions. 
On récupère la data d'une source, [[Logstash]] process cette données qui est stockée dans [[Suite ELK]], et Kibana permet de visualiser la donnée.

![[Pasted image 20251218181335.png]]