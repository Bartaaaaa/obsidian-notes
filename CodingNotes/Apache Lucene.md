**Lucene** est une bibliothèque de recherche d'information publiée par Apache. Logiciel gratuit et libre, écrit en Java. Lucene utilise des indexes pour la recherche. Pour la construction de l'indexe, il faut d'abord procéder à l'extraction des termes, que l'utilisateur peut configurer (quels champs). C'est un système avec lequel on peut chercher et trouver des informations.

Pour indexer les documents, on procède à la **tokenisation** (découpage du texte en liste). Pour une machine, un document est d'abord un assemblage de données constituée d'une chaine de caractère. A partir de cette quantité de données, des segments sont crées avec la tokenisation et les termes peuvent êtres recherchés.
Lucene effectue également une **normalisation** : les termes sont transformés en une forme standardisée (tout en minuscule, suppression des mots vides (le, la etc.)). L'utilisateur peut ensuite écrire des Query pr chercher dans le contexte.
![[Pasted image 20260707182911.png]]

**Concept de l'indexation :** 
Dans une base de données SQL traditionnelle, les données sont stockées **par ligne** (comme un tableau Excel). Si tu cherches le mot "chocolat" dans une colonne de texte avec un `LIKE '%chocolat%'`, la machine est obligée de faire un **Full Table Scan** : elle doit lire _chaque ligne_, l'une après l'autre, et scanner _toute la chaîne de caractères_ pour voir si le mot s'y trouve. S'il y a 10 millions de lignes, c'est extrêmement lourd et lent.

La raison pour laquelle un moteur de recherche comme Lucene est infiniment plus rapide, c'est une structure de données unique : **l'Index Inversé (Inverted Index)**.
Lucene fait exactement l'inverse. Au lieu de dire _"Voici le document 1, et voilà tout son texte"_, il crée un dictionnaire de mots qui dit : **"Voici le mot X, et voilà la liste de tous les documents qui le contiennent"**. C'est le principe de l'index inversé : c'est pas le document qui dit ce qu'il contient, mais le contenu qui dit ou est-ce qu'il apparait. Ainsi, on  a pas a parcourir tout le contenu pour trouver un mot.

![[Pasted image 20260707184714.png]]

Si l'utilisateur cherche les docs qui contiennent "chat, souris" -> Lucene regarde la ligne directement sans avoir à lire le moindre document.

Un champ est soit "**text**" : champ qui peut prendre toutes les valeurs, comme exemple au dessus, on stock tout apres normalisation & tokenisation.
Soit un champ "**keyword**" (valeurs exactes uniques) : prend la chaine entière sans normalisation. Attention, la recherche sur les champs keywords doit etre exacte. Si le champ est IN_PROGRESS, et l'user tape PROGRESS, pas de résultat ! Un keyword doit tjrs stocker des valeurs uniques (enum, uuid, token).

**Puisque Lucene fait déjà tout ce travail magique, pourquoi a-t-on besoin d'Elasticsearch ?**
ElasticSearch est un orchestrateur distribué (cluster manager), une appli serveur qui encapsule Lucene.
**Lucene est une bibliothèque Java, pas un serveur.** Tu ne peux pas lui parler en HTTP/REST, tu ne peux pas le brancher directement sur un réseau, et il ne sait tourner que sur **une seule machine**. Si ton index Lucene devient plus gros que le disque dur de ton serveur, tu es bloqué.
ElasticSearch est le manager de Lucene : découpe l'index géants en shards, chaque shard est une instance d'Apache Lucene indep. Permet la communication en API REST, coordination des shards.