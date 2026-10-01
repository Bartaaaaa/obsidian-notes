**Pour que git se souvienne du nom et du token à chaque push :**
```bash
git config --global credential.helper cache   # garde en mémoire 15 min seulement
git config --global credential.helper 'cache --timeout=36000'   # 10h
git config --global credential.helper store   # garde pour toujours
```
⚠️ `cache` garde le token **en mémoire** pendant 15 min par défaut, après il faut le retaper. `store` le garde définitivement, mais **en clair** dans le fichier `~/.git-credentials` (n'importe qui ayant accès à ta session peut le lire). Le plus propre : passer par **SSH** (clé SSH ajoutée sur GitLab/GitHub) et ne plus du tout utiliser de token.

**Dé "add" des fichiers :**
```bash
git reset
```

**Dé "commit" des fichiers :**
```bash
git reset --soft HEAD~1
```

**Plus dé add :**
```bash
git reset HEAD~1
```

**Ou tout enlever (modifs comprises) :**
```bash
git reset --hard HEAD~1
```

**Supprimer une branche git locale :**
```bash
git branch -D <nomBranche>
```

**Arreter tous les conteneurs d'un coup :**
```bash
docker stop $(docker ps -q)
```

**Stash un fichier en particulier :**
```bash
git stash push <Chemin du fichier>
```

**Enlever un fichier déjà push :**
```bash
echo "<fichier>" >> .gitignore   # sinon il sera re-ajouté au prochain git add .
git rm -r --cached <fichier>     # le retire du suivi git, mais le garde sur ton disque
git add .
git commit -m "Retire <fichier> du dépôt"
git push
```
Il faut bien un **commit** entre le `add` et le `push`, sinon il n'y a rien à pousser.
⚠️ Le fichier disparaît des prochaines versions, mais il reste visible dans **l'historique** des anciens commits. Si c'était un secret (mot de passe, token, `.env`), il faut considérer qu'il a fuité et **le changer**.

**Changer l'URL du dépôt distant (remote) :**
```bash
git remote set-url origin "gitclone/data"
```

**Remettre à un dossier ou fichier à la version de 'main' :**
```bash
git fetch origin   # pour avoir la dernière version de main du serveur
git checkout origin/main -- chemin/vers/ton/dossier
```

**Déplacer un commit poussé par erreur sur une mauvaise branche vers une nouvelle :**

1. Créer et sécuriser la nouvelle branche avec le commit :
```bash
git checkout -b <nouvelle-branche>
git push -u origin <nouvelle-branche>
```
2. Nettoyer l'ancienne branche locale :
```bash
git checkout <mauvaise-branche>
git reset --hard HEAD~1
```
3. Nettoyer l'ancienne branche sur le serveur distant :
```bash
git push origin <mauvaise-branche> --force
```

**Commandes terminal :**
```bash
sudo tail -f *.log   # montre les logs en live
```
Atexo pr la sandbox :
```bash
ssh etc
cd $SEM_LOGS_DIR
# puis la commande des logs
```

**Commande cherry pick :**
Lorsque tu crées une nouvelle branche, tu peux récup un commit d'une autre branche et le mettre sur cette branche :
```bash
git cherry-pick <hashDuCommit>   # ex : git cherry-pick 6e025fccc
```
On donne le **hash du commit** (trouvable avec `git log --oneline`), pas le nom d'une branche. Le commit est recopié (avec un nouveau hash) sur ta branche actuelle.

**Emmener des modifs PAS encore commitées sur une autre branche :**
T'as commencé à coder sur branche2 mais t'aurais dû être sur branche1, et t'as **pas encore commité** : `git checkout branch1` (ou `git switch branch1`) → tes modifs non commitées te suivent sur branche1. Ça marche que pour les modifs pas commitées (et si elles entrent pas en conflit avec branche1, sinon git refuse : dans ce cas `git stash`, `git checkout branch1`, `git stash pop`).
⚠️ Ça c'est **pas** un rebase.

**Rebase une branche :**
Le rebase sert à **rejouer tes commits par-dessus une autre branche**, comme si t'étais parti de sa dernière version.
Ex : t'as créé `ma-feature` depuis `develop`, entre-temps des collègues ont ajouté des commits sur `develop`. Tu veux récupérer leur travail sous le tien :
```bash
git checkout ma-feature
git fetch origin
git rebase origin/develop   # tes commits sont "décollés" puis rejoués un par un après le dernier commit de develop
# en cas de conflit : tu corriges, git add <fichier>, puis git rebase --continue (ou --abort pour annuler)
git push --force-with-lease # obligatoire si la branche était déjà poussée, car l'historique a été réécrit
```
```
Avant :                         Après git rebase develop :
develop:    A---B---C           develop:    A---B---C
             \                                       \
ma-feature:   D---E             ma-feature:            D'---E'
```
Tes commits D et E deviennent D' et E' (nouveaux hash, même contenu). Résultat : un historique bien linéaire.
Pour **changer la base** d'une branche (t'es parti de la mauvaise branche) : `git rebase --onto <bonne-branche> <mauvaise-branche> ma-feature`, ou la méthode cherry-pick juste en dessous, plus simple.
⚠️ Règle d'or : ne pas rebaser une branche sur laquelle d'autres personnes travaillent, car ça réécrit l'historique.

## La démarche la plus sûre : repartir de la support et cherry-pick

C'est plus simple qu'un `rebase --onto` et ça ne réécrit pas la branche déjà poussée.

```bash
git checkout support-release-candidate/2026-00.02.05
git pull                                   # rattrape les 32 commits
git checkout -b fix/SEM-8826               # nouvelle branche depuis la support
git cherry-pick 6e025fccc                  # reprend uniquement ton commit
git log --oneline origin/support-release-candidate/2026-00.02.05..HEAD   # doit n'afficher QUE ton commit
git push -u origin fix/SEM-8826
```

docker exec -u 1001 -w /data/apache2/sem-docker.local-trust.com/htdocs/sem sem php bin/console doctrine:migrations:generate


```bash
git show a3841742a:docker2/scripts/fix-permissions.sh | bash
docker exec sem id www-data     
```