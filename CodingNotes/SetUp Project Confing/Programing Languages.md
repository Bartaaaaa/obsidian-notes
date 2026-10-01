
**Web programing :** 
**Javascript** : langage de programmation qui permet d'effectuer des actions sur un site internet côté front, donc navigateur. Peut aussi tourner côté serveur grâce à Node.js (voir plus bas).
**PHP** : Langage de programmation côté serveur. Il se démarque par sa simplicité de déploiement et son intégration native avec HTML.
**Java** : Langage de programmation côté serveur. Il se démarque par sa portabilité grâce à la JVM (Java Virtual Machine) : "Write once, run anywhere". Très utilisé en entreprise pour des applications robustes et scalables, ainsi que pour le développement Android.
**Ruby** : Langage orienté objet, connu pour sa simplicité. Très utilisé pour le développement web côté serveur, notamment avec le framework Ruby on Rails.
**.NET** : Ce n'est pas vraiment un langage mais une **plateforme/framework** Microsoft qui permet de faire tourner C#, VB.NET, etc. Utilisé pour des applications Windows, web et mobiles. Depuis .NET Core (2016), il est aussi multiplateforme : ça tourne sur Linux et macOS.
**HTML** :  Langage de balise hypertexte : représenter le contenu et la structure d'une page web
**CSS** : Utilisé pour créer des visuels sur le site.
**SQL** : Langage de **requête** pour la gestion de bases de données relationnelles. C'est un langage **déclaratif** : on dit *ce qu'on veut* ("donne-moi les users de plus de 18 ans") et pas *comment* le faire, c'est la BDD qui choisit comment aller chercher les données.

**Visualisation, IA :** 
**Python** : Langage polyvalent utilisé en datascience, IA, machine learning. Peut aussi être utilisé pour faire des serveurs web.


**Composants, jeux :** 
 **C** : Langage bas niveau, très proche du matériel. Utilisé pour programmer des systèmes d'exploitation, des microcontrôleurs et des logiciels nécessitant de hautes performances.
 **C++** : Extension de C avec de la programmation orientée objet. Très utilisé dans le développement de jeux vidéo (Unreal Engine), logiciels graphiques et applications haute performance.
**C#** : Langage créé par Microsoft, utilisé principalement pour les applications Windows, le développement de jeux avec Unity, et les applications .NET.
**Rust** : Langage système (comme C/C++) mais conçu pour être plus sûr en évitant les erreurs mémoire. Utilisé pour des outils systèmes, WebAssembly et logiciels très performants.

**Go** : Langage créé par Google, simple et performant. Très utilisé pour les serveurs backend, les APIs et les outils cloud (Docker et Kubernetes sont écrits en Go).


////////////////////

**Node.js** : Environnement d'exécution permettant d'utiliser JavaScript en dehors d'un navigateur. 
- **Côté serveur :** Créer des backends et des API ultra-performants grâce à son architecture non-bloquante, idéale pour gérer de multiples connexions simultanées (temps réel, tchat, streaming).
- **Côté front-end (Outillage) :** Servir de moteur pour l'écosystème de développement. Il fait tourner les gestionnaires de paquets (npm, yarn), les serveurs de développement locaux, et s'occupe de compiler les projets (React, Vue) avant leur mise en ligne.

**Frameworks à connaître :** 
**NestJS** : Framework javascript côté serveur (Node.js). Il se démarque par son architecture très structurée inspirée d'Angular, populaire pour des APIs complexes en entreprise.
**Angular** : Framework front créé par Google. C'est un framework **"tout-en-un"** qui impose une structure stricte. Contrairement à React qui est une librairie qu'on complète avec d'autres outils, Angular a déjà tout intégré. Utilise **TypeScript par défaut**. 
**Next.js** : Framework javascript basé sur React, qui permet le rendu côté serveur (SSR) ou la génération de pages statiques. Il se démarque en résolvant le problème de SEO de React. Mais un projet React n'utilise **pas forcément** Next.js : on peut faire du React "pur" avec Vite (SPA), ou utiliser d'autres frameworks comme React Router (v7, ex-Remix). Next.js est juste le framework le plus populaire.
**Vue.js** : Alternative à React côté front, considéré comme plus **simple à prendre en main** que React ou Angular. Il emprunte le meilleur des deux (composants de React, structure d'Angular). 

**PHP Symfony :** 
**Spring Boot** : Framework Java qui permet de créer des **microservices et APIs** robustes avec très peu de configuration. C'est le standard dans les très grandes entreprises et les banques qui tournent en Java.

**Django** : Framework Python avec une philosophie **"batteries included"** : admin auto-généré, authentification, ORM... tout est déjà là. On monte un projet complet très rapidement. Très utilisé pour des projets data/IA qui ont aussi besoin d'un serveur web.
**Flask** : Framework Python minimaliste, à l'opposé de Django il ne fournit **que le strict minimum** et tu construis ce dont tu as besoin. Idéal pour des **petites APIs** ou exposer un modèle de machine learning rapidement.
**FastAPI** : Un des frameworks Python les plus **performants** (asynchrone) qui génère automatiquement une **documentation interactive** (Swagger). Devient le standard pour les APIs modernes en Python, notamment dans la data et l'IA.

**Librairies à connaître :** 
**ReactJS**: Librairie javascript, connue pour sa programmation en composants. Mais React "pur" (en SPA) est moins bon pour le SEO. Attention la raison c'est **pas** le .tsx : le .tsx est de toute façon compilé en JS avant d'arriver au navigateur. La vraie raison c'est le **rendu côté client (CSR)** : le serveur envoie un HTML vide (`<div id="root"></div>`) et c'est le JS qui construit la page dans le navigateur, donc un robot qui lit juste le HTML ne voit rien (voir [[React]]). Next.js règle ça avec le SSR.
**TailwindCSS** : Librairie CSS utilitaire très populaire aujourd'hui, permet de styliser directement dans le HTML sans écrire de CSS.

**CMS :** 
**Drupal** : CMS PHP flexible et robuste, orienté sites complexes et institutionnels.
**WordPress** : CMS PHP simple et très populaire (~40% du web). Idéal pour blogs, vitrines et e-commerce via WooCommerce.