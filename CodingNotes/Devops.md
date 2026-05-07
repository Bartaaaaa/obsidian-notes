

Si Kubernetes est le "moteur" qui gère tes applications, **Lens est le tableau de bord (dashboard)** qui te permet de piloter ce moteur sans avoir à taper des lignes de commande complexes en permanence.
En effet, Kubernetes fonctionne normalement via un terminal, puissant mais pas visuel. Le rôle de Lens est d'offrir une interface graphique sur les pods, services et le réseau. Mais aussi au troubleshooting (dépannage) en permettant un accès aux logs et au terminal, on peut aussi centraliser les serveurs sur Lens, surveiller des metrics etc.

test
# 📑 Fiche Mémo : Nginx

## 1. Définition

**Nginx** (prononcé "Engine-X") est un **serveur web** open-source (gère les connexions, transmet du html, css, js, parle en HTTP : en gros il prend les commandes et apporte le plat. Contrairement au **serveur d'application** qui exécute le code et traite la logique, bdd, ...). Dans une architecture moderne (comme la vôtre avec Docker), il ne se contente pas de servir des fichiers ; il sert de **porte d'entrée unique** pour toutes les requêtes arrivant sur votre projet.

---
## 2. Ses 3 Rôles Principaux

1. **Serveur de fichiers statiques :** Il livre directement les images, le CSS et le JavaScript sans solliciter PHP (gain de performance).
2. **Reverse Proxy (Proxy Inverse) :** Il reçoit la requête du navigateur et la "transmet" au bon service interne (ex: le conteneur PHP-FPM).
3. **Gestionnaire de protocoles :** Il s'occupe de la sécurité (HTTPS/SSL) et de la redirection des ports.

---

## 3. Structure d'un fichier de configuration (`.conf`)

Le fichier de configuration de Nginx (souvent situé dans `docker/web/nginx.conf` ou `/etc/nginx/conf.d/default.conf`) fonctionne par **blocs** imbriqués :
![[Pasted image 20260119092816.png]]Comme Nginx ne sait pas lire le PHP, il utilise un "traducteur" appelé **FastCGI**. On peut le retrouver dans le fichier nginx.conf.

Harbor ? Helm ? Lens ?