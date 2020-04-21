﻿# Aprender a usar el terminal GIT con ejemplos

Si sueles trabajar con alguna interfaz gráfica y quieres aprender los conceptos básicos para trabar usando el terminal, continúa leyendo.

En este Readme encontrarás paso a paso un tutorial guiado que te ayudará a aprender de forma practica como utilizar el terminal.

En este repositorio tienes un proyecto básico (un proyecto web que muestra un número por la consola), y durante el desarrollo del mismo te proporcionaremos una guía para:

- Recrear el repositorio desde 0.
- Realizar puntos de confirmación (Commit a partir de ahora) y subir los cambios (de ahora en adelante lo llamaremos Push).
- Crear ramas.
- Unir ramas.
- Resolver conflictos.

Usaremos un terminal de comando git para configurar y trabajar con el repositorio.

Prerequisitos para realizar este tutorial:

- Instalar [Git](https://git-scm.com/downloads).
- Instalar [nodejs](https://nodejs.org/es/).

# Creación del repositorio

Se puede elegir entre usar una conexión HTTPS (con tu usuario estándar de git) o una conexión SSH (una aproximación más seguro para tratar con tus repositorios, pero necesita una configuración adicional)

> Mas información sobre como [crear una clave SSH en tu ordenador](https://confluence.atlassian.com/bitbucketserver/creating-ssh-keys-776639788.html), y [configurarlo en Git](https://help.github.com/en/enterprise/2.18/user/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account)

Para crear el repositorio solo tienes que pulsar en el botón _nuevo repositorio_ en Github o en tu proveedor de repositorios favorito.

> Cuando creas un nuevo proyecto, es una buena práctica seleccionar la opción de crear un fichero _readme.md_ por defecto. Realizando este paso, tendremos nuestro commit inicial.

# Clonar repositorio

Ahora que ya tienes tu repositorio creado, es hora de traerlo a tu equipo de forma local, para hacer eso lo clonamos (descargar el repositorio a una carpeta local del ordenador).

> Asegúrate que tienes permiso para acceder a esos repositorios, y si estas usando SSH tienes que tenerlo correctamente configurado (generada localmente tu clave privada y publica SSH, y subida tu clave publica a tu repositorio).

Elige una carpeta para descargar los ficheros, abriendo un terminal windows y ejecutando el siguiente comando:

```bash
git clone <repository_address>
```

> Normalmente los proveedores de repositorios git te muestran en algún cuadro de texto la dirección url / ssh del repositorios para que la puedas copiar.

![Clonar usando SSH or HTTPS](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/clone-with-ssh-or-https.png)

# Añadir un proyecto web básico

** Esto no está relacionado con Git, sólo tenemos que crear una proyecto básico para jugar con el**

Vamos a añadirle contenido al repositorio.

Empezaremos creando un proyecto web básico (configuramos nodejs y un proyecto node que muestra un mensaje por consola), vamos a ejecutar el siguiente comando:

```bash
npm init -y
```

e instalamos un bundler:

```
npm install parcel rimraf --save-dev
```

Vamos a crear el siguiente fichero en la carpeta _src_:

_./src/index.js_

```javascript
const sampleNumber = 1;
console.log(`Hello number ${sampleNumber}`);
```

Vamos a crear una pagina HTML para el inicio de la aplicación.

_./src/index.html_

```html
<html>
  <body>
    <h1>Check the console log</h1>
    <script src="./index.js"></script>
  </body>
</html>
```

Vamos a añadir el siguiente comando al fichero _package.json_:

_./package.json_

```diff
"scripts": {
+    "start": "rimraf dist && parcel ./src/index.html --open",
    "test": "echo \"Error: no test specified\" && exit 1"
},
```

Es hora de comprobar si nuestro proyecto funciona, en el termina ejecutamos:

```bash
npm start
```

Se abrirá una ventana del navegador y si tienes abierta la consola (por ejemplo en windows pulsando F12) puedes comprobar si se muestra el mensaje.

Hasta ahora todo bien... ya tenemos un proyecto basico para jugar con el, comencemos a trabajar con Git!

# Haciendo nuestro primer commit

Quizás pensemos que ahora estamos preparado para hacer nuestro primer commit en la rama master (es la rama por defecto se crea en nuestro proyecto)

Antes de hacerlo, chequeamos que ficheros han sido modificado, para esto abrimos el termina y escribimos el siguiente comando:

```bash
git status
```

![Estado inicial de Git](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/initial-git-status.PNG)

Como ???? Sólo hemos creado unos pocos ficheros... ¿Por qué tenemos esa enorme cantidad de ficheros en nuestra lista? Bien _npm install_ los creó, puedes ver que en la carpeta _node_modules están todos los ficheros que son necesario para ejecutar la aplicación pero no son necesario en el repositorio (y no debes subirlos), tenemos que ignorarlos, para ello creamos un fichero llamado _.gitignore\_ e incluimos el siguiente contenido:

_./.gitignore_

```
/node_modules
/dist
/.cache
```

Haciendo esto, definimos que se ignore todos los ficheros incluidos en la carpeta _node_modules_ (no se subirá a nuestro repositorio), [pulsa aquí para obtener mas información sobre como incluir entradas y patrones en un fichero .gitIgnore](https://git-scm.com/docs/gitignore).

Ahora si volvemos a ejecutar el comando _status_ podemos ver que solo se muestran los ficheros que hemos creado y están pendientes de ser añadido al repositorio:

```bash
git status
```

![Git status después de añadirle gitignore ](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/after-gitignore-git-status.PNG)

Ahora si queremos subir estos cambios al nuestro servidor git, necesitamos realizar 3 pasos:

- Primero, necesitas marcar esos ficheros como listos para ser incluidos en el _commit_ (puedes hacerlo fichero por fichero o seleccionando todos los ficheros).
- Segundo, necesitas hacer el _commit_ de los ficheros en tu base de datos local de git.
- Tercero, ahora puedes hacer _push_ de estos cambios al servidor git.

Vamos a marcar a los ficheros como candidatos al _commit_ (estado _staged_)

```bash
git add .
```

Ahora podemos hacer _commit_ a nuestra base de datos local, añadiremos el parámetro _-m_ para añadir un mensaje al _commit_:

```bash
git commit -m "initial project setup"
```

![Git commit](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/git-commit.PNG)

Ya estamos listos para subir los cambios locales al servidor:

```bash
git push
```

Para comprobar que todo ha ido bien, abre tu navegador, abre la web de tu proveedor git y comprueba que el contenido subido está disponible.

# Crear y unificar ramas

Ahora que hemos realizado los primeros pasos usando git es hora de avanzar, vamos a aprender los comando básicos para la creación de ramas.

Una rama nos permite crear una copia aislada de la rama _master_ (o cualquier otra rama) y trabajar aisladamente del resto del equipo. Esto es util para:

- Evitar añadir funcionalidades no finalizadas a nuetro producto.
- Minimizar el impacto que produce a los miembros del equipo de desarrollo cuando se añaden cambios realizados por otros compañeros.
- Revisar el trabajo realizado antes de incluirlo en la rama _master_.

Vamos a implementar una nueva funcionalidad en nuestra aplicación: mostrar en la consola de log un segundo numero. Para evitar el impacto a los otros miembros del equipo mientras desarrollamos nuestra mejora, vamos a crear una rama. Para ello ejecutamos el siguiente comando:

```
git branch feature/display-number-b
```

> Sobre la convención de nombres para la rama creada, estamos usando gitflow [pulsa aquí para obtener mas información sobre como nombrar las ramas y el uso de gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)

Podemos ejecutar el comando _git branch_ para comprobar que la rama ha sido creada correctamente. Este comando nos indicará la rama activa (master):

```bash
git branch
```

Vamos a cambiar de nuestra rama _master_ a la rama _feature/display-number-b_

```bash
git checkout feature/display-number-b
```

![Git checkout branch](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/git-checkout-branch.png)

> Puedes cambiar de una rama a otra si la rama activa no tiene cambios pendientes (Si la rama activa tiene cambios pendientes, tienes que hacer un _commit_ de ellos, descartarlos o almacenarlos en un espacio temporal, veremos esto mas adelante en el tutorial).

Otra fomar más rapida es crear una rama y cambirse a ella directamente ejecutando este comando:

```bash
git checkout -b feature/display-number-b
```

![Git checkout -b](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/git-checkout-b.PNG)

Ahora podemos empezar a trabajar en nuestra rama sin provocar ningún impacto en los otros desarrolladores que estan trabajando en la rama _master_.

Vamos a añadir algunos cambios al fichero _.src/index.js_:

_./src/index.js_

```diff
const sampleNumber = 1;
+ const sampleNumberB = 2;
- console.log(`Hello number ${sampleNumber}`);
+ console.log(`Hello number ${sampleNumber} {sampleNumberB}`);
```

Guardamos los cambios, y los añadimos al _commit_:

```bash
git add .
```

> Este comando añade al _commit_ cualquier fichero nuevo o modificado.

Ya podemos hacer commit de los ficheros en nuestro base de datos local:

```bash
git commit -m "adding one more number"
```

![Git commit](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/git-commit-first-change.PNG)

Y subimos los cambios (_push_) a nuestro servidor remoto (En esta caso la rama nueva no existe en el servido, necesitamos añadir un parametro para indicar que se va a crear la rama en el servidor _origin_)

```bash
git push -u origin feature/display-number-b
```

Ahora podriamos volcar (_merge_) nuestra rama a la rama de _master_, pero no lo vamos a hacer por ahora: primero vamos a simular que otro desarrollador crea otra rama y modifica el fichero _./src/index.js_, esto provocará un conflicto, que aprenderemos a resolver en la siguiente sección del tutorial.

> Un buen consejo: nunca hagas un merge directamente de una rama a master, en vez de eso, realiza una petición para que revisen tu código (puedes realizar esta petición usando el cliente web de tu repositorio preferido de git), por otro lado es una buena práctica mergear primero la rama _master_ a tu rama usando el terminal, de esta forma actualizaras tu código con los cambios que se han realizado en master.

# Manejando conflictos

Antes de realizar el volcado de nuestra nueva rama a _master_, vamos a simular que otro programador (o nosotros mismos) ha creado una segunda rama y modifica los ficheros que nosotros acabamos de actualizar.

Cambiemos a la rama de _master_:

```bash
git checkout master
```

Vamos a crear una nueva rama partiendo de _master_:

```bash
git branch feature/display-number-c
```

Cambiemos a esa nueva rama:

```bash
git checkout feature/display-number-c
```

Y modificamos el fichero _index.js_ y incluimos un _numberC_ para mostrarlo

_/.src/index.js_

```diff
const sampleNumber = 1;
+ const sampleNumberC = 3;
- console.log(`Hello number ${sampleNumber}`);
+ console.log(`Hello number ${sampleNumber} ${sampleNumberC}`);
```

Añadimos al commit los ficheros modificados:

```bash
git add .
```

Vamos a hacer _commit_ de este fichero.

```bash
git commit -m "Adding third number"
```

Y subimos los cambios:

```bash
git push -u origin feature/display-number-c
```

Comprobemos las listas de ramas que tenemos disponibles:

```bash
git branch
```

Tenemos tres ramas: _master_, _feature/display-number-b_, _feature/display-number-c_ y la rama activa _feature/display-number-c_

Suponemos que tenemos el OK de nuestro equipo para mergear la rama _feature-display-number-b_ a _master_, vamos a ellos.

Cambiemos a la rama _master_

```bash
git checkout master
```

Mergeamos _featute/display-number-b_ en _master_:

```bash
git merge feature/display-number-b
```

![Git merge](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/git-merge-branch-display-number-b.PNG)

> Una buena practica antes de mergear una rama a _master_ es asegurarse que la rama está actualizada con la última versión de _master_ (mergear _master_ en nuestra rama)

> Esto lo podemos hacer porque tenemos las ramas disponibles localmente, si es una rama creada por otro usuario puede ser que solo este disponible en el servidor y no en mi equipo local. Tendriamos que ejecutar el comando _fetch_ para traer dicha rama a nuestro equipo (ver en la sección de varios).

En este caso no hay conflictos, por tanto podemos continuar.

Vamos a subir los cambios al servidor.

```bash
git push
```

Ahora puedes usar tu navegador web e ir a la pagina de tu proveedor de respositorios y comprobar que se han subido los cambios, genial!

El siguiente paso será mas complejo... queremos mergear la rama _feature/display-number-c_ en _master_, ¿Por qué es más dificil en esta ocasión? Porque la version de _master_ de la que partimos para crear la rama _feature/display-number-c_ es diferente a la actual, y hay conflictos (el fichero*index.js* es diferentes en ambas versiones). Necesitamos resolver los conflictos y seleccionar que parte de codigo es la correcta para el buen funcionamiento de la aplicación.

Vamos a mergear _feature/display-number-c_ en _master_.

Asegurate primero que estas en la rama de _master_, ejecutando el comando _git branch_ obtendremos la lista de ramas disponibles y se verá resaltada la rama activa.

```bash
git branch
```

Ahora volcamos esta rama en _master_.

```bash
git merge feature/display-number-c
```

![Conflictos en el volcado](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/git-merge-branch-display-number-c-with-conflicts.PNG)

**Ops tenemos conflictos !!** Tenemos que resolverlo con el comando _git mergetool_.

Antes de lanzar este comando, vamos a realizar la siguiente comprobación, tienes alguna herramienta de resolución de conflictos instalada y configurada? Si no la tienes, salta a la seccion **otros/Configurar una herramienta de resolución de conflictos** de este tutorial.

Vamos a lanzar el comando para empezar el merge:

```bash
git mergetool
```

![VSCode mergetool](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/vscode-mergetool.PNG)

Dependiendo de tu configuración, se lanzará una herramienta (KDiff, VSCode, p4Merge...). Ahora tiene que ir recorriendo todos los conflictos y eligiendo (normalmente tienes que elegir conflicto por conflicto):

- Conservar tus cambios locales y descartar los cambios de la rama donde se ha mezclado los cambios.
- Aplicar los cambios que se han producido en la mezcla de ambas ramas.
- Modificar tu mismo el codigo para resolver el conflicto de forma manual.

Dependiendo de la herramienta, se intentará resolver de forma automática algunos conflictos.

> Si no tienes configurada una herramienta de resolución de conflictos, o quieres aprender mas sobre ella, consulta la seccion _Otros/Configurar una herramienta de resolución de conflictos_

Una vez que tengas todos los conflictos resueltos, pasamos los ficheros cambiados a staging y tendras que hacer un commit.

```bash
git add .
```

```bash
git commit -m "fixing merge conflicts"
```

![Git commit merge](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/git-commit-merge-conflicts.PNG)

> Algunos desarrolladores prefieren usar herramientas con interfaz gráfica para manejar estos casos (por ejemplo Source Tree, Git Kraken, VSCode).

Ahora podemos hacer push para subir los cambios al servidor.

```bash
git push
```

Tienes muchos ficheros temporales con la extensión _\*.orig_? Puedes configurar la instalación de git para que no guarde esos ficheros:

```bash
git config --global mergetool.keepBackup false
```

# Otros

## Como crear un proyecto en local y después subirlo

Podemos crear un proyecto en nuestro ordenador (con git init) y más
adelante cambiar el _origin_ para que apunte a un repositorio en la nube
que hayamos creado.

```bash
// Resumen de comandos tirando ya de una base de código existente
Git init                                           // Initializar nuestro repo en local (crea un BBDD local)
git add origin http://........ (La url de tu repo) // Hacemos que apunte al repo en github que hemos creado en blanco
Git add .                                          // Para poder llevar los datos a mi BBDD local primero los tengo que mover a una zona que se llama staging
Git commit -m "comentario"                         // Ya de staging puedo grabarlos en mi BBDD local, lo de -m es un mensaje para el commit
Git push                                           // con el push me lo llevo al servidor de github
```

## Configurar una herramienta para hacer los merges

Existe multitud de herramientas para hacer los merges, y decidir cual utilizar puede ser dificil, por ejemplo:

- Kdiff
- VSCode
- p4Merge
- ...

En este [post](https://www.git-tower.com/blog/diff-tools-windows/) puedes ver las disponibles para Windows, en este [otro](https://www.tecmint.com/best-linux-file-diff-tools-comparison/) para Linux, y para Mac teneis este [post](https://www.lawtechnologytoday.org/2017/11/mac-comparing/).

[Como configurar una herramienta para hacer los merges en Mac](https://coderwall.com/p/3wuuda/set-diffmerge-as-default-merge-tool-in-os-x)

Por ejemplo:

Configurar Kdiff en windows:

- Instalar Kdiff, puedes decargarlo [aquí](https://sourceforge.net/projects/kdiff3/files/latest/download)
- Abrir el terminal y ejecutar los siguientes comandos:

```bash
git config --global --add merge.tool kdiff3
&&
git config --global --add mergetool.kdiff3.path "C:/Program Files/KDiff3/kdiff3.exe"
&&
git config --global --add mergetool.kdiff3.trustExitCode false
&&
git config --global --add diff.guitool kdiff3
&&
git config --global --add difftool.kdiff3.path "C:/Program Files/KDiff3/kdiff3.exe"
&&
git config --global --add difftool.kdiff3.trustExitCode false
```

Configurar Kdiff en Mac:

- Instalar Kdiff, puedes descargarlo [aquí](https://sourceforge.net/projects/kdiff3/files/latest/download)
- Abrir el terminal y ejecutar los siguientes comandos:

```bash
git config --global --add merge.tool kdiff3
&&
git config --global --add mergetool.kdiff3.path "Applications/kdiff3.app/Contents/MacOS/kdiff3"
&&
git config --global --add mergetool.kdiff3.trustExitCode false
&&
git config --global --add diff.guitool kdiff3
&&
git config --global --add difftool.kdiff3.path "Applications/kdiff3.app/Contents/MacOS/kdiff3"
&&
git config --global --add difftool.kdiff3.trustExitCode false
```

## Jugando con el estado staging

Cuando trabajamos localmente en nuestro repositorio, si quieres hacer commit a tus cambios, necesitas realizar los siguientes pasos: Primero añadir los ficheros al estado staging y luego hacer commit a esos ficheros.

¿Qué puedes hacer si has añadido a estado staging ficheros temporales? Lo puedes eliminar de este estado ejecutando el siguiente comando:

```bash
git reset HEAD <filepath>
```

![Git reset HEAD](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/git-staging.PNG)

Y... ¿si quiero eliminar todos los ficheros que estan en estado staging?

```bash
git reset HEAD .
```

Genial, pero ¿y si quiero descartar los cambios en un fichero que no esta en estado staging?

```bash
git checkout --
```

Y lo último pero no menos importante... descartar todos los cambios. Me gustaría empezar desde cero partiendo del commit anterior:

```bash
git checkout HEAD
```

![Git checkout HEAD](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/git-checkout-head.PNG)

## Fetch

Cuando trabajamos en un equipor, puede que existan ramas que han sido subidas al servidor y no estan disponibles en nuestro respositorio local, ¿cómo puedo descargarmelas? Ejecuta:

```bash
git fetch --all
```

De vez en cuando, hay ramas que no existen en el servidor remoto pero las sigues reflejando localmente, ¿cómo puedes hacer limpieza y eliminar esas referencias?, para hacer eso, ejecutamos:

```bash
git fetch --prune
```

## Pull

En ocasiones tenemos a más de una persona trabajando en la misma rama (o por ejemplo máster puede tener cambios de otros compañeros), ¿ Qué pasa si necesita actualizar mi rama activa porque hay cambios en el servidor? Si intentamos actualizarla usando **fetch** puede que no funcione porque podemos tener conflictos en la rama activa, _¿No existe un comando que se traiga los cambios de la rama activa e intente mezclarlos?_ Si, ese comando se llama **pull**.

```bash
git pull
```

## Stash

Algunas veces estas trabajando en tu codigo y no quieres hacer commit de los cambios, pero necesitas cambiar a otra rama, pero... para cambiar tienes que o hacer commit o descartar los cambios pendientes. ¿Hay alguna manera de poner los cambios "en borrador" y una vez que vuelvas recuperarlos? Eso es exactamente lo que hace el comando **stash**.

Vamos a crear una nueva rama y la llamamos _feature/saygoodbye_

```bash
git branch feature/saygoodbye
```

Cambiamos a esa rama

```bash
git checkout feature/saygoodbye
```

Modificamos algo en el código:

_./src/index.js_

```diff
const sampleNumber = 1;
const sampleNumberB = 2;
const sampleNumberC = 3;
console.log(`Hello number ${sampleNumber} {sampleNumberB} {sampleNumberC}`);
+ console.log('Goodbye !');
```

- No queremos hacer commit todavía, pero necesitamos cambiar a la rama master... ¿Cómo lo hacemos? Hacemos stash de nuestros cambios (todos los cambios serán almacenados localmente):

```bash
git stash
```

![Git stash](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/git-stash.PNG)

- Ahora podemos cambiar a la rama master

```bash
git checkout master
```

- Volvemos a la rama _feature/saygoodbye_.

```bash
git checkout feature/saygoobdye
```

- Y recuperamos el código que hemos almacenado en stash

```bash
git stash pop
```

![Git stash pop](https://github.com/Lemoncode/git-from-ui-to-terminal/raw/master/content/git-stash-pop.PNG)

> El comando stash almacena esta información localmente y se asocia al commit específico donde ha sido llamado.

## Otros trucos

A continuación se muestran algunas paginas donde podeis encontrar algunas trucos de git:

- https://i.redd.it/8341g68g1v7y.png
- https://www.git-tower.com/blog/git-cheat-sheet/
- https://github.github.com/training-kit/downloads/github-git-cheat-sheet.pdf

# Sobre Basefactor + Lemoncode

Somos un equipo innovador de expertos en Javascript, apasionados por convertir sus ideas en productos robustos.

[Basefactor, consultancy by Lemoncode](http://www.basefactor.com) proveedor de consultoria y servicios de coaching.

[Lemoncode](http://lemoncode.net/services/en/#en-home) proveedor de formación.

Disponemos de un master (castellano) en Front End, para mas información: http://lemoncode.net/master-frontend
