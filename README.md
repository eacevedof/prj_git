# prj_git  (eduardoaf.com)
## sandbox pruebas git

### PRUEBA `git reset $(git commit-tree HEAD^{tree} -m "nuevo commit fusionado dentro de rama")`
- de esta form al fusionar los commits en la rama se pierde el historial de commits desde donde se sacó la copia. En este caso *main*
en otras palabras **main** deja de ser la **base** de **my-branch**
- habria que hacer un rebase de **my-branch** contra **main** `git rebase -i main` de lo contrario indicará `fatal: refusing to merge unrelated histories`
si se desea integrar con **main**

### PRUEBA `rebase and squash multiple commits in my-branch`
- dentro de **my-branch**
- `git rebase -i main`
- corregimos conflictos
- marcamos los commits con `squash`
- hacemos `git push --force-with-lease`

### PRUEBA `merge with squash`
- de mi rama **main** cuyos ultimos commits son:
```git
commit 1b57343726b80b60db15082c79c317ed52ef6094 (HEAD -> main, origin/main, origin/HEAD)
Author: eaf@pc <eaf@eaf.com>
Date:   Sat Jan 7 13:52:41 2023 +0100
    reset antes de probar git merge with squash

commit b0e357d58a7078a1e5d6faf5552e340cef10d3f1
Author: eaf@pc <eaf@eaf.com>
Date:   Sat Jan 7 13:52:32 2023 +0100
    reset antes de probar git merge with squash
...
```
- creo una rama llamada: `feature/test-merge-squash` y hago checkout
- hago commit y push de estos **commits parciales** (como pruebas, backups, etc):
```
commit fa7adc315b547cffcc08977ebaba80f597535b16 (HEAD -> feature/test-merge-squash, origin/feature/test-merge-squash)
Author: eaf@pc <eaf@eaf.com>
Date:   Sat Jan 7 13:55:05 2023 +0100
commit echo c

commit 8232b2e212e8ed4b1fedc6a6c4e83c9204ed982f
Author: eaf@pc <eaf@eaf.com>
Date:   Sat Jan 7 13:54:52 2023 +0100
commit echo b

commit 5c235c68d13bcd7c17b1bf98c1e2435521b81ecc
Author: eaf@pc <eaf@eaf.com>
Date:   Sat Jan 7 13:54:39 2023 +0100
commit echo b

commit 940eaf72049fce08abc7df3dcb85bbcd5725c5da
Author: eaf@pc <eaf@eaf.com>
Date:   Sat Jan 7 13:54:27 2023 +0100
commit echo a
```
- cuando tengo mi **feature o fix** terminado el siguiente paso es **integrarlo en main**
- para esto, me posiciono en la rama **main** y ejecuto
  - `git merge --squash feature/test-merge-squash`
    - generará un único commit con todos los cambios de la rama `test-merge-squash`
  - `git commit -m "feature/test-merge-squash integrada en main con squash"`  
    - al commit anterior hay que configurarle un mensaje que es como aparecera al ser integrado nuestro único commit
  - `git push`
    - terminamos de integrar en main
- ahora main quedaría así:
```
commit b8c87d5d36722f078f7d0f7cfc8476a21fb5c86f (HEAD -> main, origin/main, origin/HEAD)
Author: eaf@pc <eaf@eaf.com>
Date:   Sat Jan 7 14:16:51 2023 +0100
    feature/test-merge-squash integrada en main con squash

commit 1b57343726b80b60db15082c79c317ed52ef6094
Author: eaf@pc <eaf@eaf.com>
Date:   Sat Jan 7 13:52:41 2023 +0100
    reset antes de probar git merge with squash
...
```
- con esto conseguimos que nuestra rama principal solo tenga un commit relevante con los cambios del feature/bug finalizado


### stash
- git stash
  - deja unos cambios en stand-by
- git stash apply
  - de la lista de cambios en stand-by recupera el último y lo aplica a la rama donde nos encontramos pero sin añadirlo al stage para un futuro commit


### pull request
- git rebase --intaractive #hago un commit unito
- git push --force-with-lease #fuerza escritura en la rama si son todos los commits tuyos
