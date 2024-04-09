# Control de versiones

## Introducción básica
Dentro del mundo de la programación, tener un manejo de control de versiones es parte del día a día.

Esto implica que el programador tiene un lugar donde se aloja el código de cada proyecto.

También permite que el usuario cambie de equipo para trabajar en diferentes lugares teniendo la misma configuración.

También permite que equipos de trabajo puedan compartir su código y trabajar de manera colaborativa.

Es importante que al trabajar con control de versiones entendamos en que momento se encuentran nuestros archivos para que en caso de problemas no perdamos información para ello debemos entender como funciona el **working tree**, la **staging area** y el **repository**.

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

Cuando haces un cambio en el staging area es permanente, se queda removido de la staging area y commiteado al **reppo local**. Un commit es un grabado permanente de ese cambio. El repo contiene todos los commits que se han realizado.

## Requerimientos Previos
Instala Git en tu computadora según sea la versión de tu Sistema Operativo. [Git Oficial](https://git-scm.com/)

> En windows puede que necesites utilizar el Git-Shell que se instala junto con Git, para usarlo en tu terminal normal necesitas agregarlo a las variables de entorno, investiga al respecto.

## Manejo básico de repositorios

La siguiente secuencia de comandos es para que te familiarices con Git, todavía no vamos a conectarlo con un repositorio remoto.

### Iniciar un repositorio
Al momento de iniciar un repositorio puedes hacerlo desde línea de comando.

```
git init
```

Para saber que el repositorio se inicio correctamente se debe crear un folder .git que por default es una carpeta ocular, debes habilitar ver este tipo de carpetas para poder verlas desde el manejador de archivos detu sistema operativo.

Al momento de crear un nuevo repositorio se crea un branch default, más adelante trabajaremos con los branches. Por ahora lo único que debes saber es que el default es **master**, sí creas el repositorio desde la nube en ocasiones puede ser llamado como **main**.

Todo lo que vamos a hacer en este laboratorio es para trabajar directamente en **master** o **main**.

### Setear el nombre y correo para el repo (obligatorio)
Para que en un repositorio puedas hacer commit es necesario establecer un correo y un nombre. Esto lo hacemos con los comandos.

```
git config user.email alice@example.com
git config user.name "Alice Zakas"
```

Los comandos anteriores sirven para establecer el correo y nombre para 1 solo repositorio, el que estoy utilizando.

Una forma de evitar esto cada vez que trabajemos en nuevo proyecto es usar la directiva --global, esto hace que cada comando se establezca en una configuración global de todo nuestro git para que en cada proyecto se utilice la misma configuración y evitemos tener que escribirlo.

```
git config user.email alice@example.com --global
git config user.name "Alice Zakas" --global
```

Si quieres ver tu configuración local de git puedes usar el comando

```
git config --list --show-origin
```

### Agregar archivo
Una vez que tenemos preparado nuestro repositorio podemos crear un nuevo archivo y agregarlo al staging area.

> Nota: Para los siguiente ejemplos usaremos comandos para crear rápidamente archivos de texto, pero si se te hace más fácil puedes usar el editor de texto de tu preferencia.

```
echo "git is awesom" > message.txt
git add message.txt
```

Una vez que creamos el archivo vamos a ver los cambios en el staging area

```
git diff --cached
```

El resultado debería ser algo como lo siguiente
```
diff --git a/message.txt b/message.txt
new file mode 100644
index 0000000..0165e86
--- /dev/null
+++ b/message.txt
@@ -0,0 +1 @@
+git is awesom
```

Ahora vamos a preprara un commit al repositorio local

```
git commit -m "add message"
```

### Editar archivo
Ahora vamos a ver como editar un archivo

```
echo "git is awesome" > message.txt
```

Si ejecutamos git diff 

```
git diff
```

Veremos lo siguiente

```
diff --git a/message.txt b/message.txt
index 5db4f02..d845108 100644
--- a/message.txt
+++ b/message.txt
@@ -1 +1 @@
-"git is awesom"
+"git is awesome"
```

Vamos a agregar los archivos modificados y a hacer el commit en 1 sola línea de comando

```
git commit -am "edit message"
```

El resultado será algo como

```
[master 6bb3579] edit message
 1 file changed, 1 insertion(+), 1 deletion(-)
```

> La directiva -a no agrega nuevos archivos, solo cambia los archivos que ya fueron commiteados.

### Renombrar archivo
Vamos a renombrar el archivo que previamente hicimos commit

```
git mv message.txt praise.txt
```

El cambio ya se encuentra en la staging area, por lo que git diff no lo va a mostrar. Utiliza --cached

```
git diff --cached
```

El resultado deberá verse como

```
diff --git a/message.txt b/praise.txt
similarity index 100%
rename from message.txt
rename to praise.txt
```

Ahora nuevamente has commit del cambio

```
git commit -m "rename message.txt"
```

```
[master e19e197] rename message.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 rename message.txt => praise.txt (100%)
```

### Borrar un archivo
Vamos a borrar el archivo anterior

```
git rm message.txt
```

El cambio ya se encuentra en staging, por lo que diff no lo mostrará. Usa --cached

```
git diff --cached
```

Has commit del cambio

```
git commit -m "delete message.txt"
```

### Mostrar el estatus actual
Edita el archivo previamente commiteado y agrega los cambios a la staging area.

```
echo "git is awesome" > message.txt
git add message.txt
```

Crea un nuevo archivo
```
echo "git is great" > praise.txt
```

Para mostrar el estatus actual vamos a utilizar el comando

```
git status
```

El resultado se verá como

```
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   message.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        praise.txt

```

> Nota que message.txt esta en el staging area, mientras que praise.txt no está siendo trackeado.

### Mostrar log de commits

```
git log
```

El resultado se verá como
```
commit ecdeb79aad4565d8d7d725678ffadc48b3cdec52
Author: sandbox <sandbox@example.com>
Date:   Thu Mar 14 15:00:00 2024 +0000

    edit message

commit 3a2bd8f0929c605193120bd1ad12732f49457e99
Author: sandbox <sandbox@example.com>
Date:   Thu Mar 14 15:00:00 2024 +0000

    add message
```

Otra forma de ver el log de manera simplificada es

```
git log --oneline
```

```
ecdeb79 edit message
3a2bd8f add message
```

También se puede ver el log en forma de grafo

```
git log --graph
```

```
* commit ecdeb79aad4565d8d7d725678ffadc48b3cdec52
| Author: sandbox <sandbox@example.com>
| Date:   Thu Mar 14 15:00:00 2024 +0000
| 
|     edit message
| 
* commit 3a2bd8f0929c605193120bd1ad12732f49457e99
  Author: sandbox <sandbox@example.com>
  Date:   Thu Mar 14 15:00:00 2024 +0000
```

Y por último podemos combinar la visualización en una línea y en grafo

```
git log --oneline --graph
```

```
* ecdeb79 edit message
* 3a2bd8f add message
```

### Mostrar un commit específico

Para mostrar el último commit realizo utiliza

```
git show HEAD
```

HEAD es el apuntador especial que marca donde se encuentra la línea del último commit del repositorio.

```
commit ecdeb79aad4565d8d7d725678ffadc48b3cdec52
Author: sandbox <sandbox@example.com>
Date:   Thu Mar 14 15:00:00 2024 +0000

    edit message

diff --git a/message.txt b/message.txt
index 0165e86..118f108 100644
--- a/message.txt
+++ b/message.txt
@@ -1 +1 @@
-git is awesom
+git is awesome
```

Mostrar el segundo-al-último commit

```
git show HEAD~1
```

Usar HEAD~n muestra el nth-antes-del-último commit o usa el has del commit específico en lugar de HEAD~n.

Cuando crear un commit siempre se crea un hash de ese commit, este es la firma directa de ese commit y justamente sirve para cuando necesitamos ver el detalle de ese commit o como en este caso ver o regresar a un commit.

Ten cuidado cuando trabajes con el hash, el completo es:

```
ecdeb79aad4565d8d7d725678ffadc48b3cdec52
```

Ya que algunos comandos o en algunas visualizaciones te aparece simplificado

```
ecdeb79
```

El simplificado sirve para visualización pero para los comandos utiliza el completo.

### Resumen
Con esto hemos visto los comandos básicos de git para trabajar con nuestro repositorio local.

## Manejo de repositorios en la nube

Para poder trabajar en la nube dependerá de la plataforma que usemos, pero el estándar al final del día es que git utiliza los mismo comandos, lo que va a variar es como funciona la plataforma que utilicemos.

### Crea un repositorio en la plataforma de tu elección

Dentro de la plataforma que uses para trabajar (Github, Bitbucket, Gitlab...), busca como crear un nuevo repositorio.

Configura los datos básicos necesarios hasta que puedas clonar el repositorio.

### Crea una llave de ssh para tu computadora
Para poder clonar un repositorio puedes usar **https** o **ssh**, la diferencia son protocolos de seguridad en como vamos a subir y bajar la información del repositorio, la más simple es https.

- En https inicialmente se usaba tu usuario y contraseña de la plataforma cada vez que realizas un commit para poder firmarlo y conectarte. La realidad es que existen nuevas funciones de seguridad que obligan al usuario a crear un token dentro de la plataforma y en vez de la contraseña utilizar dicho token, por lo que necesitas realizar un proceso adicional y guardar muy bien el token para poder hacer tus commits.
- El modo de ssh, es un protocolo de seguridad donde desde tu computadora creas un archivo el cual debes registrar en la plataforma de tu repositorio en la nube y con ello ya no vas a necesitar autentificarte cada vez que realices un commit.

Si bien puedes usar uno u otro, la recomendación es que por seguridad generes tu llave de ssh, ya que hay repositorios que son restringidos solo a este tipo de comunicación y siempre va a ser una mejor práctica de seguridad.

Si utilizas Github sigue este [manual](https://docs.github.com/es/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) para crear tu llave SSH.
Si utilizar Bitbucket este sería el [manual](https://support.atlassian.com/bitbucket-cloud/docs/configure-ssh-and-two-step-verification/)

Puede ser un proceso desafiante y un poco complicado al inicio pero a la larga te evitara muchos problemas y hoy en día es la forma de trabajar como todo un profesional.

### Clonar el repositorio en la nube

Ya que tienes un repositorio en la nube y que tienes un método de conexión puedes obtener la url de https o de ssh y ejecuta la siguiente línea de comando en una carpeta vacía local donde vayas a tener tu repositorio.

```
git clone {{url https o ssh del repositorio en la nube}}
```

### Subir los cambios al repositorio en la nube
Una vez que clonemos el repositorio siempre es una buena práctica utilizar el comando 

```
git pull origin
```

Lo que hace **git pull** es bajar los últimos cambios que existen desde el repositorio en la nube.

> La mejor práctica es que constantemente realices git pull para siempre tener la última versión del repositorio.

Como equipo de trabajo quien siempre va a tener la mejor versión es la nube a menos que subas un nuevo cambio, esto evitará que tú y tu equipo eliminen información valiosa del repositorio.

Ahora ya que hiciste pull puedes hacer tu trabajo dentro de tu repositorio. Al finalizar debes

```
git add -A
git commit -m "Mensaje del commit"
```

Y por último deberás subir los archivos al repositorio en la nube, esto lo hacemos con el comando

```
git push origin
```

>Nuevamente antes de realizar y subir cambios al repositorio en la nube no olvides hacer un pull, y evita hacer commits de varios días de trabajo pues esto solo genera muchos cambios que corre el riesgo de afectar a todo el equipo y poner el riesgo el avance al momento.

## Conclusión
Con esto puedes trabajar con un repositorio de manera básica, existe un uso más avanzado de Git que veremos más adelante.