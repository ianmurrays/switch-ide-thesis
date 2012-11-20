# Propuesta de Solución

\label{section:solution-proposal}

Para solucionar el problema identificado, se propone crear Switch IDE, una herramienta web (es decir, que sea accesible desde el navegador, y no una aplicación nativa), que permita editar una aplicación web completa en el mismo navegador, junto con permitir al desarrollador crear templates de manera visual, en vez de utilizar sólo código. 

El editor de templates deberá permitir al usuario crear las interfaces que presentarían sus aplicaciones de manera visual, sin obligarlo a escribir todo en código. Mediante el arrastre de diferentes widgets prefabricados (como botones, formularios, ventanas modales, entre otros), el usuario podrá generar templates en un tiempo menor y con mayor facilidad que al hacerlo de la manera tradicional.

## Requisitos

Principalmente, la herramienta será un Entorno de Desarrollo Integrado compatible con Backbone (ver Sección \ref{sections:backbone}) y deberá:

- Permitir al usuario desarrollar aplicaciones utilizando el framework Backbone y Twitter Bootstrap. Se escogió Backbone por ser un framework simple y extensible, y por ser usado por varios sitios (ver Sección \ref{sections:backbone}). Se escogió Twitter Bootstrap[@twitter-bootstrap-github] por su alta popularidad[@bootstrap-popularity] y variedad de widgets.
- Dar una estructura a las aplicaciones web, evitando que el desarrollador programe la aplicación completa en un sólo archivo
- Facilitar la creación de vistas mediante un editor que permita arrastrar diferentes widgets a un "canvas"
- Permitir ensamblar y probar la aplicación de manera similar a cómo lo haría un programa nativo

Una herramienta con tales características no podrá funcionar sólo de lado de cliente (exclusivamente en el navegador), dado que algunas funciones tendrán que ser ejecutadas en el servidor, como por ejemplo la compilación de los archivos Javascript o levantar una instancia de un servidor para probar lo que el usuario esté desarrollando. Por esto, es necesario implementar este proyecto en dos componentes distintas. Estas dos componentes serán el backend y el backend, siendo el backend el sistema que "apoye" al frontend, como se explica en la sección siguiente, y el frontend la interfaz misma, lo que el desarrollador verá de Switch IDE.

A continuación, se especificará qué roles cumplirán backend y frontend en la herramienta propuesta, junto con sus requisitos específicos.

### Definición y Requisitos del Backend

El backend será un servidor que permitirá al frontend realizar las tareas que no le serán posibles por diferentes limitaciones. Por ejemplo, los sitios web no pueden manipular archivos del sistema operativo por motivos de seguridad, por lo que es imposible realizar todo en el frontend. Por lo tanto, el backend cumplirá con las siguientes tareas:

- Deberá generar los proyectos y sus estructuras (directorios y archivos base), junto con almacenarlos, dado que no es posible almacenarlos en el cliente
- Manipular los archivos, es decir, crear, eliminar, renombrar y actualizar archivos y carpetas
- Ensamblar los proyectos
- Levantar servidores que permitan al desarrollador hacer pruebas

Es importante dejar claro que el usuario nunca deberá interactuar con el backend directamente, será el frontend el que interactúe con éste y el usuario no verá esta interacción.

### Definición y Requisitos del Frontend

El frontend será el encargado presentarle al desarrollador todo lo que necesite, como mostrar los archivos en un proyecto, mostrar un editor de código o el editor de templates propuesto.

Específiamente, el frontend deberá permitir al desarrollador:

- Elegir entre diferentes proyectos existentes
- Crear proyectos nuevos
- Explorar los diferentes directorios de un proyecto abierto
- Crear, renombrar y eliminar archivos y carpetas en un proyecto
- Editar archivos, mediante un editor de código con resaltado de sintaxis
- Editar visualmente los templates, listando diferentes widgets que el desarrollador podrá arrastrar a un "canvas" que se actualizará en tiempo real mostrando una vista previa del template
- Ensamblar y probar el proyecto

## Elección de Herramientas

En esta sección se discutirán las diferentes alternativas de herramientas para desarrollar ambos el backend y el frontend, y qué herramientas se escogieron finalmente.

### Elección de Herramientas para el Backend

Existen varias alternativas en cuanto a la creación de un backend que cumpla con los requisitos especificados anteriormente. Entre los lenguajes más populares para el desarrollo de servicios web se encuentran PHP, Java, Python y Ruby[@tiobe]. Para todos ellos existen una variedad de frameworks para desarrollar servicios web (como CodeIgniter[@codeigniter], Play[@play], Django[@django] o Ruby on Rails[@rubyonrails], respectivamente). Además, todos permiten manipular archivos en el servidor, que es uno de los requisitos especificados para el backend.

Si bien el autor tiene conocimiento de todos los lenguajes mencionados, está más familiarizado con el último (Ruby). Además, para este lenguaje existen diferentes librerías que permitirían agregar funcionalidad extra en otra oportunidad (como Grit, una librería para manipular repositorios Git desde Ruby [@grit]). 

Lo que se quiere desarrollar en el lado del backend es básicamente una API (Application Programming Interface) que el frontend utilizará parar funcionar correctamente. Existen varias formas de programar APIs en Ruby, y las más populares son Ruby on Rails y Sinatra [@ruby-framework-popularity].

Ruby on Rails es un framework extremadamente completo, y el más popular entre programadores de Ruby. Facilita una gran cantidad de tareas que son tediosas en otros frameworks, lo que la hace una herramienta ideal para todo tipo de proyectos web. Sin embargo, al ser una herramienta tan completa, agrega bastante overhead. Además, es una herramienta ideal para crear proyectos grandes y complejos, y dado que el backend de el presente trabajo no requerirá tanta complejidad, se convierte en una alternativa no tan ideal para este trabajo.

Sinatra es una versión simplificada de Ruby on Rails. Es un framework más simple y liviano, por lo que inicia más rápido y, en general, se desempeña mejor que su contraparte. Tiene la desventaja de no poseer tantas facilidades para desarrollar sitios complejos, pero dado que el backend de este proyecto no requerirá tanto trabajo, se considera como la mejor opción y se utilizará para desarrollar el backend.

El backend requerirá una base de datos para poder guardar información de los usuarios y proyectos. Se consideraron dos alternativas: MySQL[@mysql] y MongoDB[@mongodb]. La primera es una base de datos convencional y la más popular[@mysql-marketshare]. La segunda es una base de datos no relacional. Permite almacenar "documentos", o, más específicamente, objetos del estilo JSON[@json]. Además, no acepta consultas SQL, si no que las consultas se realizan utilizando métodos directamente en la base de datos.

Las ventajas que tiene MongoDB sobre MySQL son varias, entre algunas: configurar escalamiento horizontal es relativamente sencillo, y permite almacenar archivos en él usando una tecnología llamada GridFS[@mongodb-gridfs].

En términos de uso, existen librerías en Ruby para interactuar con ambas bases de datos. Para MySQL, es posible usar ActiveRecord[@activerecord], un ORM^[Object Relational Mapper: librería que permite convertir datos en una base de datos a objetos.] muy popular y muy poderoso que permite hacer consultas a la base de datos sin utilizar SQL. Para MongoDB, existe Mongoid[@mongoid], un ODM^[Object Document Mapper: librería que permite convertir documentos en una base de datos basada en documentos (como MongoDB) a objetos.] que provee una interfaz muy similar a ActiveRecord.

Las ventajas que posee MongoDB por sobre MySQL hacen de ella una mejor opción, dado que de ser necesario, podría migrarse el almacenamiento de archivos a la base de datos y configurar escalamiento horizontal^[Escalamiento Horizontal en MongoDB: permite distribuir los datos almacenados en la base de datos en diferentes instancias de MongoDB, efectivamente distribuyendo la carga a la base de datos entre diferentes máquinas. http://docs.mongodb.org/manual/core/sharding/#sharding-overview\ref{referencias}] más fácilmente que de utilizar MySQL. De todas formas, en caso de querer cambiar el sistema de base de datos, no se requerirían muchos cambios dadas las similitudes de las librerías que existen.

Por último, como ya se describió antes, el backend deberá encargarse de crear y estructurar proyectos. Para esto, existe una utilidad llamada Brunch[@brunch]. Brunch es un ensamblador de aplicaciones ("application assembler" en inglés). Básicamente, basándose en una plantilla, organiza aplicaciones en carpetas. Soporta diferentes frameworks y lenguajes, desde Backbone a Knockout[@knockout], usando Javascript o Coffeescript[@coffeescript]. Además de permitir estructurar los proyectos, los ensambla, es decir, toma todos los archivos y los junta en uno sólo de manera de optimizar la aplicación cuando esté en producción. Utilizar esta herramienta en el backend permitirá crear y mantener proyectos mucho más fácilmente que si se hiciera de forma manual

### Elección de Herramientas para el Frontend

Para el desarrollo de aplicaciones de lado de cliente existen varios frameworks, entre ellos Backbone, Knockout y Ember[@emberjs]. Todos ellos poseen sus ventajas y desventajas. Por ejemplo, Knockout provee bindings entre los modelos y las interfaces, mientras que Backbone es un framework enfocado en simplicidad y rapidez. De hecho, en [@paydirt-article] se menciona que incluyendo comentarios, posee tan sólo 1400 líneas de código.

Considerando que la solución propuesta permitirá al usuario desarrollar aplicaciones utilizando Backbone, además de que este framework se enfoca en simplicidad, se prefirió frente a las otras alternativas

En cuanto a la interfaz, se prefirió utilizar Twitter Bootstrap, dado que se encuentra en los requisitos permitir prototipar templates usando este framework. Es un conjunto de widgets y un framework de prototipado en HTML (es decir, no sólo trae widgets, sino que permite organizarlos y estructurarlos), es extremadamente popular (es el repositorio más popular en GitHub[@bootstrap-popularity]) y es muy completo, proveyendo de todo tipo de widgets que podrían encontrarse en herramientas como Visual Studio o Xcode.

Ahora, Backbone es un framework que no da una estructura a las aplicaciones que se desarrollan con él. A diferencia de, por ejemplo, Ruby on Rails, la tarea de estructurar la aplicación en carpetas o módulos depende completamente del desarrollador. En este punto se asemeja mucho más a Sinatra que a Ruby on Rails. Esto podría considerarse una desventaja, dado que sistemas complejos tienden a crecer y desordenarse bastante si no se aplica una estructura desde un principio. 

Por las razones antes descritas, es que se decidió utilizar Brunch. Esta herramienta ya se describió anteriormente, y permitirá facilitar el desarrollo del frontend. Es más, incluye un servidor web de desarrollo, que detecta cambios en los archivos y reensambla todo el sitio de manera de poder hacer pruebas.

En lo que respecta la decisión de lenguaje de programación, no hay muchas alternativas. Es posible desarrollar la solución usando Javascript o Coffeescript. Existen otras alternativas (como TypeScript de Microsoft[@typescript]), pero al momento de escribir este documento no se encuentran en etapas estables de desarrollo ni madurez. Javascript es un lenguaje poderoso pero a la vez de relativo bajo nivel. Ciertas cosas son un tanto tediosas de programar, como recorrer arreglos por ejemplo. En cambio, Coffeescript, un lenguaje de programación escrito por Jeremy Ashkenas que apareció en el 2009 [@coffeescript] y que compila a Javascript, hace que desarrollar en esta plataforma sea mucho más cómodo y simple, sin perder desempeño ni funcionalidad (pues compila directamente a Javascript). Además, Javascript es un lenguaje orientado a objetos en base a prototipos, a diferencia de otros lenguajes (como Ruby o Java) que son basados en la definición de clases. Coffeescript abstrae el prototipado de objetos en Javascript permitiendo escribir clases de una forma más simplificada.

A modo de ejemplo (y uno muy sencillo), podemos ver que para crear una clase que permita extraer la primera palabra de una cadena de texto en Javascript quedaría como sigue:

```javascript
var TextUtilities;
TextUtilities = (function() {
  function TextUtilities() {}
  TextUtilities.prototype.initialize = function(string) {
    return this.string = string;
  };
  TextUtilities.prototype.firstWord = function() {
    return this.string.split(" ")[0];
  };
  return TextUtilities;
})();
```

En cambio, con CoffeeScript, lo mismo se resume a lo siguiente:

```coffeescript
class TextUtilities
  initialize: (string) -> @string = string
  firstWord: -> @string.split(" ")[0]
```
