

LLM, Large language models : Modèle de deep learning capable de générer du texte et d'effectuer d'autres tâches. Ils sont construits sur une architecture de réseau de neurones appelé *transformer*.

Qu'est-ce qu'un réseau neuronal ?
Un réseau neuronal est un modèle d'apprentissage automatique qui empile des « neurones » simples en couches et apprend à partir des données des poids et des biais de reconnaissance de formes pour faire correspondre les entrées aux sorties.
Le réseau neuronal est la base de la vision par ordinateur, reconnaissance vocale, reconnaissance faciale etc.

Un LLM fonctionne comme une machine qui va prédire le mot suivant. 
Contrairement aux anciens algorithmes qui permettaient aux utilisateurs de faire des recherches avec des mots clés. Les LLM saisissent le  contexte, les nuances etc. Ils sont entrainés dans un but (redaction, debug, etc) et peuvent ainsi effectuer des tâches uniquement faisable auparavant par des humains.

Entrainement : 
L'entraînement commence par une quantité massive de données : des milliards, voire des billions de mots extraits de livres, d'articles, de sites web, de code et d'autres sources textuelles. Les data scientists supervisent le nettoyage et le prétraitement afin d'éliminer les erreurs, les doublons et les contenus indésirables. Ce texte est décomposé en unité plus petites telles que des mots, semi-mots : appelé tokens. Cela permet de normaliser la langue afin de traiter de manière cohérente les mots rares & nouveaux.

Les LLM sont initialement entraînés par l'apprentissage auto-supervisé. Dans l'apprentissage auto-supervisé, les tâches sont conçues de manière à ce que la vérité de référence puisse être déduite des données non étiquetées. Au lieu de recevoir une consigne sur la « sortie correcte » pour chaque entrée, comme dans l'apprentissage supervisé, le modèle tente de trouver par lui-même des schémas, des structures ou des relations dans les données.


![[Pasted image 20260515230709.png]]Le domaine de l'IA est souvent visualisé en couches :

- **L'intelligence artificielle** (IA) est un terme très large, mais il désigne généralement les machines intelligentes.
- **L’apprentissage automatique** (ML) est un sous-domaine de l’IA qui se concentre spécifiquement sur la reconnaissance de formes dans les données.==Comme vous pouvez l'imaginer, une fois que vous avez identifié un schéma, vous pouvez l'appliquer à de nouvelles observations.==Voilà en résumé l'idée, mais nous y reviendrons dans un instant.
- ==**Apprentissage profond**====est le domaine de l'apprentissage automatique qui se concentre sur les données non structurées==, qui comprend du texte et des images. Il repose sur des réseaux neuronaux artificiels, une méthode qui s'inspire (de manière assez libre) du cerveau humain.
- **Les grands modèles de langage** (LLM) traitent spécifiquement du texte.

Comment marche la prédiction ? Rappelle toi des cours de SOY9 ! 
![[Pasted image 20260515230907.png]]
On a une BDD pour l'entrainement qui présente des caractéristiques. Ainsi, dès qu'on ajoute un nouvel élément, on sera capable de prédire à quel domaine ce dernier appartient. Ici c'est de la classification, car les valeurs sont pas continues (soit R&B soit Reggeaton).

La réalité est généralement plus complexe à plus d'un autre égard. Au lieu de seulement deux entrées comme dans notre exemple, nous avons souvent des dizaines, des centaines, voire des milliers de variables d'entrée. De plus, nous avons souvent plus de deux classes. Et toutes les classes peuvent dépendre de toutes ces entrées via une relation non linéaire incroyablement complexe. 
Plus la relation entre les entrées et les sorties est complexe, plus le modèle d'apprentissage automatique nécessaire pour appréhender cette relation doit être complexe et puissant et plus les données d'entrainement doivent êtres grandes. Généralement, la complexité augmente avec le nombre d'entrées et le nombre de classes.

**Et si l'entrée est une image ?** 
![[Pasted image 20260515231540.png]]
Dans cet exemple on souhaite savoir si une image est un tigre, un chat ou un renard. C'est à nouveau un problème de classification. Etant donné qu'un ordinateur ne peut traiter que des valeurs numériques. Les images sont composés de pixels : Hauteur, largeur & 3 canneaux (RVB)
L'ordinateur doit apprendre la correspondance entre les pixels et l'étiquette.

**L'approche classique (SY32) : HOG (Histogram of oriented objects) & Sliding Window**
Dans les années 2000 (par exemple pour détecter des visages sur les premiers appareils photo numériques), on utilisait cettea méthode. L'ordinateur balayait l'image de gauche à droite, de haut en bas avec un carré (la fenêtre glissante) . Pour chaque carré, il calculait des vecteurs mathématiques (comme les contours ou les gradients de couleur) et un algorithme classique disait "Oui c'est un chat" ou "Non ce n'est pas un chat"
On verra une méthode bien plus efficace dans l'apprentissage profond.

![[Pasted image 20260515232349.png]]
Essayons de déterminer le sentiment que rejette une phrase; positive ou négative. Ici la complexité est plus grande que pour les images, car les images sont numériques grâces aux pixels, contrairement au texte. Une solution est de transformer en un vecteur de représentation vectorielle. Ce vecteur lexical représente le sens sémantique et syntaxique d'un mot comprenant des milliers de variables par mot.
Ainsi on peut transformer une phrase en une séquence d'entrée numériques.
Mais comme pour les images on se confronte au même problème : Ces données sont bien trop importantes pour faire de l'apprentissage sur un grand jeu de données.

Ce qu'il nous faut, c'est un modèle d'apprentissage automatique extrêmement puissant et une grande quantité de données. C'est là qu'intervient l'apprentissage profond.

![[Pasted image 20260516082204.png]]
Les réseaux neuronaux sont des modèles d'apprentissage automatique puissants qui permettent de modéliser des relations d'une complexité arbitraire.
En réalité, les réseaux de neurones s'inspirent vaguement du cerveau, bien que les similitudes exactes soient sujettes à débat. Leur architecture de base est relativement simple. Ils se composent d'une séquence de couches de « neurones » interconnectés que traverse un signal d'entrée afin de prédire la variable de résultat. On peut les concevoir comme plusieurs couches de régression linéaire superposées, auxquelles s'ajoutent des non-linéarités, ce qui permet au réseau de neurones de modéliser des relations hautement non linéaires.

Les réseaux neuronaux sont souvent composés de nombreuses couches (d'où le nom d'apprentissage profond), ce qui signifie qu'ils peuvent être extrêmement volumineux. ChatGPT, par exemple, repose sur un réseau neuronal constitué de 176 milliards de neurones, soit plus que les quelque 100 milliards de neurones présents dans le cerveau humain. 


![[Pasted image 20260516083143.png]]
Comme dans cet exemple, l'entrée du réseau neuronal est une séquence de mots, mais ici, la sortie est simplement le mot suivant. Il s'agit encore une fois d'une simple tâche de classification. La seule différence est qu'au lieu de deux ou quelques classes seulement, nous avons maintenant autant de classes qu'il y a de mots — disons environ 50 000. C'est le principe de la modélisation du langage : apprendre à prédire le mot suivant.

![[Pasted image 20260516084351.png]]Nous connaissons la tâche, et il nous faut maintenant des données pour entraîner le réseau neuronal. Créer une grande quantité de données pour notre tâche de « prédiction du mot suivant » est en réalité assez simple. On trouve une profusion de textes sur Internet, dans les livres, les articles de recherche, etc. Nous pouvons donc facilement constituer un vaste ensemble de données à partir de toutes ces sources. Il n'est même pas nécessaire d'étiqueter les données, car le mot suivant lui-même sert d'étiquette ; c'est pourquoi on parle également _d'apprentissage auto-supervisé_ . L'image ci-dessus illustre ce processus. Une seule séquence peut être transformée en plusieurs séquences d'entraînement.
Si le réseau neuronal est suffisamment grand et que l'on dispose de suffisamment de données, le modèle linéaire mixte (LLM) devient très performant pour prédire le mot suivant. Sera-t-il parfait ? Non, bien sûr que non, car plusieurs mots peuvent souvent se succéder. Mais il saura sélectionner le mot le plus approprié, tant sur le plan syntaxique que sémantique.

![[Pasted image 20260516084641.png]]Maintenant que nous pouvons prédire un mot, nous pouvons réinjecter la séquence étendue dans le LLM et prédire un autre mot, et ainsi de suite. Autrement dit, grâce à notre LLM entraîné, nous pouvons désormais générer du texte, et non plus un seul mot. C'est pourquoi les LLM sont un exemple de ce que l'on appelle l'IA générative. Nous venons d'apprendre au LLM à « parler », pour ainsi dire, un mot à la fois.

Il y a un détail important à comprendre : il n’est pas toujours nécessaire de prédire le mot le plus probable. On peut par exemple sélectionner les cinq mots les plus probables à un instant donné. Ainsi, le modèle de langage naturel (MLN) peut se montrer plus créatif. Certains MNL permettent même de choisir le degré de déterminisme ou de créativité du résultat. C’est pourquoi, dans ChatGPT, qui utilise cette stratégie d’échantillonnage, la réponse obtenue lors de la régénération est généralement différente.

![[Pasted image 20260516085220.png]]
We have actually just learned what the G stands for, namely “generative” — meaning that it was trained on a language generation pretext, which we have discussed. But what about the P and the T?
“transformer” — that’s simply the type of neural network architecture that is being used. Its main strength, it’s that the transformer architecture works so well because it can focus its attention on the parts of the input sequence that are most relevant at any time. You could argue that this is similar to how humans work. We, too, need to focus our attention on what’s most relevant to the task and ignore the rest.

Now to the P, which stands for “pre-training”. We discuss next why we suddenly start speaking about pre-training and not just training any longer.
The reason is that Large Language Models like ChatGPT are actually trained in phases.

![[Pasted image 20260516100612.png]]La première étape est le pré-entraînement, que nous venons d'effectuer. Cette étape nécessite une quantité massive de données pour apprendre à prédire le mot suivant. Durant cette phase, le modèle apprend non seulement à maîtriser la grammaire et la syntaxe de la langue, mais il acquiert également une grande quantité de connaissances sur le monde, et même d'autres capacités émergentes dont nous parlerons plus tard.
En réalité, ce modèle a surtout appris à divaguer sur un sujet. Il le fait peut-être même très bien, mais il ne réagit pas correctement aux interactions que l'on attend généralement d'une IA, comme une question ou une instruction. Le problème est que ce modèle n'a pas appris à être, et ne se comporte donc pas comme, un assistant.
Par exemple, si vous demandez à un LLM pré-entraîné *« Quel est votre prénom ? »*, il peut répondre *« Quel est votre nom de famille ? »* car c'est le type de données qu'il a rencontrées lors du pré-entraînement, comme dans de nombreux formulaires vides, par exemple. Il cherche simplement à compléter la séquence d'entrée. Il a du mal à suivre les instructions, car ce type de structure linguistique (instruction suivie d'une réponse) est rare dans les données d'entraînement. Quora ou Stack Overflow en seraient peut-être les exemples les plus proches.

### Réglage fin des instructions et RLHF
C’est là qu’intervient le réglage des instructions. Nous prenons le LLM pré-entraîné avec ses capacités actuelles et faisons essentiellement ce que nous faisions auparavant — c’est-à-dire apprendre à prédire un mot à la fois — mais cette fois-ci, nous utilisons uniquement des paires instruction-réponse de haute qualité comme données d’entraînement.
Ainsi, le modèle cesse d'être un simple compléteur de texte et apprend à devenir un assistant utile qui suit les instructions et répond conformément à l'intention de l'utilisateur. La taille de cet ensemble de données d'instructions est généralement bien inférieure à celle de l'ensemble de pré-entraînement. En effet, la création de paires instruction-réponse de haute qualité est beaucoup plus coûteuse, car elles proviennent généralement d'humains.
Il existe également une troisième étape que certains LLM comme ChatGPT traversent, qui est *Apprentissage par renforcement à partir de retours humains (RLHF)*. RLHF contribue également à l'alignement et garantit que la sortie du modèle de langage reflète les valeurs et les préférences humaines.


![[Pasted image 20260516101433.png]]
Un LLM est très fort pour résumer du texte puisque Internet est rempli de résumés. Il a donc été entraînés sur des longs textes et des résumés.
Un LLM peut répondre à des questions de culture générales car il a été entraînés sur tellement de texte qu'il sait tout.
Enfin, on parle d'hallucination dès qu'un LLM invente une réponse.

![[Pasted image 20260516101835.png]]
Le modèle linéaire mixte (LLM) apprend uniquement à générer du texte, et non du texte factuellement exact. Rien dans son entraînement ne lui fournit d'indicateur de la véracité ou de la fiabilité des données d'entraînement. Cependant, là n'est même pas le problème principal : **généralement, les textes que l'on trouve sur Internet et dans les livres semblent assurés, et le LLM apprend donc naturellement à adopter ce même ton**, même s'il est erroné. De cette façon, un LLM est peu sensible à l'incertitude.
![[Pasted image 20260516102404.png]]
Si on demande à un LLM quel est l'actuel président, il y'a deux risques : 
-LLM peut avoir des hallucinations et inventer un président
-LLM a été entrainé sur des vieilles données, et il a pas reçu l'information sur le dernier président

Comment résoudre ces deux problèmes ? La solution consiste à fournir au modèle un contexte pertinent. En effet, tout ce qui figure dans la séquence d'entrée du LLM est immédiatement disponible pour son traitement, tandis que les connaissances implicites acquises lors du pré-entraînement sont plus difficiles et plus incertaines à récupérer.
L'image au dessus illustre un LLM avec un prompt pour le contexte.
C'est pour ça que les LLM ont étés dotés de la capacité de **scraping** sur internet. Ils font des recherches pour avoir du contexte et etre en mesure de répondre à la question.


![[Pasted image 20260516103109.png]]
Le Zero-Shot prompting est simplement le concept lorsqu'on donne une tache nouvelle à un LLM à laquelle il n'a pas été entraîné

![[Pasted image 20260516104024.png]]
Pour des tâches plus complexes, le zero-shot prompting est souvent insuffisant. Donnez des exemples dans l'instruction est ce qu'on appelle le few-shot Learning.
![[Pasted image 20260516104256.png]]