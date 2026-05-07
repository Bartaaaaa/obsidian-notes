**Global exception filter :** 
Goblab Interceptor : 

**Request Id Interceptor :**
Rajouter un id unique à chaque requete pour les retrouver rapidement

Les modules

Une méthode statique (`@staticmethod`) est une méthode qui n'est liée ni à une instance spécifique, ni à la classe elle-même. Elle se comporte comme une fonction classique, mais est **encapsulée** dans la classe pour garder le code cohérent (namespace). Elle est principalement utilisée pour des opérations sans état (**stateless**) ou des helpers internes. Je dirai que lorsqu'une méthode n'a pas besoin du "self" ou appeler "this" on peut la rendre statique.

La méthode d'une classe @classmethod.
C'est une méthode qui a besoin de connaître la classe, souvent pour **créer une nouvelle instance** de cette classe ou lire une configuration partagée. Une méthode de class ne peut pas avoir accès à des variables définies dans un init.
Conclusion : 
![[Pasted image 20260216165936.png]]Pour cls, les données partagées par toute la classe sont des données définis ailleurs que dans le constructeur OU lorsqu'on instancie une classe avec cette méthode.
Ex : class User:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    @classmethod
    def from_dict(cls, data):
        return cls(data["name"], data["age"])
Php symfony: un cron avec scheduler qui exécuter une commande
Mais on a besoin de messenger consumme mon scheduler, à voir coment ça marche

NODE_ENV=
Executer les commande avec docker exec

C quoi le DOM

**5 - Qu'est-ce que le SSR ? Quels sont les avantages du SSR ?**
Le SSR, ou Rendu Côté Serveur (Server-Side Rendering) est une technique de rendu
d'applications web où le contenu de la page est généré sur le serveur avant d'être envoyé
au navigateur de l'utilisateur. Dans une application SSR, le serveur génère le HTML completde la page, y compris le contenu, le style, et les scripts, puis l'envoie au client. Le navigateur
reçoit ensuite cette page HTML déjà rendue et peut l'afficher immédiatement.
Quelques avantages :
- **Meilleures performances initiales :** L'utilisateur voit la page tout de suite (le HTML est prêt à l'emploi) sans attendre le téléchargement et l'exécution du JavaScript.
- **Optimisation du référencement (SEO) :** Les crawler de Google lisent directement le contenu dans le HTML généré par le serveur, ce qui garantit une bonne indexation.
- **Meilleure accessibilité :** Le code HTML sémantique et complet dès le départ est beaucoup plus facile à déchiffrer pour les lecteurs d'écran (utilisés par les personnes malvoyantes).
- **Réduction de la charge du client :** C'est le serveur (qui est puissant) qui fait le calcul lourd de construction de la page. Le client (même avec un vieux téléphone ou un ordinateur peu performant) n'a qu'à afficher le résultat.

XSS DOM BASED, pentest

**4 - Comment optimisez-vous une requête SQL pour améliorer les performances de la**
**base de données ?**
- Mes pratiques pour optimiser mes requêtes SQL sont : 
- Bannir Select etoile  , renvoyer que les données voulues.
#### Requêtes SARGable (Search Argument Able)
Une requête SARGable est une requête formulée de manière à ce que le moteur de base de données puisse exploiter efficacement les index existants pour isoler les données.
**La règle d'or :** Ne jamais appliquer de fonction, de calcul mathématique ou de modification sur la colonne ciblée dans la clause `WHERE` ou `JOIN`. La colonne doit être isolée d'un côté de l'opérateur de comparaison, face à une constante ou une variable de l'autre côté.
**L'impact technique :** Si une fonction est appliquée à la colonne indexée, le moteur SQL est forcé d'évaluer cette fonction sur _chaque ligne_ de la table avant de pouvoir effectuer la comparaison. Cela désactive l'indexation et déclenche un **Full Table Scan** (parcours complet de la table), ce qui effondre les performances sur de grands volumes de données.
**Non-SARGable (Full Table Scan) :**
```
SELECT * FROM users WHERE YEAR(date_of_birth) = 1996;
SELECT * FROM users WHERE email LIKE '%@gmail.com';
SELECT * FROM users WHERE CONCAT(first_name, ' ', last_name) = 'Jean Dupont';
```
Sargable : 
```
SELECT * FROM users WHERE date_of_birth >= '1996-01-01' AND date_of_birth <= '1996-12-31';
SELECT * FROM users WHERE email LIKE 'jean.%';
SELECT * FROM users WHERE first_name = 'Jean' AND last_name = 'Dupont';
```
Pour le deuxieme : _Problème :_ Le caractère générique `%` placé au début de la chaîne empêche l'utilisation de l'index. **Un index textuel se lit de gauche à droite** (comme un dictionnaire). Si la première lettre est inconnue, le moteur doit scanner toute la colonne. Alors que s'il est à la fin, il détecte directe par rapport aux premiers éléments.
- Utilisation de LIMIT pour ne pas renvoyer toutes les données inutillement

**Différence PUT & PATCH :** 
Put : modifier toute la donnée, PATCH : modifier qu'une partie

**Les codes retours :** 
Check : 
https://tanstack.com/query/latest

a quoi sert composer.json, package.json mais pk composer
ouvrir package deb avec sudo dpkg -i