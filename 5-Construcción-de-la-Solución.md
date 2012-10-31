# Construcción de la Solución

Este capítulo tiene por objetivo detallar todo el proceso del diseño y desarrollo de la solución propuesta anteriormente. Se dividirá en los siguiente subcapítulos:

  a. Diseño de la solución: básicamente se explicará cómo ambas componentes (frontend y backend) interactuarán entre sí. Además, cómo funcionarán ambas partes en términos de manipulación de archivos y el proyecto completo. Por último, se explicará cómo se diseñó el componente principal de la solución (el editor de interfaces).
  b. Flujo del usuario: ***EXPLICAR ESTO, SON BÁSICAMENTE CASOS DE USO***
  c. Primera etapa de construcción: el desarrollo de la solución se dividió en dos etapas principalmente. Primero, se desarrolló lo que se denominó una "base" de la solución. Esta etapa contempló el desarrollo de gran parte del backend y, en el frontend, una herramienta que permitiera crear proyectos nuevos, crear, editar y eliminar archivos, compilar y correr el proyecto. 
  d. Primera etapa de construcción: la segunda parte del desarrollo se enfocó en desarrollar y perfeccionar el editor de interfaces. Dado que este componente es el grueso de la solución, se decidió dedicar una etapa completa a él.

## Diseño de la Solución

### Backend

La solución es una aplicación mayoritariamente de lado de cliente, por lo que la mayor cantidad de lógica debe ir en este lado. Por esta razón, se diseñó el servidor de la forma más simple posible. Las tareas principales que tiene el servidor o backend de la aplicación son:

  - Autentificar usuarios
  - Generar proyectos utilizando Brunch
  - Manipular los archivos. Esto incluye crear, eliminar, renombrar y actualizar archivos (o sea, recibir el contenido de ellos y guardarlo a disco)
  - Ensamblar el proyecto
  - Levantar un servidor estático que permita al usuario probar su proyecto

Algunas de estas tareas son realizadas por la utilidad Brunch, por lo que sólo es necesario hacer que el servidor ejecute un comando en la consola para llevarlas a cabo. El resto de las tareas son básicamente manipulación de archivos, para lo cual cualquier lenguaje de programación trae funciones o librerías (y Ruby no es ninguna excepción).

Se decidió utilizar una aproximación a lo que es REST^[explicar esto] para los servicios que proveerá el backend. En REST, la idea es representar objetos, y exponer diferentes métodos para tales objetos. En este caso, se decidió que deben existir 3 objetos diferentes: usuarios, proyectos y archivos. 

  - Usuarios: dado que la idea es que varios usuarios puedan utilizar el sistema a la vez, debe existir esta entidad en el servidor (y por ende en la base de datos).
  - Proyectos: es casi la base de todo. Cada proyecto contendrá los diferentes archivos y carpetas, y pertenecerá a un usuario.
  - Archivos: esto se refiere a archivos y carpetas. Por la forma en la que se manipulan los archivos en el servidor y en el cliente, es más conveniente manejarlos de (casi) la misma forma. Esto último se refiere a la forma en la que se obtienen los archivos en el servidor, y las acciones que se realizan en ellos. Ambos archivos y carpetas se crean y eliminan, como también se renombran. La única diferencia substancial es que las carpetas no tienen contenido y no se actualizan como el resto de los archivos.

El servidor autentificará a los usuarios utilizando sus cuentas de GitHub y OAuth. OAuth es un protocolo de autentificación y autorización que permite al usuario registrarse e ingresar a la aplicación con un sólo click  (dos si ingresa por primera vez). Dado que esta es una herramienta enfocada a programadores, y considerando que GitHub es una plataforma conocida por cualquier desarrollador que se mantenga al día, utilizar este sistema para autentificar a los usuarios es simple y conveniente. Por el lado de servidor, basta con incluir una librería que redirige a los usuarios a las URLs específicas y crear un registro en la base de datos si es que el usuario está ingresando por primera vez.

### Frontend

El frontend tendrá dos tareas principalmente. Primero, deberá permitir a los usuarios autentificarse. Para esto, y para efectos del presente trabajo, será una simple página web que redirija al usuario a GitHub para autentificarse usando este sistema. Segundo, deberá proveer al usuario con la IDE que se quiere construir en este documento.

***EXPLICAR DE QUÉ TRATA LA PÁGINA DE AUTENTIFICACIÓN A GRANDES RASGOS, SCREENSHOTS Y TODO***

La IDE misma se dividirá en tres componentes principales: la barra lateral izquierda para explorar los archivos del proyecto, la barra lateral derecha con componentes visuales (como botones, formularios, etc.), y la sección central que contendrá el código del archivo actualmente seleccionado o la vista previa en caso de estar editando una vista.

Contará además con una barra superior con un menú (al igual que cualquier aplicación de escritorio), pero dado que proveerá simples accesos directos a funciones que se explicarán más adelante no se detallará su diseño ni implementación.

En los siguientes párrafos se pretende explicar qué objetos existirán en el frontend, sus responsabilidades y cómo interactuarán entre ellos. Primero se definirán algunos conceptos necesarios para entender de qué tipos de objetos se estará hablando.

**Modelo:** Representa un objeto en un proyecto, como por ejemplo un archivo, una carpeta o el proyecto mismo. Cada modelo es responsable de persistir su estado de alguna forma (comunicándose con un servidor o almacenando datos en el mismo navegador).

**Colección:** Es básicamente una lista de instancias de un tipo de modelo. Por ejemplo, una carpeta podría considerarse una colección de archivos (siendo cada archivo una instancia de un modelo).

**Vista:** Una vista en Backbone es un archivo que se encarga de presentar información al usuario, y además de interactuar con él, por ejemplo ejecutando funciones cuando se haga un click en un botón. Una vista por lo general presenta un modelo (o una colección). Por ejemplo, se puede tener una vista para cada instancia de un archivo, o bien se pueden tener vistas que no presenten a ningún modelo en particular.

**Template:** Un template es un trozo de HTML que una vista utiliza para generar lo que el usuario ve. Si bien no son enteramente necesarias y una vista podría generar todo lo que necesita con Javascript, hacen la tarea algo más fácil.

A continuación se explicará a grandes rasgos los modelos y colecciones que existirán en el frontend. Se tendrán modelos para los proyectos y los archivos. Cada uno se encargará de comunicarse con el backend para obtener los datos que le sean necesarios o bien para guardar los cambios. 

**El modelo de proyecto** guardará el nombre de éste y una referencia a una colección de los archivos que se encuentren en la raíz de su carpeta. Tendrá como responsabilidades crear archivos y carpetas, ensamblar el proyecto e iniciar el servidor de pruebas. Estas acciones se complementan con llamadas al backend que realizan las tareas mismas.

**El modelo de archivo** se encargará de guardar el nombre y el contenido (en caso de que corresponda) del archivo o carpeta al que representa, además de guardar una referencia al proyecto al que pertenece y . Tendrá como responabilidades pedir su contenido, actualizarlo, renombrar y eliminar el archivo del sistema. Todas estas acciones se complementan además con llamadas al backend.

**La colección de archivos** tendrá como responsabilidad ordenar las listas de archivos una vez que la haya obtenido (además de guardar una referencia a cada modelo de archivo que le corresponda). Ordenar las listas de archios es importante pues el backend arroja una lista de archivos ordenada alfabéticamente, pero, dado que archivos y directorios son considerados de la misma forma en el backend, es necesario ordenar la lista de manera que los directorios queden arriba. Esto facilita encontrar archivos para el usuario.

En lo que respecta a las vistas, se tendrán las siguientes:

**Explorador de Archivos:** se colocará en la barra lateral izquierda, y tendrá dos listas de archivos. Una lista de archivos actualmente abiertos y la lista de archivos y directorios en el proyecto. Tendrá entre sus responsabilidades mantener una lista de archivos que se encuentran actualmente abiertos para que el usuario pueda navegar entre ellos.

**Archivo:** esta vista representará a un archivo en la vista anterior (exporador de archivos). Mostrará su nombre y un icono que represente si es un directorio, un archivo o una vista editable con el editor que se construirá. Entre sus responsabilidades están abrir los archivos (o sea, abrir el archivo en el editor de código o en el editor de vistas en caso que corresponda), embeber listas de archivos en caso de que se clickee un directorio y permitir al usuario renombrar archivos, mostrando un menú contextual.

**Editor de Texto:** esta vista contendrá el editor de archivos de texto (editor de código). Sus responsabilidades serán mostrar un editor con resaltado de sintaxis y modificar el modelo de archivo que corresponda para guardar cambios.

**Editor de Vistas:** esta vista mostrará templates y, en una barra lateral derecha, diferentes componentes para que el usuario los arrastre y agregue. Contará además con un editor de código HTML, en caso de que el usuario quiera editar la vista o realizar cambios que el editor no permita directamente. Tiene las mismas responsabilidades que el editor de texto.

## Flujo del Usuario ("Casos de Uso")

A continuación se detallarán algunos de los casos de uso que tiene la aplicación.

### Caso de Uso: El usuario ingresa a la aplicación

### Caso de Uso: El usuario crea un proyecto nuevo

### Caso de Uso: El usuario abre un proyecto existente

## Primera Etapa de Construcción

### Creación del Entorno de Trabajo

De la misma forma en que Switch utilizará Brunch para crear y administrar proyectos, se decidió utilizar la misma solución para construir la IDE. Brunch utiliza un sistema de esqueletos ("skeletons", en inglés), los cuales utiliza para crear la estructura de los proyectos. Existe una gran variedad, utilizando diferentes lenguajes y frameworks. Se encontró uno que utiliza Backbone y CoffeeScript y se utilizó para crear la estructura de archivos inicial.

### Prototipado de la Interfaz

Se comenzó por prototipar la interfaz principal. Como ya se ha dicho, se utilizó Twitter Bootstrap, lo que permitió simplificar considerablemente esta etapa. Para realizar el prototipado se requirió realizar un poco de programación, pues hubo que crear vistas y rutas para ir testeando los casos de uso definidos anteriormente. La programación fue mínima de todas formas, enfocando esta etapa en prototipado y no en funcionalidad.

Se prototipó un menú superior con diferentes opciones de manera similar a los menú que se ven en diferentes IDE y programas de escritorio. Se incluyeron opciones como crear un nuevo proyecto, un nuevo archivo, ensamblar y ejecutar el proyecto, etc.

Se agregó la barra lateral izquierda, en la que se muestra una lista de los archivos abiertos y una lista de los archivos en el proyecto. Las carpetas cuentan con un ícono que las muestra como tal, mientras que archivos de vistas tienen un ícono que las diferencia de las demás. ***PONER ACA QUE EN LA FIGURA X SE VE BLABLABLA***

Para el editor de código central se utilizó CodeMirror^[citar esto]. Esto permitió embeber un editor de código muy extensible y completo en muy poco tiempo. El editor utiliza gran parte de la pantalla pues, siendo lo más esencial, debe dársele más espacio. ***SUENA MUY MAL ESTO :(***

En el caso del editor de vistas, se agregó un "canvas" (básicamente un espacio en el cual arrastrar los componentes que se mencionarán más adelante), y una barra lateral derecha, que sólo es visible al estar editando un archivo que la requirera. Además del canvas, en la parte superior se agregaron dos "tabs", que permitirán al usuario cambiar entre el canvas y un editor de código para la vista. En la barra lateral estarán los diferentes componentes en una lista que mostrará una pequeña vista previa del componente y su nombre.

Otras vistas corresponden a el selector de proyectos, que se creó usando una ventana modal (que Twitter Bootstrap trae consigo). Ésta se mostraría en el momento que el usuario ingrese al programa. Ahí, podrá elegir algún proyecto en el que haya estado trabajando o crear uno nuevo directamente.

### Creación de Servicios en el Backend

Para poder crear las funcionalidades necesarias en el frontend, se decidió crear primero los servicios en el Backend. Éstos son los siguientes:

  - Creación de proyectos
  - Obtención de una lista de proyectos
  - Obtención de lista de archivos
  - Operaciones CRUD en cada archivo y carpeta (incluyendo actualizar el contenido de archivos)
  - Ensamblar y correr el proyecto para pruebas

Se consideraron dos acercamientos para la creación del backend. Primero, crear un modelo para los proyectos y modelos para los archivos (que podrían o no almacenarse en una base de datos). Segundo, crear un único modelo para el proyecto y dejar que éste maneje cada archivo utilizando su ruta en disco.

La primera alternativa se descartó dado que agregaba un cierto grado de complejidad sin agregar ningún beneficio. La segunda alternativa es más simple, pues considerando que los archivos y carpetas son literalmente archivos y carpetas en disco, no es necesario agregar una capa de abstracción dado que no simplificaría su manejo. Es por esto que cada proyecto tiene un modelo (y su información sí se guarda en la base de datos), y es posible manipular archivos y carpetas en el proyecto utilizando métodos en cada instancia (junto con la ruta al archivo o directorio).

Ahora, si bien sólo existirá un modelo para el proyecto, en términos de la interfaz que proveerá el backend, si se reflejará una diferencia entre archivos y proyectos. Específicamente, se tendrán las siguientes rutas para realizar diferentes acciones:

  - `GET /projects`: entregará una lista de proyectos existentes
  - `GET /projects/:id/files`: entregará una lista de archivos en la raíz del proyecto
  - `GET /projects/:id/files?path=/ruta/al/directorio`: lista los archivos en el directorio especificado
  - ***etc ...***

Aún cuando proyectos y archivos se manejarán en el mismo modelo, se dividirán en dos controladores, a manera de encapsular en cierta medida su funcionalidad.

La creación de toda la funcionalidad ya descrita requirió utilizar varias funciones de manipulación de archivos y ejecución de comando en Ruby.

***EXPLICAR ACÁ ALGUNAS IMPLEMENTACIONES***

### Agregado de Funcionalidad al Prototipo del Frontend

Se comenzó por implementar el selector de proyectos. Para esto fue necesario  implementar el modelo y colección de proyectos. En esta etapa fueron implementados de la forma más simple posible. Básicamente, se crearon como dos clases que extienden a `Backbone.Model` y `Backbone.Collection` respectivamente. Dado que Backbone está diseñado para interactuar con APIs REST, no fue necesario configurar más que la URL del backend y especificar que la colección corresponde a una colección de Proyectos.

***PONER EJEMPLOS DE CÓDIGO ACÁ***

Luego de configurar el modelo y la colección, se agregó una ruta al enrutador de Backbone. El enrutador lee la URL del navegador e interpreta qué método llamar del enrutador. La idea es que se especifiquen las URL necesarias para la navegación de la aplicación. En el caso de esta solución, sólo existirán dos rutas principalmente. La primera será la ruta base, donde se cargará el selector de proyectos del cual se habla, y la segunda será la que tendrá cada proyecto. Por lo tanto, se configuró la URL base, y, en el método que es llamado, se inicializa la colección de proyectos.

En este punto, la colección es pasada a una vista. Las vistas, como se explicó antes, son las encargadas de presentar datos al usuario (por medio de templates). Esta vista, básicamente, presenta una lista de proyectos y un formulario para crear uno nuevo (esta última funcionalidad se creará en la segunda etapa). 

***AGREGAR SCREENSHOT DE CREAR UN PROYECTO***

Al usuario hacer click en un proyecto existente, se llama a la segunda ruta ya mencionada. Esta ruta, toma el identificador de proyecto de la url (que tienen un formato estilo `proyects/IDENTIFICADOR`) e instancia un proyecto usando ese identificador. Una vez que se haya obtenido toda su información desde el servidor, se le asigna el modelo al proyecto (de manera que su instancia quede compartida) y se inicializa el visor de archivos (que es una vista) con éste.

En este paso, se instancia el visor de archivos. El visor de archivos toma el modelo de proyecto y "pide" su carpeta raíz. El modelo hace una petición al backend, y éste contesta con una representación de cada archivo (o carpeta) del directorio raíz. A continuación un ejemplo de la respuesta del servidor:

```
JSON ACA OH SI
```

El visor de archivos, instancia una vista de archivo por cada uno de los documentos que responde el backend. La vista de archivo se encarga de mostrar el nombre y tipo de archivo, y permitir al usuario clickearlo para abrirlo o ver su contenido. En caso de que el archivo se trate de un directorio, la vista de archivo se encarga de mostrar sus contenidos. Esta parte resultó ser un poco complicada de implementar, dado que lo que se necesita es básicamente mostrar el mismo tipo de vista que la raíz del proyecto pero para un subproyecto. Lo que se hizo en este caso es lo siguiente: cuando el usuario hace click en un directorio, se instancia una colleción de archivos con la ruta a ese directorio. Se hace una petición al servidor para que entregue la lista de archivos en esa carpeta y se instancian vistas de archivo para cada uno, embebiéndolas en la misma vista del directorio que se acaba de clickear. En la Figura \ref{figure:file-browser} se puede apreciar el concepto. La carpeta `app` es una vista de archivo, y contiene todos los archivos y carpetas dentro de su recuadro. Lo mismo pasa con el directorio `models`.

![El visor de archivos: cada vista de directorio contiene más vistas de archivos de ser necesarias. \label{figure:file-browser}](figures/file-browser.png)

En caso de que el usuario haga click en un archivo, se le pasa la instancia del modelo al editor de código, el cual indica al modelo que debe pedir el contenido del archivo al servidor para mostrarlo. El editor de código es simplemente una librería llamada CodeMirror que permite al usuario editar el contenido de los archivos. El editor de código se encarga además de indicarle al modelo del archivo que guarde su contenido en el backend si el usuario lo solicita.

***NO SE SI EXPLICAR MÁS DE LO ANTERIOR***

Para esta etapa de la construcción, se incluyó además la posibilidad de ensamblar y ejecutar el proyecto. Para esto, se agregaron métodos en el modelo de proyectos que hace llamadas al backend para ensamblar y ejectuar el servidor de pruebas. El evento es manejado por la barra de navegación, que incluye varios elementos de menú que por esta etapa se mantuvieron inactivos. A la derecha de la barra de navegación se encuentra un botón que permite ensamblar y ejecutar el proyecto con un sólo click, y otros dos que permiten ejecutarlo y ensamblarlo por separado.


## Segunda Etapa de Construcción


[^backbone-colection]: En Backbone, existen modelos y, a su vez, colecciones de modelos. Las colecciones son básicamente un conjunto de instancias de un tipo de modelo. Por ejemplo, en el caso de este trabajo, existirán colecciones de *archivos*.