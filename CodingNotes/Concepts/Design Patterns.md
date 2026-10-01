## Singleton
Permet à une classe d'avoir une instance unique dans tout le projet. Son but est de réguler l'état global de l'application. Le logger est un exemple courant, qui a une seule instance et heureusement, ce qui nous évite de chercher dans plusieurs endroits pour trouver les logs qu'on souhaite (pareil pr une connexion à la BDD ou la config de l'app).
**Comment on l'écrit ?** Attention, on ne fait **pas** la vérification dans le constructeur : un constructeur crée *toujours* un nouvel objet quand on fait `new`, il ne peut pas "renvoyer l'ancien". L'astuce :
1. On met le constructeur en **privé** → personne ne peut faire `new Logger()` depuis l'extérieur
2. On garde l'instance dans un attribut **statique** de la classe
3. On passe par une méthode **statique** `getInstance()` : si l'instance existe pas encore, elle la crée, sinon elle renvoie celle qui existe
```php
class Logger
{
    private static ?Logger $instance = null;

    private function __construct() {} // personne ne peut faire new Logger()

    public static function getInstance(): Logger
    {
        if (self::$instance === null) {
            self::$instance = new Logger(); // créé une seule fois
        }
        return self::$instance;             // ensuite on renvoie toujours le même
    }
}

$a = Logger::getInstance();
$b = Logger::getInstance(); // $a et $b sont le même objet
```
Note : le Singleton est souvent critiqué (état global, dur à tester). En Symfony on n'en écrit quasi jamais : les **services du DIC** sont déjà instanciés une seule fois par défaut, et on les injecte.

## Strategy
Un Design Pattern qui permet de **changer de comportement (d'algorithme) à la volée**, sans toucher au code qui l'utilise. Au lieu d'avoir un gros if/else (ou switch) dans le code, on isole chaque comportement dans **sa propre classe**, et toutes ces classes respectent **la même interface**. L'appelant reçoit une stratégie et l'utilise sans savoir laquelle c'est.

3 éléments :
- **L'interface (la stratégie)** : le contrat commun, ex `PaymentStrategy` avec une méthode `pay()`
- **Les stratégies concrètes** : une classe par comportement (`CreditCardPayment`, `PaypalPayment`…)
- **Le contexte** : la classe qui utilise une stratégie (`Checkout`). Elle a juste un attribut de type `PaymentStrategy`, qu'on peut remplacer quand on veut.

Un exemple courant est celui du paiement : 
L'utilisateur peut changer le moyen de paiement à tout moment, le Checkout s'en fiche, il appelle juste `pay()`.
```typescript
// 1. Le contrat commun
interface PaymentStrategy {
  pay(amount: number): void;
}

// 2. Une classe par comportement
class CreditCardPayment implements PaymentStrategy {
  constructor(private cardNumber: string) {}
  pay(amount: number) { console.log(`${amount}€ payés par carte`); }
}

class PaypalPayment implements PaymentStrategy {
  constructor(private email: string) {}
  pay(amount: number) { console.log(`${amount}€ payés via PayPal (${this.email})`); }
}

// 3. Le contexte : il ne connaît QUE l'interface
class Checkout {
  constructor(private strategy: PaymentStrategy) {}

  setStrategy(strategy: PaymentStrategy) { this.strategy = strategy; } // on change à la volée

  checkout(amount: number) { this.strategy.pay(amount); } // aucun if/else !
}

const checkout = new Checkout(new CreditCardPayment("4242..."));
checkout.checkout(50);                                  // 50€ payés par carte
checkout.setStrategy(new PaypalPayment("moi@mail.com")); // l'user change d'avis
checkout.checkout(50);                                  // 50€ payés via PayPal
```
Avantage : pour ajouter Bitcoin, on crée juste une classe `BitcoinPayment` → on ne modifie pas `Checkout` (c'est le **O de SOLID** : ouvert à l'extension, fermé à la modification).
⚠️ Les méthodes ne doivent **pas** être statiques : c'est justement parce que ce sont des **objets** qu'on peut les passer en paramètre et les échanger.

**Différence avec Factory :** la Factory sert à **créer** le bon objet, la Strategy sert à **changer de comportement**. Souvent on combine les deux : la Factory crée la bonne stratégie selon le choix de l'user, puis on la donne au Checkout.

## MVC - Model View Controler (Pattern architectural)
Sépare l'application en trois responsabilités distinctes : 
**Model** : Les données et la logique métier (classes BDD + Services et repository)
**View** : Ce qui est affiché à l'utilisateur
**Controller** : Recoit les requetes, appelle le model et choisit le View à retourner

## Factory
Un design pattern qui délègue la création d'objets à une méthode dédiée, au lieu d'utiliser `new` directement partout dans le code. Le but : centraliser la logique de création, surtout quand elle est complexe ou qu'elle dépend d'une condition.

Le code appelant demande juste "donne-moi un paiement de type X", sans savoir comment cet objet est construit en interne.
- centralise la logique de création (un seul endroit à modifier si la création change)
- le code appelant ne dépend pas des classes concrètes, seulement de l'interface `Payment`
- facile d'ajouter un nouveau type sans toucher au code qui utilise la Factory

```
// 1. L'interface commune (le contrat)
interface Payment {
    process(amount: number): void;
}

// 2. Les classes concrètes (la logique spécifique)
class CreditCardPayment implements Payment {
    process(amount: number): void {
        console.log(`Paiement de ${amount}€ via Carte Bancaire traité.`);
    }
}

class PaypalPayment implements Payment {
    process(amount: number): void {
        console.log(`Paiement de ${amount}€ via PayPal traité.`);
    }
}

// 3. La Factory (le centre de création)
class PaymentFactory {
    static createPayment(type: string): Payment {
        switch (type.toLowerCase()) {
            case 'credit_card':
                return new CreditCardPayment();
            case 'paypal':
                return new PaypalPayment();
            default:
                throw new Error(`Le type de paiement '${type}' n'est pas supporté.`);
        }
    }
}

// --- 4. Le code appelant (Client) ---
// Le client demande juste "donne-moi un paiement de type X"
function handleCheckout(paymentType: string, amount: number) {
    try {
        // Aucune utilisation de "new" ici, on délègue à la Factory
        const paymentMethod = PaymentFactory.createPayment(paymentType);
        
        // Le client ne connaît que l'interface "Payment" et sa méthode "process"
        paymentMethod.process(amount); 
    } catch (error) {
        console.error(error.message); // ex : type de paiement non supporté
    }
}

// Exécution
handleCheckout('paypal', 150);       // Affiche : Paiement de 150€ via PayPal traité.
```

## Builder
Builder est un design pattern qui a pour but de résoudre le problème du constructeur géant possédant 10 paramètres.
Le principe est de laisser le constructeur avec les éléments essentiels, et ce qui peut varier on y crée des méthodes dédiées qu'on appelle en fonction du besoin.
PHP
On passe de : 
```
// Constructeur illisible et source d'erreurs d'ordre de paramètres
$maison = new House(4, 2, 1, true, false, true, "tuile", null, 2);
```
à :
```
$builder = new HouseBuilder();

$maison = $builder
    ->setWalls(4)
    ->setWindows(2)
    ->setRoof("tuile")
    ->hasGarage(true)
    ->build(); // Renvoie l'objet House final assemblé et validé
```
Les méthodes se mettent dans une classe "builder", ainsi on aura la classe House, et la classe HouseBuilder. Le housebuilder construit un objet House dans une de ses méthodes avec les bons params.