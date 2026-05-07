Prometheus est un service de monitoring. Il permet de détecter les éventuels problèmes ou bugs qui pourraient survenir après le déploiement sous forme de logs ou dashboards. Permet d'analyser les performances de l'application, identifier les goulots d'étranglements, temps de réponses latents.
![[Pasted image 20251217100653.png]]
Son architecture est basée sur un modèle de type **pull** (récupère les données dans des services (scraping), et les services peuvent récupérer des données de lui)dans lequel Prometheus récupère les données de métriques auprès des cibles qu'il surveille. Son composant principal est son serveur qui va collecter les données à interval régulier. Ses targets sont des app, services, serveurs, conteneurs,...
Les données sont stockées dans une BDD locale conçue pour les données de séries temporelles. Il a son propre langage de requete pour les BDD.

Il est possible de faire de la visualisation de données en le combinant à [[Grafana]].
D'ailleurs Prometheus stoque les valeurs sous formes de table: clef/timestamp/valeur

Pour récupérer des données depuis [[Mongo]], on utilise MongoDB Exporter. On obtient l'architecture suivante : 
MongoDB → MongoDB Exporter → Prometheus → Grafana
MongoDB : BDD qui stocke les données et exécute les requêtes.
MongoDB Exporter qui transforme les statistiques internes de MongoDB en métriques compréhensibles par Prometheus.
Prometheus : scrappe et collecte ces métriques et les stoques
[[Grafana]] : Récupère les données dans Prometheus en s'y connectant et visualise les métriques sous formes de dashboards et graphiques

Prometheus contient deux volumes : 
**Le premier** qui contient le fichier prometheus.yml pour lui dire à quoi il doit accéder
**Le deuxieme** prometheus_data pour ne pas perdre les données à l'arret du docker

**Note histoire :** Dans la mythologie grecque, Prometheus était un titan qui était connu pour son ingéniosité et son amour pour l’humanité. Il a désobéi aux dieux en volant le feu sacré et en le donnant aux humains, leur offrant ainsi des connaissances et des capacités qui les ont propulsés vers le progrès.