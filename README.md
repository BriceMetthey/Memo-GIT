# Memo-GIT



## References

https://www.atlassian.com/git/tutorials/setting-up-a-repository


## Configurer git

Activer la couleur dans Git

git config --global color.diff auto

git config --global color.status auto

git config --global color.branch auto


git config --global user.name "votre_pseudo"

git config --global user.email moi@email.com

vim ~/.gitconfig

git config --list --show-origin

git config user.name


## Initialiser git

git init


## Indexer et valider

Indexer les modifications qui ont été faites dans les fichiers du répertoire de travail 

git add -A ou git add .

git add <file>

git add <directory>

git add -p => commit par portions


La commande capture un instantané des modifications actuelles :

git commit

=> Lancera un éditeur de texte vous invitant pour un message de validation. Une fois que vous avez entré un message : enregistrez et fermez pour créer la validation réelle.


git commit -m "Initial commit"


Affiche l’état du répertoire de travail et la zone de transit.

git status



Comment mettre à jour (modifier) un commit ?

git add hello.py

git commit --amend


## Comparaisons

Comparaison des changements :

```
mkdir diff_test_repo
cd diff_test_repo
touch diff_test.txt
echo "this is a git diff test example" > diff_test.txt
git init .

git add diff_test.txt
git commit -am "add diff test file"

echo "this is a diff example" > diff_test.txt

git diff

git diff --color-words
```

## Les branches

git branch  => Liste toutes les branches   ou   git branch --list

git branch <branch>   => Créé une nouvelle branche appelée <branch>

Attention : ne fait que créer une nouvelle branche. 
Par ajouter des nouveaux commits : selectionner la branche via :

git checkout + nom de la branche, 

Puis utiliser les commandes git add   et   git commit  



git branch -d <branch>  => detruit la branche spécifiée

git branch -m <branch>  => Renomme la branche courante

git branch -a     => Liste toutes les branches distantes


Comparaison de 2 ou n branches : 
```
git diff branch1..other-feature-branch
git diff branch1 branch2
```

Comparaison de fichiers entre 2 branches

git diff master new_branch ./diff_test.txt


## git checkout

git checkout <Nom de la branche>

Permet la navigation entre les branches

Faire un git checkout sur une branche mets à jour le repertoire et dit à Git de sauver tous les commit de la branche.


## Git Merge

https://www.atlassian.com/git/tutorials/using-branches/git-merge

1/ On se positionne sur la branche qui va recevoir le Merge

git checkout <receiving> => to switch to the receiving branch

Ex: git checkout master


2/ Fetch latest remote commits

Execute git fetch to pull the latest remote commits. 

Once the fetch is completed ensure the master branch has the latest updates by executing git pull


3/ Merging

git merge <branch name> => where <branch name> is the name of the branch that will be merged into the receiving branch.


## Merge 1

```
# Start a new feature
git checkout -b new-feature master
# Edit some files
git add <file>
git commit -m "Start a feature"
# Edit some files
git add <file>
git commit -m "Finish a feature"
# Merge in the new-feature branch
git checkout master
git merge new-feature
git branch -d new-feature
```



## Merge 2

Demarre une nouvelle branche et passe sur cette nouvelle branche
```
git checkout -b new-feature master
# Edit some files
git add <file>
git commit -m "Start a feature"
# Edit some files
git add <file>
git commit -m "Finish a feature"
# Develop the master branch
git checkout master
# Edit some files
git add <file>
git commit -m "Make some super-stable changes to master"
# Merge in the new-feature branch
git merge new-feature
git branch -d new-feature
```

## Les conflits

```
$ mkdir git-merge-test
$ cd git-merge-test
$ git init .
$ echo "this is some content to mess with" > merge.txt
$ git add merge.txt
$ git commit -am "we are commiting the inital content"

$ git checkout -b new_branch_to_merge_later
$ echo "totally different content to merge later" > merge.txt
$ git commit -am"edited the content of merge.txt to cause a conflict"
```
```
git checkout master
Switched to branch 'master'
echo "content to append" >> merge.txt
git commit -am "appended content to merge.txt"
[master 24fbe3c] appended content to merge.tx
1 file changed, 1 insertion(+)
```

```
$ git merge new_branch_to_merge_later
Auto-merging merge.txt
CONFLICT (content): Merge conflict in merge.txt
Automatic merge failed; fix conflicts and then commit the result.
```

```
$ git status
On branch master
You have unmerged paths.
(fix conflicts and run "git commit")
(use "git merge --abort" to abort the merge)

Unmerged paths:
(use "git add <file>..." to mark resolution)

both modified:   merge.txt
```


```
$ cat merge.txt
<<<<<<< HEAD
this is some content to mess with
content to append
=======
totally different content to merge later
>>>>>>> new_branch_to_merge_later
```

## Résoudre le conflit manuellement

1/ Je modifie merge.txt

2/ git add merge.txt

3/ git commit -m "merged and resolved the conflict in merge.txt"

[master 93ae695] merged and resolved the conflict in merge.txt


## Résoudre le conflit par outils

git status => aide a identifier les fichiers en conflits

git log --merge  => 1 liste de commits en conflit entre de branches de merge.

git diff => Trouve les differences entre fichiers en conflit.

git checkout => changer de branches ou annuler les modifs en cours sur un fichier.

git reset --mixed    => utilisé pour annuler des changements apportés au répertoire de travail et à la zone de staging.


## Outils quand les conflits arrivent durant un merge

git merge --abort     => quittera le processus de fusion et ramènera la branche à l'état avant le début de la fusion.

git reset    => réinitialiser les fichiers en conflit à un état fonctionnel connu




## Workflow de branche de fonctionnalité Git

https://www.atlassian.com/fr/git/tutorials/comparing-workflows/feature-branch-workflow

```
Push an existing folder
cd existing_folder
git init
git remote add origin git@gitlab.com:bxxx.xxxx.com/my-app.git
git add .
git commit -m "Initial commit"
git push -u origin master
```

Commencez par la branche master.


git checkout master

git fetch origin                 ==> récupère tout ajout qui a été poussé vers ce dépôt depuis que vous l’avez cloné ou la dernière fois que vous avez récupéré les ajouts

git reset --hard origin/master   ==>  jetez tous mes changements locaux, oubliez tout sur ma branche locale actuelle et mettez tous comme sur la branche master du depot distant.


Créer une nouvelle branche (et se positionner dessus)

git checkout -b post-model



Mettez à jour, ajoutez, commitez et pushez des changements

```
git status
git add <some-file>
git commit
```


Pushez la branche de fonctionnalité vers le dépôt distant

git push -u origin post-model



Pour obtenir du feedback sur la nouvelle branche de fonctionnalité, créez une pull request dans une solution de gestion de dépôt 



Résolvez du feedback

Désormais, les membres de l'équipe commentent et approuvent les commits pushés.

Résolvez les commentaires en local, commitez, puis faites un push des changements suggérés.

Vos mises à jour apparaissent dans la pull request.



Mergez votre pull request

Avant de faire un merge, vous devrez peut-être résoudre des conflits de merge si d'autres ont apporté des changements au dépôt. Une fois votre pull request approuvée et exempte de conflit, vous pouvez ajouter votre code à la branche principale (master).

Faites un merge à partir de la pull request

```
git merge post-model
git status
git push -u origin master
```






Utilisation des pull requests

1. 1 dév crée la fonctionnalité dans une branche dédiée (dépôt local)

2. Il fait un push de la branche vers un dépôt public

3. Il fait une pull request via le site web du dépôt public

4. L'équipe révise le code, en discute, puis le modifie.

Le mainteneur de projet fait un merge de la fonctionnalité dans le dépôt officiel, puis ferme la pull request.



## Cas oublie de création d'une nouvelle branche

git stash

git checkout -b feature/transfert-order

git stash pop

git add .

git commit -m "Add Transfert Order features"

git push origin feature/transfert-order
