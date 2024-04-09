# Manejo de ramas en Git

## Introducción a las ramas

En el laboratorio 2 estuvimos trabajando con la introducción al control de versiones, desde aquí vimos el diagrama y los elementos principales que componen la herramienta de Git. A manera de repaso los veremos nuevamentes pues serán fundamentales en el siguiente paso.

```
┌──────────────┐         ┌──────────────┐
│ local        │ push ─> │ remote       │
│ repo         │ <- pull │ repo         │
└──────────────┘         └──────────────┘
check │  ↑↓ commit / reset
out   │ ┌──────────────┐
      │ │ staging area │
      │ └──────────────┘
      ▽  ↑↓ add / restore
┌──────────────┐
│ working tree │
│ .            │
│ ├── go.mod   │
│ └── main.go  │
└──────────────┘

```
- El working tree es el pedado de proyecto en cualquier momento (usualmente es el momento actual). Cuando agregas o editas código, modificas el working tree.
- El stagin area es donde se colocan los cambios del working tree antes de hacerlos permanentes.
- Un repository es la colección de cambios permanentes (commits) realizados a través de la historia del proyecto. Típicamente, existe un repositorio remotor (Github,Gitlab,Bitbucket,etc.) y muchos repositorios locales, uno para cada desarrollador involucrado en el proyecto al menos.

Ahora bien, si esta es la forma de cliente-servidor que tenemos para visualizar en donde se encuentran nuestros archivos, es importante mencionar que el siguiente paso para trabajar con control de versiones es a través del manejo de branches o ramas.

Desde este punto toma la siguiente analogía, eres dueño del tiempo y el espacio en tu proyecto, hasta ahora hemos trabajado con el espacio, es hora de incorporar el tiempo. Pero recuerda el manejo de estas 2 dimensiones debe realizarse con responsabilidad pues en caso contrario **terribles cosas podrán pasar**.

Antes de entrar de lleno recuerda los 2 valores fundamentales que te transmití para el manejo de versiones que son:

1. Disciplina - Aquí viene la responsabilidad de la persona en asegurarse que se encuentrar con los cambios más recientes y no forzar a agregar ningún cambio con comandos extraños que puedan llegar a afectar la línea del tiempo del proyecto o los archivos del mismo repositorio de manera remota.
2. Comunicación - Cuando algo extrano suceda siempre es mejor invocar al equipo para que entre todos apoyen a resolver los conflictos, dejarlo en manos de una sola persona puede llevar a perder archivos importantes.

>Nota: Recuerda que puedes recuperar un archivo que haya sido commiteado al repositorio remoto siempre y cuando no borren los apuntadores de commits que vienen, como regla específica si ya se subió mal, mejor arreglarlo en commits posteriores, si el error compromete seguridad por regla mejor crear un nuevo repositorio y un nuevo historial de commits.

Ahora bien, las ramas te permiten desarrollar características, funcionalidades o features, corregir errores, o experimentar con seguridad las ideas nuevas en un área contenida de tu repositorio.

Siempre puedes crear una rama a partir de una rama existente. Habitualmente, puedes crear una rama nueva desde la predeterminada de tu repositorio. Podrás entonces trabajar en esta rama nueva aislado de los cambios que otras personas hacen al repositorio. Ala rama que crear para construir una característica se le conoce como rama de característica o rama de tema.

Dependiendo de tu equipo puede o no ser común trabajar con los comandos de creación de ramas. Aquí veremos la forma más tradicional tomando como base que tenemos un repositorio vacío.

![lab_7](/Tutorials/Lab7Branches/imgs/001.jpg)

Para ir ubicandonos, vamos a ver que el branch default para GitHub es main pero si nos acercamos al menú que aparece podremos desplegar todos los branches que tenga nuestro repositorio. Nuevamente como apenas vamos comenzando solo existe main.

![lab_7](/Tutorials/Lab7Branches/imgs/002.jpg)

Ahora vamos a clonar nuestro repositorio en nuestra computadora local, elige una carpeta donde estará almacenado tu repositorio dentro de tu máquina y ejecuta el comando git clone seguido de la url del repositorio, recuerda que puedes usar https o ssh, la recomendación es que utilices ssh por facilidad y seguridad.

![lab_7](/Tutorials/Lab7Branches/imgs/003.jpg)

Dentro de la consola entonces tendrías el siguiente comando

```
git clone {{url https o ssh del repositorio}}
```

El resultado de tu terminal será algo como lo siguiente:

```
Cloning into 'git-learning-branches'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
```

> Nota: Si utilizas la interfaz gráfica de git, también es válido, y podrás realizar el mismo procedimiento que veremos a nivel de comandos, solo necesitas encontrar los botones que realicen las acciones que estamos realizando.

Como hemos clonado la carpeta del repositorio debemos entrar a ella y nos encontraremos de lleno en nuestro repositorio.

![lab_7](/Tutorials/Lab7Branches/imgs/004.jpg)

En mi caso se creo un archivo **README.md** al momento de crear el repositorio, si no lo tienes no te preocupes te invito a que lo empieces a crear en este momento.

El contenido de mi archivo es el siguiente

```
# git-learning-branches
Este repositorios es para demostrar el uso de los branches y el gitflow simplificado
```

Que para efectos prácticos es el nombre y la descripción de mi repositorio.

Hasta este punto ya podemos empezar a trabajar y algo que vamos a notar con la descripción que tengo es que me equivoque al escribir repositorio y escribí **repositorios**, aprovechemos este momento para empezar a trabajar con los branches.

Idealmente veremos que existen 2 tipos de branches, los de control y los de trabajo.

- Control - Este tipo de branches no deben tener commits directos de los miembros del equipo, quizás cuando se está configurando se pueden tener algunos commits justo a medio de configuración, pero la tendencia es evitar escribir en ellos. Esto ayuda a mantener la calidad de los productos de trabajo, ejemplos de branches de este tipo serían main, develop, release.
- Trabajo - Este tipo de branches contienen el trabajo y por tanto los commits de los miembros del equipo, es muy común que aquí se genere un historial completo del trabajo de la persona, se recomienda ampliamente realizar commits de manera continua para evitar perder cambios y también se recomienda siempre estar sincronizándolos con las últimas versiones de main o develop.

Regresando a nuestro repositorio vamos a modificar el archivo entonces con lo siguiente:

```
# git-learning-branches
Este repositorio es para demostrar el uso de los branches y el gitflow simplificado.

```

> No olvides al final agregar el salto del línea.

Ahora, si guardamos el archivo y revisamos en nuestra terminal podemos escribir lo siguiente:

```
git status
```

El resultado debería aparecer como lo siguiente:

Por un lado VSCode nos detecta que ya existen cambios dentro del archivo.

![lab_7](/Tutorials/Lab7Branches/imgs/005.jpg)

```
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

Recuerda que de momento nuestros cambios están en el working tree, primero necesitamos agregarlos al staging area y después debemos firmarlo con un commit.

Pero **alerta** veremos que al hacer el **git status**, nos regresa en que branch nos encontramos actualmente.

```
On branch main
Your branch is up to date with 'origin/main'.
```

Esto es justo lo que no queremos que suceda pues main debería ser intocables, por lo tanto debemos crear un nuevo branch que será **develop**, si recuerdas te dije que develop tampoco debería de incluir commits, pero como es la configuración de la rama para este primer caso será permitido.

Entonces para crear un nuevo bran realizaremos lo siguiente

```
git checkout -b "develop"
```

De manera automática nuestro repositorio local crea el nuevo branch y nos da el siguiente resultado.

```
Switched to a new branch 'develop'
```

Éxito, si volvemos a ejecutar

```
git status
```

El resultado entonces será:

```
On branch develop
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md
  no changes added to commit (use "git add" and/or "git commit -a")
```

Observa que nuestro archivo **README.md** sigue en el working tree, pero ahora el resultado nos dice que estamos en el branch develop **On branch develop**.

Entonces ahora sí podemos agregar el archivo y hacer el commit de la siguiente manera:

```
git add -A
git commit -m "fix: Se corrige el typo de la descripción del README.md"
```

Algo que también es importante mencionar, es que observa que mi commit no tiene una gran cantidad de trabajo, un buen programador hace iteraciones cortas en su trabajo para evitar perder sus cambios, esta es la diferencia de pasar horas sentado tratando de resolver algo y poder levantarte cada x tiempo de la silla para obtener aire.

El resultado de nuestro comandos será entonces:

```
[develop 5a06f6d] fix: Se corrige el typo de la descripción del README.md
 1 file changed, 1 insertion(+), 1 deletion(-)
```

 Y si volvemos a ejecutar un

```
 git status
```

El resultado será entonces

```
On branch develop
nothing to commit, working tree clean
```

Ahora recuerda el diagrama del funcionamiento de git y preguntante. ¿En qué parte se encuentra nuestro archivo?

Ya pasamos del **working tree** al **staging area** usando el **git add**, de ahí firmamos el cambio del **staging area** con un commit hacia nuestro **repositorio local.**

Por tanto la respuesta es que nuestro cambio se encuentra en el **repositorio local**, hasta este punto debemos seguir teniendo cuidado por que mientras no lo subamos al **repositorio remoto**, corremos el riesgo de perder nuestro cambios. Ejemplo: si pierdes tu computadora, si tu disco duro falla, etc. Son razones por las cuales podrías perder tus cambios aunque ya hayan sido firmados.

Por tanto vamos a subir el archivo a la nueva rama, normalmente cuando la rama existe debemos realizar un:

```
git push origin
```

Pero si lo hacemos veremos el siguiente resultado:

```
fatal: The current branch develop has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin develop

To have this happen automatically for branches without a tracking
upstream, see 'push.autoSetupRemote' in 'git help config'.
```

Esto sucede por que al haber creado el branch de **develop** lo tenemos creado solamente en nuestro repositorio local, es más si nuevamente nos vamos a github, y desplegamos la lista de branches, veremos que este aún no existe.

![lab_7](/Tutorials/Lab7Branches/imgs/002.jpg)

Esto es normal y justo lo que queremos es crear de manera remota el nuevo branch, por lo que si leemos el resultado de la terminal verás que nos da el comando que necesitamos para subir nuestros cambios y crear el nuevo branch, en este caso sería:

```
git push --set-upstream origin develop
```
Este comando se ejecuta solo 1 vez al crear un nuevo branch ya que de aquí en delante puedes usar **git push origin** sin problema. El resultado entonces será:

```
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 16 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 323 bytes | 323.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
remote:
remote: Create a pull request for 'develop' on GitHub by visiting:
remote:      https://github.com/black4ninja/git-learning-branches/pull/new/develop
remote:
To github.com:black4ninja/git-learning-branches.git
 * [new branch]      develop -> develop
branch 'develop' set up to track 'origin/develop'.
```
Y sí actualizamos la página de GitHub veremos que no solo ya nos aparece **develop**, sino que además GitHub nos da una alerta que un nuevo branch fue creado.

![lab_7](/Tutorials/Lab7Branches/imgs/006.jpg)

Aquí podemos podemos examinar varias cosas.

Si cargo el branch de main, veré que los cambios siguen como eran inicialmente.

![lab_7](/Tutorials/Lab7Branches/imgs/007.jpg)

Pero si selecciono el branch de develop veremos que tiene el contenido actualizado de nuestro archivo **README.md**. 

![lab_7](/Tutorials/Lab7Branches/imgs/008.jpg)


Aquí esta todo el poder del control de versiones, pues pensemos un minuto lo que esto significa, **main** no se ha modificado, eso implica que si tuviera un sistema está en su versión más estable, quizás **develop**, necesite ser probado, pero eso no va a impedir que el equipo pueda bajar los cambios necesarios en sus máquinas o automatizar un set de pruebas automáticas sin comprometer la version final a los usuarios finales.

Con esto ya tienes el conocimiento de trabajar con ramas en tus repositorios, ahora lo que veremos en los siguientes pasos es como estandarizar nuestra forma de trabajo utilizando todo lo que hemos aprendido hasta ahora.

## Cambio entre ramas y obtener la última versión
Como ya mencionamos es importante que al trabajar con ramas el equipo se asegure que al cambiar entre una y otra se baje la última versión remota, pues el **repositorio remoto** es la última fuente de la verdad, si tratamos de forzar cambios, entonces seguramente romperemos la versión para todo el equipo. En términos de daños cuantificables, siempre es mejor que solo 1 persona tenga conflictos a todo el equipo y más si eso compromete el repositorio remoto.

Vamos a regresar a nuestra terminal y vamos a empezar a cambiar entre branches.

En ocasiones podemos olvidar el branch en el que nos encontramos y esto es perfectamente normal, previamente vimos como con **git status** podemos ver el branch en el que nos encontramos.

Pero git tiene un comando que nos permite ver no solo en que branch estamos sino también que branches tenemos actualmente. Antes de hacer cualquier cosa es recomendado hacer un **git pull origin** para obtener los últimos cambios incluidos nuevos branches remotos que hayan creado nuestros compañeros de trabajo.

Por tanto los comandos a ejecutar serían:

```
git pull origin
git branches
```

En este caso particular **git pull origin** no va a realizar ningún cambio pues nuestro **repositorio local** es igual al **repositorio remoto**. Pero, **git branches** nos mostrará la lista de ramas disponibles en nuestro repositorio local que en este caso coinciden con el remoto.

```
* develop
  main
```

También veremos que se nos marca en un asterisco, el branch donde nos encontramos actualmente, por lo que si necesitamos pasar de develop a main basta con ejecutar el siguiente comando:

```
git checkout main
```

El resultado:

```
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
```

Si recuerdas, cuando ejecutamos el comando que creaba la rama de **develop** ejecutamos **git checkout -b "develop"**, la bandera **-b** sirve para crear una nueva rama seguido del nombre de la misma, cuando utilizamos **git checkout** directamente es por que ya tenemos la rama que vamos a utilizar.

Aunque nos dice que tenemos la última versión no está por demás tener la buena práctica:

```
git pull origin
git branches
```

El resultado final:

```
  develop
* main
```

Y ahora veremos que nos marca el asterisco que estamos en **main** y nos aseguramos con el pull de tener los últimos cambios siempre.

## Gitflow Simplificado

Dentro del mundo del control de versiones existen varias estrategias recomendadas para trabajar con branches en un repositorio. La que veremos en este laboratorio se conoce como GitFlow y de está existen a su vez varias versiones, con tal de seguir un estándar pero tratando de no complicarnos demasiado estaremos aplicando el GitFlow simplificado.

Este GitFlow simplificado es un mapa de como debemos trabajar en un repositorio. Esto nos da un estándar de como manejar los branches, ejecutar commits, etc.

![lab_7](/Tutorials/Lab7Branches/imgs/009.png)

Nuevamente y haciendo incapié, recuerda que las ramas de master o main y develop no se tocan para trabajo continuo, solo para configuraciones o en su defecto no se tocan.

Las configuraciones de GitHub te permiten bloquear estas ramas de recibir commits, sin embargo pueden limitarse al plan que tienes, por lo que más que la automatización establece con tu equipo que no deben realizarse commits a estas ramas.

Por lo mismo vamos a realizar algo similar a lo que hicimos previamente en develop pero vamos a simular el trabajo continuo de un día a día.

Vamos a pensar que nos toca trabajar en el **README.md** y que además nos solicitan un archivo **CONTRIBUTING.md** en donde específiquemos la estrategia para desarrollar commits y branches dentro del equipo. Además nos solicitan que todo el desarrollo se realice en Inglés.

Antes que nada vamos a crear un nuevo branch de **Trabajo**.

```
git checkout -b "feature/contributing-standard"
```
```
Switched to a new branch 'feature/contributing-standard'
```

Y ahora sin haber realizado ningún cambio vamos a crear el nuevo bran en el repositorio remoto. Si no recuerdas específicamente el comando recuerda que puedes hacer un **git push origin** y te marcará error que el branch actual no existe en remoto.

```
 git push --set-upstream origin feature/contributing-standard
```
```
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
remote:
remote: Create a pull request for 'feature/contributing-standard' on GitHub by visiting:
remote:      https://github.com/black4ninja/git-learning-branches/pull/new/feature/contributing-standard
remote:
To github.com:black4ninja/git-learning-branches.git
 * [new branch]      feature/contributing-standard -> feature/contributing-standard
branch 'feature/contributing-standard' set up to track 'origin/feature/contributing-standard'.
```

Con esto hemos visto que no se necesitan tener cambios para crear ramas en el proyecto, procura como buena práctica crear tus ramas de trabajo antes de empezar a trabajar de manera normal.

![lab_7](/Tutorials/Lab7Branches/imgs/010.jpg)

Pero, ohh no  hemos cometido un error fatal, si te diste cuenta al momento de crear el branch estabamos en el branch de **main** y no de develop, por lo que al crear nuestra nueva rama el contenido de **README.md** es el original y no contiene el cambio que realizamos.

Caos, muerte y destrucción, pero es justo por este tipo de detalles que se recomienda hacer iteraciones cortas, ya que el error lo voy a tener en 2 líneas, pero imagina que es 1 mes de trabajo entre versiones, entonces ahí si puedo tener un problema.

Aquí tengo 2 opciones, si el cambio entre versiones es muy amplio, lo mejor sería borrar mi branch remoto y volver a empezar, pero hay algo que podemos hacer para arreglarlo.

Asegurandonos que estamos en nuestro nuevo branch,dentro de nuestra consola vamos a realizar un:

```
git pull origin develop
```

El resultado es que se integrarán todos los commits de **develop** que no existan en **main**:

```
From github.com:black4ninja/git-learning-branches
 * branch            develop    -> FETCH_HEAD
Updating 808115e..5a06f6d
Fast-forward
 README.md | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

Y por ahora hemos evitado la crisis. Pero de mi parte si los cambios hubieran sido mayores y al momento de ejecutar la versión local algo falla, lo más probable y más confiable es volver a crear otro branch asegurándome que ahora si empiece por **develop**.

Algo que es vital entender es que **sí debemos tener cuidado al ejecutar el git checkout -b**, pues va a depender el branch en el que estemos es desde donde se creará la rama.

Por eso es importante que antes de crear una nueva rama nos coloquemos desde donde queremos crear la nueva versión.

Ya que tenemos nuestra rama creada, podemos ahora sí trabajar en ella como lo haríamos en el día a día.

Primero vamos a modificar el título del **README.md** por algo más legible. Cambia la línea 1 por lo siguiente:

```
# My Git Learning of branches
```

Vamos a considerar que  este cambio representa un espacio de horas de trabajo, ahora realizamos nuestro commit como normalmente lo haríamos:

```
git add -A
git commit -m "feature: Changed title README.md and started migration to English language."
git push origin
```

![lab_7](/Tutorials/Lab7Branches/imgs/011.jpg)

Nuestra versión fue actualizada correctamente.

Sigamos ahora traduciendo el texto de la descripción para nuestro **README.md** pensando que es otro espacio de horas de trabajo.

```
This repository is to demonstrate the use of branches and simplified gitflow.
```

Nuevamente finalizamos haciendo commit de nuestros cambios.

```
git add -A
git commit -m "feature: Changed description README.md"
git push origin
```

![lab_7](/Tutorials/Lab7Branches/imgs/012.jpg)

Perfecto, ahora nos hicieron la solicitud de agregar un archivo **CONTRIBUTING.md** al repositorio donde vamos a establecer el estándar para este repositorio para la generación de commits y branches.

Este archivo es un poco extenso pero te lo dejo [aquí](/Tutorials/Lab7Branches/CONTRIBUTING.md), utilizalo como base para el repositorio de tu equipo y futuros proyectos.

![lab_7](/Tutorials/Lab7Branches/imgs/013.jpg)

Ya que tengo mi archivo **CONTRIBUTING.md**, nuevamente vamos a llamarlo un día, y hacemos el commit correspondiente.

```
git add -A
git commit -m "feature: Added CONTRIBUTING.md with standard specification on how team must work with branches, commits and additional stuff."
git push origin
```

![lab_7](/Tutorials/Lab7Branches/imgs/014.jpg)

Finalmente nos hace falta agregar una cceso rápido al archivo **CONTRIBUTING.md** desde nuestro archivo **README.md**. Por lo que actualizaremos el archivo a lo siguiente:

```
# My Git Learning of branches
This repository is to demonstrate the use of branches and simplified gitflow.

--- 

## Contributing

Please see the [Contributing Guide](CONTRIBUTING.md)

```

No olvides el salto de línea al final.

Por último vamos a hacer commits de los últimos cambios a nuestra actualización.

```
git add -A
git commit -m "feature: Linked CONTRIBUTING.md file with README.md."
git push origin
```

Excelente, hemos finalizado nuestra iteración de trabajo.

![lab_7](/Tutorials/Lab7Branches/imgs/015.jpg)

Dentro de GitHub también podremos ver que hemos realizado 6 commits desde este branch, si seleccionamos esta parte nos llevará a la siguiente vista.

![lab_7](/Tutorials/Lab7Branches/imgs/016.jpg)

Aquí veremos literalmente todos los commits que hemos realizado durante el tiempo de vida a este branch.

Si nos cambiamos a otro branch, por ejemplo **develop** observa que no existen estos cambios aún.

![lab_7](/Tutorials/Lab7Branches/imgs/017.jpg)

Con esto ya tenemos todo listo para empezar con el último paso para poder trabajar con nuestro repositorios y equipos y es unificar el trabajo.

## Hacer merge de las ramas con Pull Request

Hasta el momento hemos visto comandos y estrategias diferentes para trabajar con nuestros archivos, realizar nuestros commits y crear ramas, pero ahora viene la parte más engorrosa de todo el proceso, unificar el tiempo y el espacio.

Si bien existen los comandos para que puedas hacer merge entre las ramas directamente, hoy en día se considera una mala práctica hacerlo. Si bien entendemos que vamos empezando en este mundo de control de versiones, es mejor que empieces usando los estándares de la comunidad de desarrollo para que puedas ser parte de dicha comunidad.

Otro punto a favor de por que no vemos los commits de merge branches desde la línea de comando es que existe un riesgo real de que unifiquemos cosas que no son necesarias. Ya lo viste en el caso donde me equivoque en el branch, pero lo peor que puede pasar es que mi versión local se vuelva corrupta y tenga que clonar mi repositorio local nuevamente, pero en el caso de hacer un merge con comandos entre ramas puede llevar a que corromper el repositorio remoto y por tanto dar un gran dolor de cabeza al equipo.

Por ello vamos a utilizar una técnica que va a evitar que realicemos errores o al menos nos va a hacer más difícil poder hacerlos, y es una estrategia de Pull Request.

Dentro del archivo **CONTRIBUTING.md** viene el link oficial de GitHub, de como crear un pull request, pero esto no es exclusivo de GitHub, todas las plataformas que usan control de versiones tienen incorporada la herramienta de Pull Request para unificar cambios.

Un Pull Request no es nada más que realizar un aviso al repositorio de que se intentan realizar unión de ramas, esto permite al equipo revisarle a la persona que es lo que se intenta realizar y en cuestión de código revisar si se siguen las buenas prácticas de programación del equipo.

Algo importante es que un Pull Request engloba una rama en particular que queremos combinar con otra específica, y por tanto una rama es un conjunto de commits que incluyen varios cambios realizados.

Para nuestro ejemplo vamos a querer realizar un pull request desde nuestro branch **feature/contributing-standard** hacia develop.

Para evitar rollos entre comandos de GitHub empezaremos a ver los Pull Request directamente desde la página, pero es posible crearlos directamente desde terminal, esto no lo abarcaremos por ahora, con entender el concepto es más que suficiente para que puedas trabajar.

Empecemos observando que cada vez que hacemos un commit en la rama, GitHub nos da una notificación de que un cambio se realizó y desde aquí podemos crear el Pull Request.

![lab_7](/Tutorials/Lab7Branches/imgs/018.jpg)

La otra forma, es en la parte superior vienen las opciones del repositorio y una de ells es el menú de Pull Request. Si la notificación no apareciera desde aquí podemos crear un Pull Request.

![lab_7](/Tutorials/Lab7Branches/imgs/019.jpg)

Como ya dijimos un Pull Request es el intento de hacer un merge entre una rama y otra. Por lo que al crear uno nuevo veremos una ventana como lo siguiente.

![lab_7](/Tutorials/Lab7Branches/imgs/020.jpg)

Esto me aparece si seleccione crear el Pull Request desde el menú, si seleccionaste la notificación, puede que te precargue algunos datos, pero vamos desde el inicio.

La parte más importante del Pull Request es seleccionar a donde quiero hacer el merge y desde que branch, esto lo haremos en el siguiente espacio.

![lab_7](/Tutorials/Lab7Branches/imgs/021.jpg)

Aquí es muy importante que seleccionemos lo necesario ya que si nos equivocamos, GitHub pudiera encontrar conflictos entre archivos y sería forzar el cambio. Si bien 1 persona es quien escribe el Pull Request, es responsabilidad del equipo revisar que la información sea correcta.

![lab_7](/Tutorials/Lab7Branches/imgs/022.jpg)

Vamos a seleccionar que vamos a mergear en **develop** nuestro branch **feature/contributing-standard**. Y observa que al seleccionarlos, nos aparece un **Able to merge**. Si se encontrarán conflictos entre los archivos puede darse a 2 razones:
1. La versión de **feature/contributing-standard** no esta actualizada a la última versión de **develop**.
2. Seleccionaste un branch para mergear diferente al esperado.
   
En caso de existir diferencias entre la última versión de **develop** y tu rama actual que quieres mergear no lo olvides.

Realiza un pull desde tu rama actual a la rama que quieres hacer merge, esto descargará todos los nuevos cambios. Por ejemplo:

```
git pull origin develop
```

> Nota: Hacer esto es probable que te genere conflictos entre archivos si otros miembros del equipo tocaron archivos que tú también utilizaste. Aquí es cuando entra la comunicación y avisa a las personas correspondientes para resolver los conflictos, **no se vale solo quedarse con tus cambios**.


De momento no tenemos conflictos entonces daremos clic en el botón **Crear Pull Request**

Esto nos creará una vista como la siguiente:

![lab_7](/Tutorials/Lab7Branches/imgs/023.jpg)

![lab_7](/Tutorials/Lab7Branches/imgs/024.jpg)

En la primera parte tenemos un espacio para describir lo que hemos realizado.

En la segunda parte veremos que se hace la lista de todos los commits que contiene ese branch.

Lo siguiente que debemos hacer es crear la descripción de nuestro Pull Request.

Una muy mala práctica es solo poner una frase corta, los equipos de desarrollo más experimentados tienen plantillas donde se incorpora una explicación de los cambios, checklist para verificar que su proceso de desarrollo y calidad se realiza correctamente, e incluso algunos agregan fotos o videos para que la o las personas que revisen el Pull Request entiendan más fácimente de que se está hablando.

Dar información es más fácil a que las personas adivinen que estabas haciendo.

Una ventaja es que el contenido de este espacio recibe lenguaje markdown por lo que puedes hacer muchas combinaciones cuando empiezas a conocer el lenguaje.

En nuestro caso vamos a agregar lo siguiente a manera de estándard.

```
<!--- Provide a general summary of your changes in the Title above -->

## Description
This feature adds a CONTRIBUTING.md file that gives a standard for the team on how to generate commits and branches with git, and also some additional information that can be used to contribute to this repository.

## Motivation and Context
This feature follows the requirements [001]() that can be found on the Excel sheet of the management log of the team.

## Where can I try this functionality?
This feature is not intended for development but to give the team a guide on how to contribute to the project.

## Screenshots (if appropriate):
Not available

## Types of changes
<!--- What types of changes does your code introduce? Put an `x` in all the boxes that apply: -->
- [ ] Bug fix (non-breaking change which fixes an issue)
- [X] New feature (non-breaking change which adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)

## Checklist:
<!--- Go over all the following points, and put an `x` in all the boxes that apply. -->
- [X] I have included links to all Excel Sheet or Traceability matrices (if
      applicable)
- [X] I have added a link to this PR to the Excel sheet
- [ ] I have updated `CHANGELOG.md` accordingly.  (if
      applicable)
- [ ] Another developer has performed a code review on this PR and approved it
- [ ] Quality Assurance has taken place by a team member not directly involved
      in developing this PR.
- [ ] I have updated the related Excel sheet and will move this to "done" after
      merge & deploy.
```

Al finalizar actualizamos el Pull Reques y el resultado se debería ver como el siguiente:

![lab_7](/Tutorials/Lab7Branches/imgs/025.jpg)

Date el tiempo para escribir pull requests, de lo contrario a tu equipo le costará trabajo entender lo que realizaste.

Hasta este punto todo lo hemos realizado como nosotros mismos, el último paso necesario para la persona que realizó el Pull Request es avisar que hay uno nuevo y que se necesita revisar.

> Nota: Es responsabilidad de la persona que crea el Pull Request insistir en que el equipo revise el trabajo, pero no siempre estamos todos disponibles todo el tiempo, por lo que busca la mejor estrategia con tu equipo para que no pase mucho tiempo.

> Nota 2: Algo muy común que puede pasar es que varios miembros del equipo realicen Pull Request al mismo tiempo, uno de los conflictos que tienen al hacer esto es que todos quieren mergear su trabajo a la vez. Pero los Pull Request son una **fila** cuando se atiende uno y se hace merge, el siguiente debe bajar la última versión, resolver conflictos y atenderse. Como ya dije, lo más común es hacerme responsable de mi trabajo pero olvidar a los demás, esto es trabajo en equipo, comuniquense efectivamente para llevar un buen control de actualizaciones para evitar algún problema entre archivos.

Los siguientes pasos los realizaremos en el supuesto de que somos otro miembro del equipo que va a revisar el Pull Request que acabamos de subir.

Si entramos al repositorio el miembro del equipo nos debió compartir el link al Pull Request, de lo contrario podemos encontrarlo desde el menú de Pull Request, cuando hay pocos no pasa nada pero cuando hay varios es mejor que nos indiquen exactamente donde está.

![lab_7](/Tutorials/Lab7Branches/imgs/026.jpg)

Al entrar veremos el detalle que ya vimos previamente,el trabajo de alguien que va a revisar el Pull Request, es ver que la información del mismo tenga sentido, se entienda la funcionalidad que se está realizando y revisar el código.

La primera parte vamos a asumir que es correcta, por lo que vamos a revisar el "código".

Aquí vamos a seleccionar de las opciones en la parte superior la que dice **Files changed**.

![lab_7](/Tutorials/Lab7Branches/imgs/027.jpg)

Aquí nos va a aprecer una vista mostrandonos los cambios y los archivos nuevos, y en particular veremos que aparece primero el **CONTRIBUTING.md**, la pocisión dependerá de la estructura de tu proyecto.

Lo ideal al revisar el código es ver cada cambio, entenderlo y en caso de que algo no quede claro crear un comentario y una revisión para que la persona responsable haga el chequeo pertinente. Ahora bien, estos cambios son sugerencias de mejora, si aplican o no depende del responsable del Pull Request o justificar su acción o realizar el cambio pertinente.

En mi caso voy a ir a la línea 25 y al seleccionar veremos que puedo escribir un comentario para la persona y daremos clic en **Start Review**.


![lab_7](/Tutorials/Lab7Branches/imgs/028.jpg)

Lo que va a suceder es que se agregará el comentario y se notificará al responsable que existe una revisión pendiente.

![lab_7](/Tutorials/Lab7Branches/imgs/029.jpg)

La persona que revisa podrá decidir si revisa lo que resta del código o esperar a que se realice el cambio para hacer una nueva revisión.

En micaso seguir revisando el código. Cuando finalice el archivo **CONTRIBUTING.md** puedo marcarlo como revisado.

![lab_7](/Tutorials/Lab7Branches/imgs/030.jpg)

Termino la revisión y veo que no hay nada más por lo que marco ambos archivos como revisados.

![lab_7](/Tutorials/Lab7Branches/imgs/031.jpg)

Ahora lo que debo hacer es terminar la revisión, en la parte superior tengo un botón que dice **Finish your review.**

![lab_7](/Tutorials/Lab7Branches/imgs/032.jpg)

Aquí voy a poder agregar un comentario global a toda la revisión y voy a poder seleccionar que sigue:

- Comment - Solo agregará los comentarios pero le permitirá a la persona hacer merge de sus cambios.
- Approve - Aprobar los cambios para hacer merge.
- Request changes - Solicitar cambios al responsable y todavía no se puede hacer merge.

Por default, los branches a mergear no vienen bloqueados, por lo que es posible hacer merges sin revisión, ten mucho cuidado con esto siempre realiza un proceso de revisión y estandariza con tu equipo cuales son los pasos a seguir al momento de hacer un Pull Request.

Como en mi ejemplo soy yo mismo no me deja ni aprobar ni solicitar cambios, así que de momento solo vamos a comentar.

![lab_7](/Tutorials/Lab7Branches/imgs/033.jpg)

Damos clic en **Submit Review** y ahora volvemos cmo responsables del Pull Request.

Al entrar nuevamente al Pull Request veremos que se va agregando una conversación, con todos los comentarios echos por el equipo, si necesitara realizar cambios igualmente GitHub me avisaría.

![lab_7](/Tutorials/Lab7Branches/imgs/034.jpg)

Aquí nuevamente, es responsabilidad de la persona atender o no los cambios, pero la realidad es que siempre es bueno al menos contestar si no se ejecuta algún cambio y justificar el por que. Simulando la conversación entre el equipo pasaría lo siguiente.

![lab_7](/Tutorials/Lab7Branches/imgs/035.jpg)

Al final terminamos la conversación con **RESOLVE CONVERSATION** y pasamos al siguiente comentario.

En este Pull Request ya hemos realizado todo lo necesario, entonces como responsables del mismo podemos ir al resumen y prepararnos para el último paso hacer el merge.

![lab_7](/Tutorials/Lab7Branches/imgs/036.jpg)

## Estrategias de Merge (merge, rebase, squash+merge)

Antes de terminar con el PR, vamos a resolver uno de los mitos en el control de versiones, las estrategias de mergeo.

Dentro de Git, tenemos varias opciones para fusionar ramas, y cada una hace algo diferente, es importante conocerlas.

A nivel general las estrategias de mergeo es la forma en la que Git tratará los commits de la rama que estamos fusionando. No hay una sola forma válida, cada estrategia tiene sus pro y sus contras, algunas empresas optan por ciertas estrategias que otras por conveniencia.

Vamos a ver cada estrategia en particular. Veremos que tenemos como estrategias el merge, el rebase y el squash+merge.

![lab_7](/Tutorials/Lab7Branches/imgs/037.jpeg)

- Merge - Es la más amigable de todas las estrategias, esta genera una visualización de como se unifica la rama a través de los commits sin alterar el historial de commits de la rama a fusionar.
- Rebase - Es la más agresiva de las estrategias, aquí se empuja el historial de commits al final de la fila en la rama a fusionar, aquí se debe tener cuidado pues si no se está en la última versión se generarán conflictos importantes.
- Squash+merge - Esta estrategia es sencilla, parecida al rebase, empuja los cambios al final del historial de la rama fusionada, peros con la diferencia al rebase que combina todo el historial de commits de la rama a fusionar en uno solo, por lo que tendremos commits que en su detalle incluirán commits de viejos de la rama a fusionar.

Como puedes ver lo único importante de la estrategia de mergeo es como queremos que se visualice el historial de commits al momento de hacer la fusión.

Para mi pull request yo voy a seleccionar la estrategia de merge pues prefiero mantener la visualización de todo el historial y por donde va pasando. Pero esto no significa que se la única estrategia que utilizó, con mis clientes en ocasiones utilizo la estrategia de rebase por la simplicidad del historial de commits.

Habla con tu equipo y dfefinan que estrategia utilizarán para su proyecto.

![lab_7](/Tutorials/Lab7Branches/imgs/038.jpg)

Nos aseguramos que seleccionamos la estrategia adecuada y damos clic en **Merge Pull Request**.

![lab_7](/Tutorials/Lab7Branches/imgs/039.jpg)

Se nos pedirá una confirmación del merge. Y finalmente habremos terminado.

![lab_7](/Tutorials/Lab7Branches/imgs/040.jpg)

Aquí recuerda lo que te dije, este branch que finalizamos es de **Trabajo** por lo que deberíamos borrar el branch.

![lab_7](/Tutorials/Lab7Branches/imgs/041.jpg)

Si algo malo sucediera, nota que puedes restaurar el branch o incluso revertir el merge, ten cuidad que sea solo al momento, ya que si lo haces más adelante nuevamente puedes poner en riesto el repositorio remoto.

Si regresamos a la vista principal del repositorio y seleccionamos el branch de **develop**, veremos que tenemos la última versión actualizada en el repositorio remoto.

![lab_7](/Tutorials/Lab7Branches/imgs/042.jpg)

Si nos vamos a la lista de branches, veremos que nuestro branch ya no existe.

![lab_7](/Tutorials/Lab7Branches/imgs/043.jpg)

Si nos vamos al menú de Pull Request y modificamos el filtro podremos ver todos los PR que han sido cerrados.

![lab_7](/Tutorials/Lab7Branches/imgs/044.jpg)

Y si nos vamos al historial de commits de **develop** veremos lo siguiente.

![lab_7](/Tutorials/Lab7Branches/imgs/045.jpg)

Ve como la estrategia de merge me agrega todos los commits a **develop**, esto es lo que buscamos ya que al final no tocamos **develop** y tenemos nuestra última funcionalidad añadida.

## Actualizando cambios remotos

Ya que hemos finalizado ahora puedes estarte preguntando, que pasa en mi repositorio local.

Lo primero que debemos hacer es actualizar nuestros cambios, por lo que si nuestro PR, ya finalizó, lo correcto sería cambiarnos al branch de **develop** y actualizar la últma versión.

Si somos otro miembro del equipo lo ideal es actualizar los cambios de **develop** en la rama que estamos trabajando.

Si es a o b, el comando es el mismo, no lo olvides

```
git checkout develop
git pull origin
```

Como resultado actualizaremos a la última versión disponible sea el miembro del equipo que sea, por eso es importante que al hacer un merge avises a tu equipo para que actualicen lo que sea necesario.

```
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (1/1), 912 bytes | 2.00 KiB/s, done.
From github.com:black4ninja/git-learning-branches
   5a06f6d..a78b66b  develop    -> origin/develop
Updating 5a06f6d..a78b66b
Fast-forward
 CONTRIBUTING.md | 153 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++  README.md       |  10 +++++--
 2 files changed, 161 insertions(+), 2 deletions(-)
 create mode 100644 CONTRIBUTING.md
```

Otra pregunta que te puedes estar haciendo es ¿qué pasó con mi branch local de **feature/contributing-standard** pues antes de cambiarnos a **develop** estabamos en él.

Si ejecutamos:

```
git branch
```

Obtendremos que sigue ahí

```
* develop
  feature/contributing-standard
  main
```

Nuevamente debemos entender que aunque realizamos la actualización a la última versión, y que el branch ya no existe en el repositorio local, un branch no se eliminará hasta que no le digamos a git, en este caso podríamos dejarlo hasta ahí y realmente no nos afectaría en nada tenerlo, pero después de trabajar un rato puede que tengas una lista enorme de branches que ya no utilices.

Para sincronizar mis branches locales con los del repositorio remotor vamos a ejecutar la siguiente secuencia de comandos.

```
git fetch
git remote prune origin
git branch | grep -v "main" | xargs git branch -D
```

Donde vamos a sustituir main por master en caso de que así se llamara el principal.

- git fetch - Es un pull pero para los branches del repositorio, esto actualiza cualquier branch que no tengamos y que este en el repositorio remoto.
- git remote prune origin - Purga los branches no utilizados para marcar cuales existen en remoto y cuales en local
- La última línea es una combinación para borrar todos los branches locales que no estén sincornizados con remoto o que no sean el principal **main** o **master**.

Después de prune el resultado de la consola será:

```
Pruning origin
URL: git@github.com:black4ninja/git-learning-branches.git
 * [pruned] origin/feature/contributing-standard
```

Y el resultado del último comando será el siguiente:

```
error: branch '*' not found
error: cannot delete branch 'develop' used by worktree at 'D:/ITESM/TC2005B/Practicas/Lab7Branches/git-learning-branches'
Deleted branch feature/contributing-standard (was 9edef4d).
```


Si ejecutamos un:

```
git branch
```

Obtendremos:

```
* develop
  main
```

Con esto nuestro repositorio queda limpio y podemos seguir trabajando en nuevas funcionalidades.

Solo por curiosidad si hacemos en la consola un:
```
git log --oneline --graph
```

Veremos un resultado interesante:
```
*   a78b66b (HEAD -> develop, origin/develop) Merge pull request #1 from black4ninja/feature/contributing-standard
|\
| * 9edef4d feature: Linked CONTRIBUTING.md file with README.md.
| * 38278cf feature: Added CONTRIBUTING.md with standard specification on how team must work with branches, commits and additional stuff.
| * 6dc4490 feature: Changed description README.md
| * 5117385 feature: Changed title README.md and started migration to English language.
|/
* 5a06f6d fix: Se corrige el typo de la descripción del README.md
* 808115e (origin/main, origin/HEAD, main) Initial commit
```

Aquí veremos más clara la estrategia del merge pues nos dice como se hizo la bifurcación, los commits de nuestra rama y como se pego al final.

Además veremos que nuestro HEAD apunta a **develop** indicando donde estamos y también que **main** se ha quedado atrás en su última versión, pero que al final tenemos la versión más actualizada del mismo.

Por lo mismo ya podemos planear hacer un merge a **main**.

## Actualizar main o master

Hasta ahora, ya tenemos todas las herramientas para un proceso de control de versiones completo, pero nos falta subir nuestros cambios a main.

De aquí en adelante y no será nada nuevo lo que haremos pues los pasos a seguir son:

- Bajar la última versión de **develop** y asegurarnos que no hay cambios adicionales a realizar.
- Crear un pull request para hacer merge en **main** desde **develop**.
- Hacer la revisión correspondiente.
- Corregir cualquier cambio adicional.
- Hacer el merge.

Iremos un poco más rápido pero iremos paso a paso, si consideras que ya entendiste toda la leccón te invito a que lo intentes, incluso si fallas aprenderás a que no hacer y como resolver problemas, mejor ahorita que en tu proyecto.

### Bajar la última versión de **develop** y asegurarnos que no hay cambios adicionales a realizar.

Hacemos un:
```
git pull origin
git pull origin main
```
Primero bajamos los últimos cambios de **develop** y luego nos aseguramos que la posición de **develop** corresponda al final de **main** 

En este caso particular no necesitamos hacer nada más y sabemos que el repositorio remoto tiene todos los cambios necesarios para hacer el Pull Request.

### Crear un pull request para hacer merge en **main** desde **develop**

Para este PR, es diferente a los PR que vas a trabajar con tu equipo, pues este es el condensado de todo el trabajo que hayan realizado hasta el momento.

Idealmente sería suficiente llenar la información con la lista de PR que incluye para que en caso de querer ver el detalle, la persona que revise entienda que incluye. No es necesario repetir información de PR  de trabajo ya cerrados.

![lab_7](/Tutorials/Lab7Branches/imgs/046.jpg)

También observa que el título del PR lo modifique del default que me da como nombre del branch **develop** a **Release 0.0.1** Aquí dependerá la convención que utilizes con tu equipo pero sean descriptivos para entender mejor en el historial de commits.

### Hacer la revisión correspondiente.

La persona que revisa el PR, podrá irse a **Files Changed** y verán que son todos los archivos que modificamos en el PR anterior, aquí es más cuestión del equipo si deciden hacer una última revisión o al considerar que el PR anterior ya cubrió con todo los estándares pueden evitar la revisión.

### Corregir cualquier cambio adicional

Lo más común es que para este momento se ejecute un set de pruebas automáticas indicando al equipo si todas las pruebas se pasaron correctamente, en caso de que sí se puede pasar a hacer el merge.

### Hacer el merge

Nuevamente seleccionamos la estrategia de merge que queremos aplicar, en mi caso merge solamente, y aceptamos los cambios.

![lab_7](/Tutorials/Lab7Branches/imgs/047.jpg)

En este caso no queremos eliminar **develop** pues seguiremos trabajando como está.

Pero si regresamos a la página principal seleccionando **main** veremos:

![lab_7](/Tutorials/Lab7Branches/imgs/048.jpg)

Que efectivamente main contiene la última versión.

Listo, has sobrevivido al GitFlow. Ahora es que empieces a practicar y hagas el compendio de los comandos necesarios para que recuerdes lo que debes trabajar.

Pero hey, podemos hacer un último detalle para trabajar como los profesionales.

## Añadiendo etiquetas a versiones terminadas

Un paso que realizan muchas empresas al finalizar una versión, es marcarla como una versión estable, ya que si algo malo sucede puede entrar en acción rápidamente sin depender del repositorio, esto permite automatizar a que en el momento de crear estas versiones especiales se suban de manera automática a los servidores y ahorran mucho tiempo y esfuerzo por parte del equipo de desarrollo.

Marecar las versiones estables tiene muchas formas de hacerse, algunos extienden el GitFlow creando ramas especiales donde solo se hacen merge de dichas versiones, en otros casos al finalizar un merge a **main** se ejecuta una serie de comandos para generar esta versión especial.

En nuestro caso vamos a crear una etiqueta y que eso quede en el historial aparte para que de ser necesario podamos bajar la última versión estable.

Para hacerlo no olvidemos actualizar nuestr última version desde terminal:

```
git checkout main
git pull origin
```

Solo como formalidad el resultado de la actualización debería verse como el siguiente:

```
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (1/1), 897 bytes | 7.00 KiB/s, done.
From github.com:black4ninja/git-learning-branches
   808115e..b3ff2e1  main       -> origin/main
Updating 808115e..b3ff2e1
Fast-forward
 CONTRIBUTING.md | 153 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++  README.md       |  10 +++++--
 2 files changed, 161 insertions(+), 2 deletions(-)
 create mode 100644 CONTRIBUTING.md
```

Si ejecutamos un:

```
git log --oneline --graph
```

Veremos como ha evolucionado nuestro historial de commits con todo lo que hemos echo actualizando hasta **main**.

```
*   b3ff2e1 (HEAD -> main, origin/main, origin/HEAD) Merge pull request #2 from black4ninja/develop
|\
| *   a78b66b (origin/develop, develop) Merge pull request #1 from black4ninja/feature/contributing-standard
| |\
| | * 9edef4d feature: Linked CONTRIBUTING.md file with README.md.
| | * 38278cf feature: Added CONTRIBUTING.md with standard specification on how team must work with branches, commits and additional stuff.
| | * 6dc4490 feature: Changed description README.md
| | * 5117385 feature: Changed title README.md and started migration to English language.
| |/
| * 5a06f6d fix: Se corrige el typo de la descripción del README.md
|/
* 808115e Initial commit
```

Ahora bien, para crear una etiqueta con la nueva versión debemos asegurarnos que estamos en **main**.

Desde aquí vamos a ejecutar lo siguiente **git tag -a v[version]**, donde version es el número de versión que asignaremos a nuestro trabajo:

```
git tag -a v0.0.1
```

Aquí en nuestra terminal se nos va a abrir una especie de editor de texto, si es la primera vez que lo haces seguramente te preguntará cuál. Selecciona el de tu preferencia.

En este editor agrega una pequeña descripción de que es lo que contiene la versión nueva.

![lab_7](/Tutorials/Lab7Branches/imgs/049.jpg)

Por último ejecutamos el siguiente comando **git push origin v[version]** donde la versión debe coincidir con la que creamos anteriormente.

```
git push origin v0.0.1
```

El resultado en la terminal será lo siguiente:

```
Enumerating objects: 1, done.
Counting objects: 100% (1/1), done.
Writing objects: 100% (1/1), 209 bytes | 209.00 KiB/s, done.
Total 1 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:black4ninja/git-learning-branches.git
 * [new tag]         v0.0.1 -> v0.0.1
 ```

Pero lo más práctico es ir a GitHub y ver que suicedió.

![lab_7](/Tutorials/Lab7Branches/imgs/050.jpg)
![lab_7](/Tutorials/Lab7Branches/imgs/051.jpg)
![lab_7](/Tutorials/Lab7Branches/imgs/052.jpg)
![lab_7](/Tutorials/Lab7Branches/imgs/053.jpg)

El contador de releases ha aumentado en nuestro repositorio.

Si entramos a esta sección veremos lo siguiente.

Primero tendremos una lista de versiones que en nuestro caso coincide con la versión creada.

![lab_7](/Tutorials/Lab7Branches/imgs/054.jpg)

Si entramos al detalle de la versión veremos entonces:

![lab_7](/Tutorials/Lab7Branches/imgs/055.jpg)

Y aquí observa como todo lo que contiene nuestro repositorio fue añadido a un archivo .zip y .tar.gz, este es nuestro código integro, para que en caso de que algo malo pase en el repositorio ya tenemos una versión aislada para recuperar y subir a nuestro servidor.

Eso es todo, acabamos de realizar un proceso de desarrollo completo desde el punto de vista de control de versiones.

 Nuevamente de inicio puede parecer desafiante, pero este es el día a día de todos los desarrolladores en el mundo que siguen este estándar, revisa otros repositorios de GitHub y observa las diferencias entre los commits, los pull request, los branches y adquiere tu propio estilo de trabajo para tus equipos.

 Al final del día lo que se busca es hacer más fácil la vida de los programadores, no olvides que si tienes alguna duda acercarte con tus profesores para no quedarte atrás.