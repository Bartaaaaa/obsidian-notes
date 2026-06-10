
**Tokens** :
IA models ne lisent pas de mots ni de lettres, mais des tokens.
Un token peut représenter une partie d'un mot, ou un mot entier s'il est long. Exemple : 'cat', 'tion', 'noto'.
Plus on envoie de tokens au modèle, plus il a de traitement à faire. Plus de token ce dernier génère, plus il est couteux à exécuter.
Un modèle a une mémoire (aujourd'hui de millions de tokens), c'est pour cela qu'il peut oublier ce qui a été dit auparavant quand la conversation devient longue. Les vieux tokens sont supprimés quand la RAM se remplie.

La manière dont est découpé un mot se fait lors de la phase d'apprentissage. Plusieurs milliards de mots pour l'entrainement. L'algorithme BPE (Byte pair encoding) va trouver les combinaison de mots qui apparaissent le plus souvent. 
Au début les tokens sont les lettres d'alphabet 'a', 'b' etc.
L'algo va trouver ensuite les paires les plus fréquentes. Par exemple e + r donne "er" qui apparait très souvent. Ensuite il voit que "er" et "g" forme souvent "erg" et en crée un token.
L'algorithme s'arrête quand le dictionnaire atteint une taille définie à l'avance (par exemple 32 000 ou 100 000 tokens uniques).

Si on prend en exemple "Asperge":
L'algorithme regarde le début du mot : `A`. Est-ce que `As` est dans le dictionnaire ? Oui. Est-ce que `Asp` y est ? Si oui, il prend `Asp`. Est-ce que `Aspe` y est ? Non.   Il valide donc le premier token : **`Asp`**.
 Il regarde ce qu'il reste : `erge`. Est-ce que `er` est dans le dictionnaire ? Oui. `erg` ? Oui. `erge` ?  Il valide le deuxième token : **`erge`**.
Le mot est donc découpé en `[Asp, erge]`.


**Context window** : 
Imagine tu parles à qqn, mais sa mémoire peut uniquement retenir les dernières X minutes de la conversation. C'est le context window. C'est le nombre de Token qu'un modèle puisse voir et considérer en même temps. Plus le context window est grand (genre des millions de Tokens) plus le modèle est capable de garder en mémoire ce qu'on lui dit.

**Température** :
La température est une variable qui va de 0 à 1. 
Température de 0 signifie que le modèle sera plus prévisible et moins aléatoire. On peut aussi dire moins créatif. Son output sera similaire d'un appel à l'autre.
Température de 1 est logiquement l'inverse. Le modèle va prendre des mots plus improbables, créatifs, des choix auxquels on s'attend pas. L'appel sera plus souvent différent d'un appel à l'autre.

**Hallucination** 
Une Hallucination est lorsqu'un LLM donne une mauvaise réponse,un fausse réponse en l'affirmant comme vraie, sans aucun doute avec pleine confidence.
Exemple : Tu demandes à une IA une info sur un auteur, elle te donne la date de naissance, le lieux, ses livres etc. Mais cet auteur n'existe pas.
*Pourquoi cela arrive ?*
Les gens oublient que les IA sont pas des bases de données. Elles prédisent juste le mot le plus probable basé sur l'entrainement reçu. Donc quand une IA ne sait pas qqchose, elle dit pas "Je sais pas", elle génère ce qui semble etre une bonne réponse car elle a été entrainée pour cela.
*Le problème n'est pas que l'IA se trompe, mais que l'IA se trompe avec autant de confidence que lorsqu'elle a raison.*


**RAG** = Retrieved augmented Generation
Un modèle a été entrainé jusqu'à une certaine date sur certaines données. Le problème c'est qu'elle ne sait rien des données de l'entreprise, des dernières news (sauf aujourd'hui où elles ont accès aux recherches internet).
C'est ce qui correspond à la majorité des chatbots par exemple. Afin de pouvoir poser des questions sur une entreprise, il faut un RAG.
Le système fonctionne en 3 étapes : 
**L'ingestion (La bibliothèque) :** On prend tous les documents de l'entreprise (PDF, Word, Notion, etc.), on les découpe en petits morceaux (_chunks_) et on les stocke dans une base de données spéciale appelée **base de données vectorielle**. 
**La récupération (Le documentaliste) :** Quand l'utilisateur pose une question, le système va chercher dans cette base les 3 ou 4 morceaux de documents qui ressemblent le plus à la question.
**La génération (La rédaction) :** Le chatbot reçoit la question de l'utilisateur **+** les morceaux de documents trouvés. Il utilise ensuite ses capacités de rédaction pour formuler une réponse ultra-précise basée _uniquement_ sur ces documents.
Faut le voir comme une passerelle entre une IA et une source de données externe à celle de l'entrainement. Permet à l'IA de chercher une info dans la BDD avant de donner sa réponse.