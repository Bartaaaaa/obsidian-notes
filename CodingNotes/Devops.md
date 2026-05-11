

Si Kubernetes est le "moteur" qui gère tes applications, **Lens est le tableau de bord (dashboard)** qui te permet de piloter ce moteur sans avoir à taper des lignes de commande complexes en permanence.
En effet, Kubernetes fonctionne normalement via un terminal, puissant mais pas visuel. Le rôle de Lens est d'offrir une interface graphique sur les pods, services et le réseau. Mais aussi au troubleshooting (dépannage) en permettant un accès aux logs et au terminal, on peut aussi centraliser les serveurs sur Lens, surveiller des metrics etc.


# 📑 Fiche Mémo : Nginx

## 1. Définition

**Nginx** (prononcé "Engine-X") est un **serveur web** (comme Apache) open-source  qui a été initialement conçu pour les sites avec beaucoup de visiteurs (performance, stabilité).  Nginx est souvent utilisé comme intermediaire entre le client et un second serveur web en tant que terminateur SSL/TLS pour gérer les taches susceptibles de ralentir le serveur. Il est utilisé par exemple comme cache de contenu afin de réduire la charge sur les serveurs d'application.
Principalement, si le client demande une page qui n'a pas besoin de calcul ((images, fichiers CSS, HTML pur), nginx s'en charge pour renvoyer les infos. Par contre, si la page fait des appeles, nginx transmet l'information au serveur d'application php qui va exécuter le code, renvoyer à nginx qui va renvoyer au client. Nginx faut le voir comme un service qui améliore considérablement ton serveur. C'est le seul port quoi doit avoir ses ports ouverts sur extérieure.

Il joue le rôle de Reverse Proxy : 
**D'abord c'est quoi un Proxy ?** 
Un proxy est un serveur relais qui va intercepter les requetes des utilisateurs d'un réseau privé afin de filtrer les sites interdits, cacher l'adresse IP des employés.
Reverse Proxy a pour role d'intercepter les requetes entrantes pour les distribuer au serveur interne. Cela permet de protéger l'application puisque l'adresse IP du serveur n'est pas exposé et d'organiser le flux entrant des requêtes pour les distribuer aux bons services (front, back, paiement, etc).

**Sécurité SSL/TSL :** sont des protocoles qui chiffrent les échangent entre le navigateur et le serveur (chiffrer les données envoyés dans les requetes). Nginx on parle de terminateur SSL car il recoit les paquets chiffrés du client, les déchiffre et les envoie au serveur d'application. De plus il force le port HTTPS au lieu du HTTP qui n'est pas sécurisé.

Il joue aussi le role du Terminateur SSL/TSL & Reserve Proxy, gestionnaire de protocoles HTTPS
Contrairement au **serveur d'application** qui exécute le code et traite la logique, bdd, ...). Dans une architecture moderne (comme la vôtre avec Docker), il ne se contente pas de servir des fichiers ; il sert de **porte d'entrée unique** pour toutes les requêtes arrivant sur votre projet.


---

## 2. Structure d'un fichier de configuration (`.conf`)

Le fichier de configuration de Nginx (souvent situé dans `docker/web/nginx.conf` ou `/etc/nginx/conf.d/default.conf`) fonctionne par **blocs** imbriqués :
![[Pasted image 20260119092816.png]]Comme Nginx ne sait pas lire le PHP, il utilise un "traducteur" appelé **FastCGI**. On peut le retrouver dans le fichier nginx.conf.


Harbor ? Helm ? Lens ?
WebSocket

