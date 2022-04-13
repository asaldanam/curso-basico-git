# CURSO BÁSICO GIT

## Introducción

> Git es un software de control de versiones diseñado por Linus Torvalds, pensando en la eficiencia, la confiabilidad y compatibilidad del mantenimiento de versiones de aplicaciones cuando estas tienen un gran número de archivos de código fuente. Su propósito es llevar registro de los cambios en archivos de computadora incluyendo coordinar el trabajo que varias personas realizan sobre archivos compartidos en un repositorio de código. [https://es.wikipedia.org/wiki/Git](https://es.wikipedia.org/wiki/Git)

Git nos permite
* Mantener un histórico de los cambios que hemos hecho sobre el código fuente
* Trabajar simultaneamemente con otros compañeros sobre carpetas y ficheros con orden
* Subirlos a la nube y mantenerlos sincronizados con nuestro local
* Mantener diferentes versiones del código según su grado de avance en el proceso (desarrollo local, desarrollo entorno, preproducción, produción)
* Versionar el código fuente según los criterios de versionado de nuestro producto de software (1.0.0, 1.0.1, 1.1.0, 2.0.0....)
* Revertir a puntos del código fuente más estables

## Que és un repositorio

Un repositorio o *"repo"* es aquel proyecto (directorio, carpeta) que tiene git inicializado en él y desde el cual podemos ejecutar comandos de git y empezar a mantener su histórico.

**IMPORTANTE**: una cosa es el repositorio git de nuestro local y otra es el **repositorio online**. 
Algunos servicios como `Github`, `Gitlab` o `Bitbucket` nos permiten subir nuestros repositorio para que otras personas puedan bajarse una copia sincronizada de él (clonar).

## Crear un repositorio en local y subirlo a Github

Lo primero de todo, para poder utilizar Git tendremos checkear si lo tenemos instalado. Abrimos un terminal y comprobamos

```git --version```

En ubuntu viene preinstalado pero en Windows y posiblemente MacOs habrá que instalarlo [https://git-scm.com/book/en/v2/Getting-Started-Installing-Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git);

Una vez lo tengamos, debemos movermos a la carpeta donde tengamos nuestro proyecto de código.
Es importante tener claro qué carpeta es lo que identificamos como **raíz** del proyecto porque todos los ficheros
que estemos usando y no estén dentro de él quedaran fuera del control de versiones de git.

- Ejecutamos `git init`

Podremos observar que se ha creado una carpeta `.git`. Esta es la que usa Git para guardar el histórico. **ES MUY IMPORTANTE NO BORRARLA** salvo que queramos borrar nuestra copia del repositorio.

Y para confirmar que tenemos git en esta carpeta:

- `git status`

Este comando es muy importante, deberemos ejecutarlo constantemente para saber el estado de nuestro repo. Si todo va bien, nos dirá "On branch master, no commits yet"

### Crear el repositorio en la nube (origin)
Usaremos `Github` para esto:
[https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository)

Una vez creado, github ya nos indicará como vincularlo a nuestro repositorio local:

![](docs/github-2.png)

### Vincular nuestro repositorio local al repositorio de github
Con ambos repositorios ya creados, el siguiente importante paso es **VINCULARLOS**,
de esta forma el repositorio local quedará como esclavo (una copia o clon local) del de Github.

- Añadirmos el "origin": `git remote add origin URL_AL_ORIGIN`

En principio esto debería ser suficiente, pero hay un problema: al iniciar nuestro repo local,
por defecto la rama principal se llama `master` mientras que la rama principal de Github es `main`. 
Podemos solucionar esto cambiando el nombre de la rama en local:

- Renombrar la rama: `git branch -M main`
- Sincronizarla: `git push -u origin main`

## Clonar un repo ya existente
Cuando quieres crearte una copia de un repositorio en Github ya existente, podemos hacerlo con el siguiente comando:
`git clone URL_AL_ORIGIN`

Esto nos creará una carpeta con el nombre del directorio (como haría un mkdir)
pero con los ficheros actualizados de la rama principal del repositorio (main).
Esta carpeta ya tendrá su sincronización con git y podremos empezar a trabajar con git sobre ella directamente.

## El flujo de los cambios: de local al origin

Desde que hacemos cambios y guardamos un fichero hasta que este se encuentra en Github, tiene que pasar por las siguientes fases:

0. **Working area**: Aquí se añaden automáticamente todos los ficheros que git detecta que han sufrido cambios

1. **Staging area**: Aquí deberemos añadir aquellos ficheros que vayamos a subir con el próximo commit
2. **Commited**: Una vez tengamos listos nuestros cambios, debemos confirmarlos (commit) para que queden reflejados
como un hito dentro del histórico de cambios (un punto en la rama).

Podemos dividir nuestros cambios en diferentes commits tanto como queramos, 
pero hay que tener en cuenta que hasta aquí **SOLO ESTAMOS USANDO GIT EN LOCAL**. 
Si queremos que otros participantes del proyecto puedan bajarse nuestros cambios, **debemos subir nuestros commits**

3. **"Pusheado"**: Una vez hayamos "empujado" nuestros commits hacia el origin, 
al checkear con `git status` nos dirá "your branch is up to date with origin".
Con esto sabremos que efectivamente nuestros cambios están ya en Github

### 1. Añadir un fichero al Staging Area
Una vez hayamos hecho cambios en un fichero lo tendremos en el **Working area**. Para pasarlo al **Staging area**:

- Añade "mifichero.txt" `git add mifichero.txt`

Si quisieramos pasar todo lo que tenemos en el working area al staging, tenemos un atajo:

- Añadir todos los cambios: `git add .`

### 2. Confirmar (commitear) los cambios
Con esto confirmarmos todos los cambios que tenemos en el **Staging area** para crear un "commit"
 que formará ya parte del histórico de git en nuestro local.
Requiere un mensaje que deberá describir que tipo de cambios estamos haciendo:

- Commitear: `git commit -m "Mi mensaje de cambios"`

Este comando no funcionará si no tenemos al menos un fichero en el Staging area

### 3. Empujar (pushear) los cambios hacia el origin (Github)
Podemos realizar tantos commits seguidos como queramos, pero no estarán subidos hasta que los "pusheemos" hacia el origin:

- Empujar cambios: `git push`

Esto hará que nuestra rama `main` local (que estaba más "adelantada", es decir, tenía más commits)
 se sincronice con nuestra rama `origin/main` (su equivalente en Github)

### Operaciones de vuelta atrás y descarte
Cuidado con estas operaciones porque pueden llevar a perder trabajo.

- `git reset my-file.txt` nos moverá todos los ficheros que tengamos en el staging al working de nuevo.
- `git restore my-file.txt` nos permite descartar los cambios de un fichero que se encuentra en el working o en el staging area. 

Estos comandos los podemos usar con todos los ficheros, por ejemplo `git reset .` o `git restore .`;

### Ver todos los commits 
Podemos usar el comando git log con algunos parámetros para tener una visualización simplificada:
`git log --graph --oneline --decorate --all`

La visualización por terminal de las ramas suele ser compleja, pueden usarse herramientas de UI de git como la extensión de VSCode `Git Graph`
o la aplicación `GitKraken`

#### Revert
Esto nos permite deshacer los cambios de un commit. Revert hará un nuevo commit eliminando los cambios que hayamos hecho (por ejemplo, si en el commit introdujimos una línea de código, el revert lo que hará será borrarla).

- `git revert HEAD` hará un revert del último commit.
- `git revert HEAD~3` hará un revert con los cambios los tres últimos commits.

> Hay otras opciones, como "git reset --hard" que nos permitrá restablecer la rama en un punto concreto o "git drop" que nos permitirá borrar commits
> Estas opciones destruyen histórico y deben ser manejadas con cuidado cuando se tenga un uso de git más completo

## Sincronizar repo y traer (pull) cambios desde el origin
¿Y si otro desarrollador ha subido cambios? Nosotros no los veremos hasta que ejecutemos algunos comandos:

### Fetch 
`git fetch` nos permitirá bajarnos los cambios para que `git status` nos de información actualizada. 
Ojo!, fetch no nos hace disponer inmediatamente de los cambios que hay en la rama, pues disponer de ellos se realiza
en una operación aparte

Si tras el fetch se han realizado cambios en la rama por parte de otro desarrollador, veremos este mensaje en el status:
> Your branch is behind 'origin/main'

Esto quiere decir que en `origin/main` hay commits que no tienes en tu rama `main` de local

### Pull
`git pull` nos permite traer a local esos commits adicionales que tenemos en el origin.
Una vez lo ejecutemos podremos ver que nuestros ficheros han cambiado con lo más actual que hay en el origin

##  Ramas, merge, pull request y conflictos

Todo lo que hemos hecho hasta ahora sería utilizando la rama `main` y su rama en el origin,
pero no se considera una buena práctica utilizar la rama principal para realizar los cambios sobre el código.

Las convenciones y flujos usados en git habitualmente normalmente implican que la rama `main` estará **protegida**. 
Esto quiere decir que **no se podrá realizar push por métodos convencionales**.

Git nos permite abrir `branches` o ramas, es decir, líneas de commits alternativas a la rama principal. 
Estas ramas nos permiten crear commits sin afectar a la rama principal, aunque siempre el objetivo es integrarlas sobre ésta
(normalmente, cuando la tarea está concluída)

Normalmente estas ramas representan tareas concretas y/o cambios con un sentido en conjunto:

![](https://wac-cdn.atlassian.com/dam/jcr:7afd8460-b7bf-4c42-b997-4f5cf24f21e8/01%20Branch-2%20kopiera.png?cdnVersion=309)

### Crear una rama
Podemos usar el comando `git checkout -b my-branch` para crear una nueva rama `my-branch` en nuestro repo local.
Esta rama partirá desde donde la hayamos creado `main` en este caso. Es importante por tanto si queremos estar actualizados
con los últimos cambios que existan en la rama `main` que primero hagamos un fetch y un pull de esa rama antes de crear la nuestra.

> Una manera de cambiar el punto de partida de una rama a posteriori es con el comando "git rebase", 
> aunque esto es algo más avanzado y no vamos a verlo aquí [git rebase](https://www.atlassian.com/es/git/tutorials/rewriting-history/git-rebase)

### Ver todas las ramas que hay en local y el origin
Podemos usar para esto `git branch -a`. Si queremos información lo más actualizada posible del origin, 
es conveniente primero ejecutar un fetch para tener los últimos cambios que haya sobre el origin.

### Cambiar nuestra rama de trabajo
Sólo podemos estar trabajando en una única rama al mismo tiempo en nuestro local.
Con el comando `git checkout -b main` podremos movernos de nuevo a `main` o a cualquier otra rama que queramos.
Hay que tener en cuenta que no podremos movernos si tenemos cambios en el working/staging area, así que debemos primero
descartarlos o commitearlos antes de tratar de movernos

> Una opción para guardar los cambios temporalmente es los stash. Los stash son commit temporales para guardar cambios en ficheros temporalmente
> Como no es algo imprescindible de conocer para el manejo básico no lo veremos [git stash](https://www.atlassian.com/es/git/tutorials/saving-changes/git-stash).

### Integrar cambios de una rama en otra
El objetivo último de todas las ramas que no sean la principal es terminar siendo integradas en esta, a esto se le llama **merge**

Esto llevará todos los cambios que hayamos hechos en los commits de la rama alternativa 
a un nuevo commit en la rama de destino (normalmente `main`). 

![](https://wac-cdn.atlassian.com/dam/jcr:c6db91c1-1343-4d45-8c93-bdba910b9506/02%20Branch-1%20kopiera.png?cdnVersion=309)

Hay varias formas de provocar un merge. La más sencilla y utilizando comandos ya vistos es con `git pull`:

0. Cerciorarnos de que la rama que queremos integrar la tenemos actualizada (fetch, pull, status)

1. Situarnos con `git checkout` en la rama de destino del merge, por ejemplo: `main`;
2. Realizar un pull, pero indicando la rama que queremos integrar: `git pull my-branch`.
3. `git push` para que el merge quede subido al origin

Esto habrá traído los cambios de `my-branch` en `main`.

Mergear `main` 

### Merge contra ramas protegidas: Pull request
En los proyectos es habitual que la rama principal `main` o `master` esté **protegida**.
Esto quiere decir que no podrán mergearse otras ramas sobre la principal por el método convencional 
porque requiere un proceso de validación previo manual (una o varias personas debe aprobar los cambios) 
o automático (debe pasar primero unos tests)

Esta forma de trabajo nos permite evitar introducir errores en la rama principal.

De esta forma, el método de merge "estándar" queda normalmente relegado a traer cambios desde la rama principal
`main` a nuestra rama de trabajo cuando `main` ha sufrido alguna actualización y queremos tener los últimos cambios.

Para solicitar estos cambios se suele utilizar el método de `Pull request` o `Merge request`. Esto se suele realizar
desde la interfaz del servicio de repos que usemos (Github, Gitlab, Bitbucket) y puede variar, pero en general suele 
ser el siguiente:

1. Pulsar el botón "Pull request" en la web del respositorio
2. Indicar qué rama queremos mergear en qué otra rama (A -> B)
3. Revisar cómo el código cambia en la rama principal e introducir comentarios
4. Que la persona con permisos le de al botón "Merge" para mergearlo

### Conflictos
Puede darse el caso de que desde que abrimos en nuestra rama hayamos hecho cambios sobre líneas de código 
que hayan sufrido cambios por parte de otro desarrollador. 

Lo más habitual es que esta situación se dé de la siguiente forma:

1. Un compañero abre una rama desde `main` y nosotros abrimos otra.
2. Tanto como el compañero como nosotros efectuamos cambios sobre las mismas líneas de código
3. El compañero abre una Pull Request y se mergea mientras nosotros seguimos trabajando en cambios
4. Al actualizar nuestra rama (hacer un merge de main -> nuestra rama) git nos indica que hay líneas con conflictos.

Git nos abrirá el editor de texto que tengamos por defecto (normalmente 'Vim') y nos envolverá las líneas de código
con unos guiones, tanto la nuestra como la de nuestro compañero. Podemos editar el código desde aquí para dejarlo en el estado
que queramos

En estos casos lo mejor es hablar con el compañero y tratar de resolverlos entre los dos, pues la resolución debe hacerse manualmente.

## Convenciones y "git flows".
WIP





