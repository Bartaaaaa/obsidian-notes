Logstash s’insère dans la stack Elastic (ELK/Elastic Stack) aux côtés d’Elasticsearch, Kibana et Beats. Il joue le rôle de **moteur ETL** (Extract, Transform, Load), en centralisant et normalisant les données avant leur indexation dans Elasticsearch
. Il s'accompagne d'un ensemble de modules permettant d'ingérer des multitudes de formats de logs courants, notamment les BDD, fichiers de logs, files de messages, api REST,....

**Concrètement ?** 
Logstash est un moteur de collecte et de traitement des données via plug-in. Il est doté de nombreux plug-ins qui permettent de configurer facilement l'outil pour collecter, traiter et transférer les données dans un grand nombre d'architectures variées.

Lorsque vous configurez le fichier, il est utile de considérer Logstash comme un pipeline qui prend les données à une extrémité, les traite d’une manière ou d’une autre et les envoie à leur destination (dans ce cas, la destination est Elasticsearch). Un pipeline Logstash comporte deux éléments obligatoires, `input` (l’entrée) et `output` (la sortie), et un élément optionnel, `filter` (le filtre). Les plugins d’entrée consomment les données d’une source, les plugins de filtrage traitent les données, et les plugins de sortie écrivent les données vers une destination.
![[Pasted image 20251218181928.png]]
Faut lui configurer un fichier logstash.conf dans lequel on va lui définir un json avec les données sur lesquelles il doit taper (l'URI), la collection en question, l'host (9200) ...

Mini exemple de pipelinelogstash : 
```conf
input {
  mongodb {
    uri => "mongodb://localhost:27017"
    database => "logs_db"
    collection => "logs"
  }
}

filter {
  mutate {
    add_field => { "source" => "mongodb" }
  }
}

output {
  elasticsearch {
    hosts => ["http://localhost:9200"]
    index => "mongodb-logs"
  }
}
```
`