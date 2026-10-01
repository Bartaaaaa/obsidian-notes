
**Tokens** :
IA models ne lisent pas de mots ni de lettres, mais des tokens.
Un token peut représenter un mot entier s'il est **court et fréquent** ('the', 'cat', 'maison'), ou une partie de mot si le mot est **long ou rare** ('anticonstitutionnellement' → 'anti', 'constitution', 'nelle', 'ment'). Exemple de tokens : 'cat', 'tion', 'noto'. En moyenne en anglais, 1 token ≈ 4 caractères ≈ 3/4 d'un mot (le français en consomme un peu plus).
Plus on envoie de tokens au modèle, plus il a de traitement à faire. Plus de token ce dernier génère, plus il est couteux à exécuter.
Un modèle a une limite de tokens qu'il peut lire d'un coup : la **context window** (aujourd'hui entre ~100k et ~1M tokens selon les modèles), c'est pour cela qu'il peut oublier ce qui a été dit auparavant quand la conversation devient longue.
⚠️ Ça n'a rien à voir avec la RAM : le modèle **ne retient rien** entre deux messages, il est sans mémoire (stateless). À chaque message, c'est l'application (ChatGPT, Claude…) qui lui **renvoie tout l'historique** de la conversation. Quand l'historique dépasse la context window, l'appli doit **couper les vieux messages** ou les **résumer** pr que ça rentre → d'où l'oubli.

La manière dont est découpé un mot se fait lors de la phase d'apprentissage. Plusieurs milliards de mots pour l'entrainement. L'algorithme BPE (Byte pair encoding) va trouver les combinaisons de **caractères** qui apparaissent le plus souvent côte à côte. 
Au début les tokens sont les caractères de base 'a', 'b' etc. (en vrai les modèles modernes partent des **octets**, ce qui permet de gérer n'importe quel texte, emojis compris).
L'algo va trouver ensuite les paires les plus fréquentes. Par exemple e + r donne "er" qui apparait très souvent. Ensuite il voit que "er" et "g" forme souvent "erg" et en crée un token.
L'algorithme s'arrête quand le dictionnaire atteint une taille définie à l'avance (par exemple 32 000 ou 100 000 tokens uniques).

Si on prend en exemple "Asperge":
L'algorithme regarde le début du mot : `A`. Est-ce que `As` est dans le dictionnaire ? Oui. Est-ce que `Asp` y est ? Si oui, il prend `Asp`. Est-ce que `Aspe` y est ? Non.   Il valide donc le premier token : **`Asp`**.
 Il regarde ce qu'il reste : `erge`. Est-ce que `er` est dans le dictionnaire ? Oui. `erg` ? Oui. `erge` ?  Il valide le deuxième token : **`erge`**.
Le mot est donc découpé en `[Asp, erge]`.
(Simplification : en vrai BPE ne cherche pas "le plus long morceau", il rejoue dans l'ordre les fusions apprises pendant l'entrainement. Mais l'idée est la même : les morceaux fréquents deviennent un seul token.)


**Context window** : 
Imagine tu parles à qqn, mais sa mémoire peut uniquement retenir les dernières X minutes de la conversation. C'est le context window. C'est le nombre de Token qu'un modèle puisse voir et considérer en même temps. Plus le context window est grand (genre des millions de Tokens) plus le modèle est capable de garder en mémoire ce qu'on lui dit.

**Température** :
La température est un réglage qui va de 0 à 1 ou de 0 à 2 selon le fournisseur (0-1 chez Anthropic, 0-2 chez OpenAI). 
Température de 0 signifie que le modèle prend quasi toujours le token le plus probable : plus prévisible et moins aléatoire. On peut aussi dire moins créatif. Son output sera très similaire d'un appel à l'autre (mais pas garanti identique à 100%, des petites variations restent possibles).
Température de 1 est logiquement l'inverse. Le modèle va prendre des mots plus improbables, créatifs, des choix auxquels on s'attend pas. L'appel sera plus souvent différent d'un appel à l'autre.

**Hallucination** 
Une Hallucination est lorsqu'un LLM donne une mauvaise réponse,un fausse réponse en l'affirmant comme vraie, sans aucun doute avec pleine confidence.
Exemple : Tu demandes à une IA une info sur un auteur, elle te donne la date de naissance, le lieux, ses livres etc. Mais cet auteur n'existe pas.
*Pourquoi cela arrive ?*
Les gens oublient que les IA sont pas des bases de données. Elles prédisent juste le mot le plus probable basé sur l'entrainement reçu. Donc quand une IA ne sait pas qqchose, elle dit pas "Je sais pas", elle génère ce qui semble etre une bonne réponse car elle a été entrainée pour cela.
*Le problème n'est pas que l'IA se trompe, mais que l'IA se trompe avec autant de confidence que lorsqu'elle a raison.*


**RAG** = Retrieval-Augmented Generation (génération augmentée par la récupération d'infos)
Un modèle a été entrainé jusqu'à une certaine date sur certaines données. Le problème c'est qu'elle ne sait rien des données de l'entreprise, des dernières news (sauf aujourd'hui où elles ont accès aux recherches internet).
C'est ce qui correspond à la majorité des chatbots par exemple. Afin de pouvoir poser des questions sur une entreprise, il faut un RAG.
Le système fonctionne en 3 étapes : 
**L'ingestion (La bibliothèque) :** On prend tous les documents de l'entreprise (PDF, Word, Notion, etc.), on les découpe en petits morceaux (_chunks_) et on les stocke dans une base de données spéciale appelée **base de données vectorielle**. 
**La récupération (Le documentaliste) :** Quand l'utilisateur pose une question, le système va chercher dans cette base les 3 ou 4 morceaux de documents qui ressemblent le plus à la question.
**La génération (La rédaction) :** Le chatbot reçoit la question de l'utilisateur **+** les morceaux de documents trouvés. Il utilise ensuite ses capacités de rédaction pour formuler une réponse ultra-précise basée _uniquement_ sur ces documents.
Faut le voir comme une passerelle entre une IA et une source de données externe à celle de l'entrainement. Permet à l'IA de chercher une info dans la BDD avant de donner sa réponse.