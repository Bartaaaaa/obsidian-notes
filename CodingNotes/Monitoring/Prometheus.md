Prometheus est un service de monitoring. Il permet de détecter les éventuels problèmes ou bugs qui pourraient survenir après le déploiement grâce à des **métriques** (des valeurs chiffrées dans le temps : CPU, RAM, nombre de requêtes, temps de réponse…) et des **alertes**. Attention, Prometheus ne gère **pas les logs** (le texte des erreurs) : pour ça on utilise ELK ([[Suite ELK]]) ou Loki. Et les dashboards c'est Grafana. Permet d'analyser les performances de l'application, identifier les goulots d'étranglements, temps de réponses latents.
![[Pasted image 20251217100653.png]]
Son architecture est basée sur un modèle de type **pull** (c'est Prometheus qui va lui-même chercher les données chez les services, en appelant régulièrement leur URL `/metrics` : c'est le **scraping**. Les services, eux, n'envoient rien, ils exposent juste leurs chiffres) dans lequel Prometheus récupère les données de métriques auprès des cibles qu'il surveille. Son composant principal est son serveur qui va collecter les données à intervalle régulier. Ses targets sont des app, services, serveurs, conteneurs,...
Les données sont stockées dans une BDD locale conçue pour les données de séries temporelles. Il a son propre langage de requête : **PromQL** (ex : `rate(http_requests_total[5m])` = nombre de requêtes par seconde sur les 5 dernières minutes).

Il est possible de faire de la visualisation de données en le combinant à [[Grafana]].
D'ailleurs Prometheus stocke les valeurs sous forme de **séries temporelles** : nom de la métrique + labels / timestamp / valeur. Ex : `http_requests_total{route="/login", status="200"}` → à 10h00 : 1520, à 10h01 : 1534…

Pour récupérer des données depuis MongoDB, on utilise MongoDB Exporter. On obtient l'architecture suivante : 
MongoDB → MongoDB Exporter → Prometheus → Grafana
MongoDB : BDD qui stocke les données et exécute les requêtes.
MongoDB Exporter qui transforme les statistiques internes de MongoDB en métriques compréhensibles par Prometheus.
Prometheus : scrappe et collecte ces métriques et les stoques
[[Grafana]] : Récupère les données dans Prometheus en s'y connectant et visualise les métriques sous formes de dashboards et graphiques

Prometheus contient deux volumes : 
**Le premier** qui contient le fichier prometheus.yml pour lui dire à quoi il doit accéder
**Le deuxieme** prometheus_data pour ne pas perdre les données à l'arret du docker

**Note histoire :** Dans la mythologie grecque, Prometheus était un titan qui était connu pour son ingéniosité et son amour pour l’humanité. Il a désobéi aux dieux en volant le feu sacré et en le donnant aux humains, leur offrant ainsi des connaissances et des capacités qui les ont propulsés vers le progrès.