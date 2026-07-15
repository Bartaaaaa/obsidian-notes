## Singleton
Permet à une classe d'avoir une instance unique dans tout le projet. Son but est de réguler l'état global de l'application. Le logger est un exemple courant, qui a une seule instance et heureusement, ce qui nous évite de chercher dans plusieurs endroits pour trouver les logs qu'on souhaite. Dans le constructeur il suffit de vérifier si l'instance n'existe déjà pas, et si non on la crée.

## Strategy
Un Design Pattern qui permet à une classe d'avoir différentes méthodes en fonction de l'action de l'utilisateur. Au lieu d'avoir de longs if/else dans le code, on isole chaque comportement dans sa propre méthode. L'appelant choisit dynamiquement la stratégie à utiliser.
Les fonctions doivent êtres statiques, et chaque méthodes prend le même argument en paramètre.

Un exemple courant est celui du paiement : 
L'utilisateur peut changer le moyen de paiement à tout moment et le paiement marchera qd même puisqu'il couvrira ces options.
```typescript
class PaymentMethodStrategy {

  const customerInfoType = {
    country: string
    emailAddress: string
    name: string
    accountNumber?: number
    address?: string
  }

  static BankAccount(customerInfo: customerInfoType) {
    const { name, accountNumber, routingNumber } = customerInfo
    // do stuff to get payment
  }

  static BitCoin(customerInfo: customerInfoType) {
    const { emailAddress, accountNumber } = customerInfo
    // do stuff to get payment
  }

  static CreditCard(customerInfo: customerInfoType) {
    const { name, cardNumber, emailAddress } = customerInfo
    // do stuff to get payment
  }
}
```

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