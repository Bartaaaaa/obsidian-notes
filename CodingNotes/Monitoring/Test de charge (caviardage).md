**K6** est un outif développé par Grafana qui va physiquement exécuter le test de charge
 C'est lui qui va stresser le GPU.
 Il va lire le script et le jeu de données, et va simuler des dizaines ou centaines d'utilisateurs virtuels.
 Il va bombarder trois API : LLM, CLIP & YOLO  avec les images et textes des PDF
 Pendant qu'il tire sur les serveurs, k6 va mesurer le temps de réponse de chaques API avec les images et textes des PDF

**InfluxDB** est une BDD TSDB (time series database), une bdd optimisée pour stocker des données horodatées (liées au temps). Pendant le test de charge, k7 génère des milliers de métriques à la seconde. Il faut un endroit ultra rapide pour écrire ces données en temps réel. C'est le rôle d'influxDB.

**Grafana** : Plateforme web de visualisation de données, et génère des graphiques dont les données proviennent d'influxDB. Il lit les données horodatées et génère les graphiques en temps réel.