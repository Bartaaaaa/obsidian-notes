
**Tout d'abord, les bases.** Qu'est-ce qu'un agent IA, quels sont les concepts fondamentaux et dans quels cas peut-on l'utiliser ? Nous aborderons également des solutions sans code si vous souhaitez commencer à expérimenter sans écrire une seule ligne de code.

**Ensuite, niveau intermédiaire.** Nous aborderons la conception et l'évaluation de systèmes multi-agents pour résoudre des problèmes concrets. Je ferai une démonstration d'un système multi-agents que j'ai créé et qui me permet actuellement de gagner plusieurs heures de travail par semaine.

**Ensuite, la question est plus avancée.** Que faut-il concrètement pour construire des systèmes d'agents fiables en production ?


### Qu'est-ce qu'un agent ?

Voici la façon la plus simple de l'imaginer. Imaginez que vous deviez rédiger une dissertation. Si vous utilisez un sujet classique de master en droit, vous diriez en gros : « Dis ChatGPT, rédige-moi une dissertation sur comment débuter en salle de sport », et le logiciel écrirait le texte en une seule fois, du début à la fin.

Mais ce n'est pas comme ça que vous ou moi écririons une dissertation, n'est-ce pas ? On ne produit pas une première version parfaite du premier coup. On planifie, on fait un plan, on se documente, on écrit un premier jet brouillon, on le relit et on le corrige. C'est tout un processus.

==C’est le principe de l’IA agentive. Au lieu de demander à l’IA de tout faire en une seule étape linéaire, on la laisse travailler de manière itérative, comme le ferait un humain.==

![[Pasted image 20260516111051.png]]
Un workflow d'agent est un process dans lequel un LLM exécutes différentes étapes pour répondre à une tâche.
C’est ce qu’on appelle la  **ReAct Loop**. Le modèle raisonne sur la prochaine étape, agit (souvent en appelant un outil, dont nous parlerons plus tard), observe le résultat, puis soit fournit une réponse, soit retourne au point de départ pour un nouveau raisonnement.
![[Pasted image 20260516111340.png]]
Cela fonctionne car chaque étape apporte de la profondeur. On obtient un raisonnement plus solide, moins d'hallucinations et une meilleure organisation, soit tout ce qui se perd lorsqu'on essaie de tout faire d'un coup.
Bien sûr, cette spécialisation et cette précision accrues ont un coût en termes de complexité. Dès lors, une question évidente se pose : *pour quels types de tâches justifient réellement la création d’agents ?*

### Pour quels types de tâches les agents sont-ils performants ?

Un exemple très simple de système automatisé pourrait consister à extraire les champs clés des factures, puis à les enregistrer dans une base de données. Les tâches aux processus clairs et répétables comme celle-ci sont parfaitement adaptées aux agents.
L'use Case Matrix permet de rapidement savoir si un agent vaut le coup pour l'utilisation d'agents.

![[Pasted image 20260516112002.png]]
La plus grande valeur ajoutée provient souvent des tâches complexes, et les premiers succès les plus rapides se situent généralement du côté des tâches moins exigeantes en termes de précision. C'est pourquoi le quadrant « haute complexité, faible précision » constitue souvent un point de départ judicieux.

### Spectre d'autonomie

Maintenant que vous savez à quoi servent les agents, voyons comment les constituer. La première grande décision à prendre concerne le degré d'autonomie que vous souhaitez leur accorder. Faut le voir comme un spectre.
![[Pasted image 20260516113354.png]]D'un côté, on trouve les agents scriptés où chaque étape est programmée en dur. Par exemple, pour la rédaction d'un essai, cela pourrait consister à générer les termes de recherche, effectuer un moteur de recherche, récupérer les pages, puis rédiger l'essai. C'est tout. C'est déterministe, prévisible et facile à contrôler. Le modèle n'a qu'à générer le texte, car vous avez tout défini.

D'un autre côté, on trouve **des agents hautement autonomes** . Le LLM décide alors s'il doit effectuer une recherche sur Google, des sites d'actualités ou des articles scientifiques. Il détermine le nombre de pages à extraire, s'il faut convertir les PDF et s'il convient de les analyser et de les corriger. Il peut même écrire et exécuter de nouvelles fonctions. Cette approche est plus puissante, mais aussi plus imprévisible et plus difficile à contrôler.

En pratique, la plupart des agents du monde réel se situent entre les deux et sont semi-autonomes. L'agent choisit parmi les outils que vous avez définis et prend des décisions dans le cadre des limites que vous avez fixées.

### Ingénierie du contexte

Mais comment un agent peut-il savoir quels outils sont disponibles ou comment prendre des décisions ?

On appelle cela « l'ingénierie du contexte », qui consiste à déterminer les informations dont dispose l'agent. Cela inclut des éléments tels que le contexte de la tâche, le rôle de l'agent, la mémoire de ses actions passées et les outils disponibles.

Si l'on considère l'ensemble de ces éléments, ce contexte oriente un modèle non déterministe vers des résultats cohérents et de haute qualité.

### Décomposition des tâches

Une fois que l'agent dispose de son contexte, il est temps de définir les tâches qu'il doit accomplir. Déterminer ces tâches est sans doute l'élément le plus important que vous apprendrez en matière de création d'agents.

Un agent doit posséder aussi une mémoire, pour se souvenir ce qui a marché afin de procéder différemment la prochaine fois. Ainsi, au cours des exécutions, ce dernier va apprendre de ses erreurs. Une bonne méthode est d'avoir un LLM pour l'évaluation à la fin.

Enfin, pour éviter que l'agent hallucine, on rajoute un Guardrail. Cela peut etre un LLM ou du code, voire même un humain, qui va vérifier l'output final pour vérifier sa conformité.

Il existe différents designs patterns pour améliorer les agents. 
- **Relection** : Le modèle produit un contenu, le relis, et le corrige.
- **Tool** **use** : Une liste d'éléments que le LLM peut appeler : web search, database queries, code execution. C'est important car un LLM est uniquement un générateur de texte. Et plus il a de contexte plus sa réponse sera bonne. A noter : Un LLM peut pas exécuter du code, mais peut en faire la requête.
- **Planning** : Au lieu d'une liste claire de tâches à effectuer, le LLM peut créer sa propre liste pour répondre au mieux à un besoin. C'est pas mal de lui dire de faire le plan en JSON
* **Multi-agent** : Chaque agent a un role précis. Pour éviter qu'un agent soit généraliste à tout faire avec un prompt énorme. Cela permet d'utiliser différents LLM, un plus rapide pour une tache rapide, un autre plus lent pour une autre tache. Paralléliser leur tache. Mais attention cela peut créer du conflit si deux agents modifient le meme fichier etc.
On va détailler davantage le pattern Multi-agent.
Chaque agent doit avoir son rôle, avec une description claire de ses outils et de son job à faire. Exemple : 
![[Pasted image 20260516202332.png]]
Y'a le pattern séquentiel : chaque agent exécutes l'un après l'autre. 
Le parallèle : parallélisation des appels.
Single Manager Hierarchy : Un agent qui en controle d'autres. Chaque agent fait son boulot et le rapporte au agent manager. Le manager peut changer les étapes, supprimer les éléments inutiles. La deep hierarchy est la meme mais avec des managers qui gèrent des managers.
Pour un projet web on peut décomposer les agents ainsi : **Functional Decomposition**![[Pasted image 20260516203205.png]]
Ou **Spacial Decomposition** : Un agent par dossiers.
- Agent 1 handles /services/users/*
- Agent 2 handles /services/orders/*
- Agent 3 handles /services/payments/*
- Agent 4 handles /services/notifications/*
**Temporal Decomposition** : Quand des étapes dépendent des précédentes.

Enfin quand tout est prêt il est temps d'essayer d'améliorer les process. Choisir le meilleur LLM, les meilleures API, fine tuner le modèle etc. Enfin, il faut calculer le coût d'exécution.
![[Pasted image 20260516203631.png]]

Une fois que c'est fini il faut mettre en place la **sécurité** : 
Attention au prompt injection, unsafe code generation, data leakage, resource exhaustion (un llm qui tourne en boucle, opération couteuse)
**Les bonnes pratiques sont :** 
Exécution dans un sandbox, limiter les resources (timeouts, memory caps, CPU limites), whitelist librairies (une liste avec les librairies autorisées), 