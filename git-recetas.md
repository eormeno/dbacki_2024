# Snippets de Git
## Clone
```bash
$ git clone [--branch branch_name] https://github.com/[...].git [folder]
```
## See current branch
```bash
$ git rev-parse --abbrev-ref HEAD
```
## List branches
```bash
$ git fetch --all  # to ensure all branches are updated
$ git branch -r -v # all remote branches with last commit message
$ git branch -a    # all local and remote branches
```
## Change current branch
```bash
$ git checkout [branch_name]
```
## Create branch
```bash
$ git checkout -b new_branch
$ git push -u origin <branch>
```
## Merge to master
```bash
$ git checkout master
$ git pull               # to update the latest  master state
$ git merge develop      # to merge branch to master
$ git push origin master # push current HEAD to master
```
## Delete branch
```bash
$ git branch -d -r localBranchName   # delete branch locally (the -r option should be checked)
$ git push origin --delete remoteBranchName # deletes the branch remotely
```
## Undelete local branch
```bash
$ git branch -D branch_name
Deleted branch branch_name (was 130d7ba).
$ git branch branch_name 130d7ba
```
Also, if the SHA1 is unknown, you can use `git reflog` to find the SHA1 of the last commit of the branch. From that point, you can recreate a branch using
```bash
$ git branch branchName <sha1>
```
## List the changes of a branch
```bash
git log ..otherbranch
```
list of changes that will be merged into the current branch.
```bash
git diff ...otherbranch
```
diff from common ancestor (merge base) to the head of what will be merged. Note the three dots, which have a special meaning compared to two dots (see below).
```bash
gitk ...otherbranch
```
Graphical representation of the branches since they were merged last time.
## Two ways to safer merge
### Strategy 1: The safe way – merge off a temporary branch:
```bash
$ git checkout mybranch
$ git checkout -b mynew-temporary-branch
$ git merge some-other-branch
```
That way you can simply throw away the temporary branch if you just want to see what the conflicts are. You don't need to bother "aborting" the merge, and you can go back to your work -- simply checkout 'mybranch' again and you won't have any merged code or merge conflicts in your branch.
### Strategy 2: When you definitely want to merge, but only if there aren't conflicts
```bash
$ git checkout mybranch
$ git merge some-other-branch
```
If git reports conflicts (and ONLY IF THERE ARE conflicts) you can then do:
```bash
$ git merge --abort
```
If the merge is successful, you cannot abort it (only reset).

If you're not ready to merge, use the safer way above.
## Remove last pushed commit ([from here](https://stackoverflow.com/questions/448919/how-can-i-remove-a-commit-on-github))

```bash
$ git reset HEAD^ --hard
$ git push origin -f
```
## Mostrar el último mensaje de commit
```bash
$ git log -1 --pretty=%B
```
## Mostrar el nombre del branch actual
```bash
$ git rev-parse --abbrev-ref HEAD
```
## Listar las modificaciones (diferencias) para hacer commit
```bash
$ git diff
```
Print out differences between your working directory and the index.
```bash
$ git diff --cached
```
Print out differences between the index and HEAD (current commit).
```bash
$ git diff HEAD
```
Print out differences between your working directory and the HEAD.
```bash
$ git diff --name-only
```
Show only names of changed files.
```bash
$ git diff --name-status
```
## Hacer commit y push
Una vez identificado haciendo:
```bash
$ git config user.email "nombre@email.com"
$ git config user.name "Nombre"
```
Para que la configuración sea global, agregar `--global` luego de `config`.
```bash
$ git commit -am "el mensaje del commit"
```
Las opciones:
* a: significa que agrega al commit todos los cambios que encuentre
* m: especifica que habrá un mensaje
Por último:
```bash
$ git push
```
## Deshacer todos los cambios locales No commited ([from](https://stackoverflow.com/questions/14075581/git-undo-all-uncommitted-or-unsaved-changes))

```bash
$ git reset
$ git checkout .
```
## Script de Linux ejecutable en Git
```bash
$ git update-index --chmod=+x ruta/al/script.sh
```