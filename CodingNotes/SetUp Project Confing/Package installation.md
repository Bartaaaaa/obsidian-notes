**YARN** : Open-source package manager pour gérer les dépendances dans les projets javascripts.
**NPM** (Node Package Manager): Package manager for Node.js. Le but est de pouvoir déclarer toutes les dépendances d'un projet javascript dans un fichier package.json. Ainsi quand une personne rejoint le projet, elle tape npm install pour avoir toutes les dépendances installées (dans node_modules). On peut ensuite lancer le projet avec npm start.
Le fichier package-lock.json est un fichier de verrouillage de dépendances, il décrit ce qui est installé. Il permet de garantir la reproductibilité et la cohérence des installations. Quand on fait npm install, npm lit package.json, consulte package-lock.son et installe les versions exactes du lock file. Si package-lock.json n'existe pas, il le crée. Il est ensuite mis à jour automatiquement par npm lors de mise à jour de paquets. 



source ven.sh

what is OCR
ollama
