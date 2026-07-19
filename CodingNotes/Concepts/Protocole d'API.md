Une API (Application Programming Interface) est un **contrat d'interface** qui permet à deux logiciels de communiquer sans qu'ils aient besoin de connaître les détails de leur implémentation interne. Il existe plusieurs protocoles pour construire une API, les principaux sont REST, SOAP & GraphQL

## ****API REST****
**API REST** est un **style d'architecture** basé sur le protocole HTTP. On peut le voir comme un ensemble de convention & de contraintes.
- Expose les ressources sur lesquelles on va venir taper à l'aide des verbes HTTP **CRUD**, chaque ressources à son URL unique
- Stateless (une requête ne peut dépendre d'une autre, et chaque requête contient toutes les informations nécessaires à chaque fois)
- Idempotence : GET, PUT & DELETE sont idempotentes (si on les joue 100 fois d'affilées, ça fait le même résultat que si on les avait joué qu'une seule fois, sauf pour POST & Patch)
- Domination du format JSON, mais peut accepter du XML, HTML, etc.
- Code de statut HTTP (200, 201, etc) plutôt qu'un champ "success : true"
- Cache : certaines requêtes **GET** peuvent êtres mises en cache, pas besoin de rappeler le serveur.

## **API SOAP**
**API SOAP** est un protocole strict et normé. Au lieu de manipuler des ressources, il est orienté actions/RPC. L'API expose des opérations et pas des ressources.
**WSDL (Web services Description Language)** est le contrat central de SOAP, équivalent de la documentation OpenAPI pour REST, mais obligatoire pour SOAP.
C'est un fichier XML qui décrit :
- quelles opérations le service expose, quels paramètres chaque opération attend, quel format de réponse elle attend.
**L'enveloppe SOAP** : 
Exemple appel : 
#### Appel 1 : je veux l'utilisateur
```http
POST /soap-service HTTP/1.1
Content-Type: text/xml

<soap:Envelope>
  <soap:Body>
    <GetUtilisateur>
      <id>1</id>
    </GetUtilisateur>
  </soap:Body>
</soap:Envelope>
```
![[Pasted image 20260712214541.png]]
Le body contient littéralement un appel de fonction.
SOAP fonctionne avec tous les protocoles de transport (HTTP, SMTP etc.)
Format de données : XML (assez volumineux, et le parsing est lourd)
Fiable pour la sécurité et la fiabilité avec des normes standards.
Messages volumineux, moins performants
Bien adapté aux applications existantes (legacy) et API privées/internes — en particulier dans les secteurs **bancaire, assurance, systèmes d'entreprise** qui ont besoin de contrats stricts et de garanties transactionnelles fortes (là où une simple API REST + HTTPS ne suffit pas toujours).

## **API GraphQL**
**API GraphQL** est un **langage de requête** qui donne le **contrôle total des données au client**. Le client envoie une requête textuelle (syntaxe GraphQL) vers un **endpoint unique** (`/graphql`), et le serveur répond en JSON avec **exactement** la structure demandée.

Ex: Sur une route REST /utilisateurs/1 tu télécharges tout le profil même si t'as besoin uniquement du nom, GraphQL on peut exiger  ce qu'on veut et le serveur enverra uniquement la ressource demandée. C'est déjà plus léger que les autres types d'API. 
De plus, avec GraphQL, en une seule requête on peut récupérer des informations là ou en reste il en faudrait 2.
```graphql
query {
  utilisateur(id: 1) {
    nom
    commandes {
      montant
    }
  }
}
```
**Trois types d'opérations :** 
Query -> Lire des données (= GET en REST)
Mutation -> Créer/modifier/supprimer (POST, PUT, DELETE)
Subscription -> Recevoir des mises à jour en temps réel (webSocket)

Utilité : Apps mobiles/front avec besoins de données variables
![[Pasted image 20260713180809.png]]

API first