[[Package installation]]
Pour que git se souvienne du nom et du token à chaque push : 
git config --global credential.helper cache


Dé "add" des fichiers : 
git reset

Dé "commit" des fichiers : 
git reset --soft HEAD~1
Plus dé add : 
git reset HEAD~1
Ou tout enlever (modifs comprises): 
git reset --hard HEAD~1

Supprimer une branche git locale : 
git branch -D <noBranche>

Arreter tous les conteneurs d'un coup : 
docker stop $(docker ps -q)

Stash un fichier en particulier : 
git stash push <Chemin du fichier>

Enlever un fichier déjà push : 
git rm -r --cached <fichier>
Puis git add et git push

Reset a clone source : 
git remote set-url origin "gitclone/data"

Remettre à un dossier ou fichier à la version de 'main'
git checkout origin/main -- chemin/vers/ton/dossier

Déplacer un commit poussé par erreur sur une mauvaise branche vers une nouvelle : 

1. Créer et sécuriser la nouvelle branche avec le commit : 
git checkout -b <nouvelle-branche> 
git push -u origin <nouvelle-branche>
2. Nettoyer l'ancienne branche locale : 
git checkout <mauvaise-branche> 
git reset --hard HEAD~1
3. Nettoyer l'ancienne branche sur le serveur distant : 
git push origin <mauvaise-branche> --force