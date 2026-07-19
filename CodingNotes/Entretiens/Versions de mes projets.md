
## **DMM**
**Back End**
Php : 7.4
Symfony : 5.4

Stack legacy d'où les anciennes versions.
7.4 est EOL, plus soutenu, c'est clairement une dette technique à corriger.
Symfony en 5.4 est une LTS (Long Term support, version majeur, symfony tjrs des X.4). LTS un choix légitime, soutenu et stable.
**Front End** 
React : 16.13
Node : 16.2
Create Ract App 3.4

React en 16 possède les hooks et a été simplement la version utilisé lors de la création du projet.
CRA était le standart de scaffolding en 2020, aujourd'hui abandonné, c'est de la dette technique, React l'a retiré depuis 2025 de sa doc.
## **Mires**
**Back End**
Php : 8.4
Symfony : 7.2

Les versions les plus récentes ont étés utilisées lors de la création de l'application.
Version 8 de PHP ajoute des attributs natifs au lieu des commentaires, opérateurs nullsafe, code plus concis dans le constructeur, plus optimisé, soutenu etc.
Symfony n'est pas une LTS mais la version la plus récente avait simplement été selectionnée, la version 7 exige Php en  version 8.X, ça va de pair.
**Front End** 
React : 18.3
Vite :  5.4

Version plus récente de React a été utilisée, 19 existe mais 18 est plus stable.
Vite est le standart actuel : ESM natif (permet au navigateur de mettre uniquement à jour le fichier modifié et ne pas tout rebuild) + esBuild en dev (outil de compilation écrit en GO, rapide).
Vite et pas de Next car Mires est une appli interne, pas besoin de SEO, vite suffit et est simple qu'un framework fullstack.
## **Caviardage :** 
Archi distribuée, un orchestrator côté Django et une API d'inférence FastAPI côté GPU qui communiquent en HTTP. (contrainte matérielle, isoler les modèles GPU-bound). C'est 2 services, "leger pr microservice, plutot architecture orientée service"
Django : 6
Python 3.12 car cette version supporte bien les Machine Learning (torch, CUDA,  transformers, ultralytics). Torch : Biblio python pr construire et entrainer des modeles, CUDA : techno NVIDIA pr exécuter des calculs sur le GPU, Transformers : librairie huggingFace pr le choix des modeles, Ultralytics : Entreprise derrière YOLO : détecter signatures
ElasticSearch 8.10
FastAPI  0.136 pour la partie GPU car FastAPI est asynchrone nativement et très léger donc rapide alors que Django est plus lourd mais apporte plus de features
Les stacks sont pr la plupart modernes et figées, on prend pas automatiquement la plus récente pour ne pas casser un système.

## **Projets persos :** 
**Primobati**
FrontEnd : NextJs 16.2
React : 19.4
UI : Tailwind & Shadcn 
Hébéregement :  Ionos
Formulaires : Web3Forms pour envoi de mails gratuitements
NextJs pour le SEO et car la majorité de projets sur lesquels j'ai bossé étaient en NextJs.


**Vite** est un outil de build : prend le code source et le transforme pour qu'il tourne sur navigateur. C'est le moteur pr compiler le code. Un builder.
**NextJs** : Framework construit par dessu React, qui apporte : routing automatique, SSR/SSG (génération cote serveur ou génération statique (SSG permet de générer l'HTML au moment du build donc good pr SEO)), opti d'images, SEO. Il utilise son propre systeme de build.
**Angular** : Framework autonome
**Webpack** : standard historique pr le build, très configurable mais lent
Bibliothèques : React, Vue
Framework Complet : UI + undler + rounting : NextJs (React), Nuxt (Vue), Angular
**NodeJs** : environnement d'exécution qui permet de lancer du Javascript  en dehors du navigateur, sur un serveur. NodeJs permet de faire du dev cote back avec du SSR, pr tout projet on a besoin de node pr faire tourner les outils de compilation & de build.