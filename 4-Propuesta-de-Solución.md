# Propuesta de Solución

\label{section:solution-proposal}

Para solucionar el problema identificado, se propone una herramienta web (es decir, que sea accesible desde el navegador, y no una aplicación nativa), que permita editar una aplicación web completa en el mismo navegador, junto con permitir al desarrollador crear templates de manera visual, en vez de utilizar sólo código. 

El editor de templates deberá permitir al usuario crear las interfaces que presentarían sus aplicaciones de manera visual, sin obligarlo a escribir todo en código. Mediante el arrastre de diferentes widgets prefabricados (como botones, formularios, ventanas modales, entre otros), el usuario podrá generar templates en un tiempo menor y con mayor facilidad que al hacerlo de la manera tradicional.

## Requisitos

Principalmente, la herramienta será un Entorno de Desarrollo Integrado compatible con Backbone^[ver estado del arte blabla] y deberá:

- Permitir al usuario desarrollar aplicaciones utilizando el framework Backbone^[será necesario especificar por qué?] y Twitter Bootstrap^[explicar o citar, no sé U_U]
- Dar una estructura a las aplicaciones web, evitando que el desarrollador programe la aplicación completa en un sólo archivo
- Facilitar la creación de vistas mediante un editor que permita arrastrar diferentes widgets a un "canvas"
- Permitir ensamblar y probar la aplicación de manera similar a cómo lo haría un programa nativo

Una herramienta con tales características no podrá funcionar sólo de lado de cliente (exclusivamente en el navegador), dado que algunas funciones tendrán que ser ejecutadas en el servidor, como por ejemplo la compilación de los archivos Javascript o levantar una instancia de un servidor para probar lo que el usuario esté desarrollando. Por esto, es necesario implementar este proyecto en dos componentes distintas.

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

### Elección de Herramientas para el Backend

Existen varias alternativas en cuanto a la creación de un backend que cumpla con los requisitos especificados anteriormente. Entre los lenguajes más populares para el desarrollo de servicios web se encuentran PHP, Java, Python y Ruby^[http://www.tiobe.com/index.php/content/paperinfo/tpci/index.html]. Para todos ellos existen una variedad de frameworks para desarrollar servicios web (como CodeIgniter, Play, Django o Ruby on Rails, respectivamente^[links!]). Además, todos permiten manipular archivos en el servidor, que es uno de los requisitos especificados para el backend.

Si bien el autor tiene conocimiento de todos los lenguajes mencionados, está más familiarizado con el último (Ruby). Además, para este lenguaje existen diferentes librerías que permitirían agregar funcionalidad extra en otra oportunidad (como Grit, una librería para manipular repositorios Git desde Ruby)^[link]. 

El backend no se programará directamente, sino que se utilizará un framework. Lo que se quiere desarrollar en el lado del backend es básicamente una API (Application Programming Interface), que el frontend utilizará parar funcionar correctamente. Existen varias formas de programar APIs en Ruby, donde las más populares son Ruby on Rails y Sinatra^[http://hotframeworks.com/languages/ruby].

Ruby on Rails es un framework extremadamente completo, y el más popular entre programadores de Ruby^[misma cita de antes]. Facilita una gran cantidad de tareas que son tediosas en otros frameworks, lo que la hace una herramienta ideal para todo tipo de proyectos web. Sin embargo, al ser una herramienta tan completa, agrega bastante overhead. Además, es una herramienta ideal para crear proyectos grandes y complejos, y dado que el backend de el presente trabajo no requerirá tanta complejidad, se convierte en una alternativa no tan ideal para este trabajo.

Sinatra es una versión simplificada de Ruby on Rails. Es un framework más simple y liviano, por lo que inicia más rápido y, en general, se desempeña mejor que su contraparte. Tiene la desventaja de no poseer tantas facilidades para desarrollar sitios complejos, pero dado que el backend de este proyecto no requerirá tanto trabajo, se considera como la mejor opción y se utilizará para desarrollar el backend.

El backend requerirá una base de datos para poder guardar información de los usuarios y proyectos. Se consideraron dos alternativas: MySQL^[link] y MongoDB^[link]. La primera es una base de datos convencional y la más popular^[http://www.mysql.com/why-mysql/marketshare/]. La segunda es una base de datos de la familia conocida como "NoSQL", es decir, no es una base de datos relacional. Permite almacenar "documentos", o, más específicamente, objetos del estilo JSON^[link?]. Además, no acepta consultas SQL, si no que las consultas se realizan utilizando métodos directamente en la base de datos.

Las ventajas que tiene MongoDB sobre MySQL son varias, entre algunas: configurar escalamiento horizontal es relativamente sencillo, y permite almacenar archivos en él usando una tecnología llamada GridFS^[http://www.mongodb.org/display/DOCS/GridFS].

En términos de uso, existen librerías en Ruby para interactuar con ambas bases de datos. Para MySQL, es posible usar ActiveRecord^[link!], un ORM^[Object Relational Mapper] muy popular y muy poderoso que permite hacer consultas a la base de datos sin utilizar SQL. Para MongoDB, existe Mongoid^[link!], un ODM^[Object Document Mapper] que provee una interfaz muy similar a ActiveRecord.

Las ventajas que posee MongoDB por sobre MySQL hacen de ella una mejor opción, dado que de ser necesario, podría migrarse el almacenamiento de archivos a la base de datos y configurar escalamiento horizontal más fácilmente que de utilizar MySQL. De todas formas, en caso de querer cambiar el sistema de base de datos, no se requerirían muchos cambios dadas las similitudes de las librerías que existen.

Por último, como ya se describió antes, el backend deberá encargarse de crear y estructurar proyectos. Para esto, existe una utilidad llamada Brunch^[poner referencia a esto!]. Brunch es un ensamblador de aplicaciones ("application assembler" en inglés). Básicamente, basándose en una plantilla, organiza aplicaciones en carpetas. Soporta diferentes frameworks y lenguajes, desde Backbone a Knockout^[referencia!], usando Javascript o Coffeescript^[referencia]. Además de permitir estructurar los proyectos, los ensambla, es decir, toma todos los archivos y los junta en uno sólo de manera de optimizar la aplicación cuando esté en producción. Utilizar esta herramienta en el backend permitirá crear y mantener proyectos mucho más fácilmente que si se hiciera de forma manual

### Elección de Herramientas para el Frontend

Para el desarrollo de aplicaciones de lado de cliente existen varios frameworks, entre ellos Backbone, Knockout y Ember^[links]. Todos ellos poseen sus ventajas y desventajas. Por ejemplo, Knockout provee bindings entre los modelos y las interfaces, mientras que Backbone es un framework enfocado en simplicidad y rapidez. De hecho, en ***citararticulo***^[https://paydirtapp.com/blog/backbone-in-practice-memory-management-and-event-bindings/] se menciona que incluyendo comentarios, posee tan sólo 1400 líneas de código.

Considerando que la solución propuesta permitirá al usuario desarrollar aplicaciones utilizando Backbone, además de que este framework se enfoca en simplicidad, se prefirió frente a las otras alternativas

En cuanto a la interfaz, dado que también se encuentra en los requisitos permitir prototipar templates usando Twitter Bootstrap, se prefirió inclinarse por este framework. Es un conjunto de widgets extremadamente popular (es el repositorio más popular en GitHub^[https://github.com/popular/starred]) y muy completo, proveyendo de todo tipo de widgets que podrían encontrarse en herramientas como Visual Studio o Xcode.

Ahora, Backbone es un framework que no da una estructura a las aplicaciones que se desarrollan con él. A diferencia de, por ejemplo, Ruby on Rails, la tarea de estructurar la aplicación en carpetas o módulos depende completamente del desarrollador. En este punto se asemeja mucho más a Sinatra que a Ruby on Rails. Esto podría considerarse una desventaja, dado que sistemas complejos tienden a crecer y desordenarse bastante si no se aplica una estructura desde un principio. 

Por las razones antes descritas, es que se decidió utilizar Brunch. Esta herramienta ya se describió anteriormente, y permitirá facilitar el desarrollo del frontend. Es más, incluye un servidor web de desarrollo, que detecta cambios en los archivos y reensambla todo el sitio de manera de poder hacer pruebas.

En lo que respecta la decisión de lenguaje de programación, no hay muchas alternativas. Es posible desarrollar la solución usando Javascript o Coffeescript. Existen otras alternativas (como TypeScript de Microsoft^[http://www.typescriptlang.org/]), pero al momento de escribir este documento no se encuentran en etapas estables de desarrollo ni madurez. Javascript es un lenguaje poderoso pero a la vez de relativo bajo nivel. Ciertas cosas son un tanto tediosas de programar, como recorrer arreglos por ejemplo. En cambio, Coffeescript, un lenguaje de programación escrito por Jeremy Ashkenas que apareció en el 2009 ^[https://github.com/jashkenas/coffee-script/commit/8e9d637985d2dc9b44922076ad54ffef7fa8e9c2] que compila a Javascript, hace que desarrollar en esta plataforma sea mucho más cómodo y simple, sin perder desempeño ni funcionalidad (pues compila directamente a Javascript). Además, hace muchísimo más fácil programar "orientado a objetos". Si bien ECMAScript (la base de Javascript) está definido como orientado a objetos ^[http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf página 1], su estilo es diferente al resto de los lenguajes. Coffeescript agrega palabras clave como `class` y `extends` de manera de facilitar escribir clases y manipular objetos en este lenguaje.

Por ejemplo, escribir una clase en Javascript podría llegar a esto:

```javascript
var ViewController, viewController,
  __hasProp = {}.hasOwnProperty,
  __extends = function(child, parent) { 
    for (var key in parent) { 
      if (__hasProp.call(parent, key)) child[key] = parent[key]; 
    } 
    function ctor() { 
      this.constructor = child; 
    } 
    ctor.prototype = parent.prototype; 
    child.prototype = new ctor(); 
    child.__super__ = parent.prototype; 
    return child; 
  };

viewController = ViewController = (function(_super) {

  __extends(ViewController, _super);

  function ViewController() {
    return ViewController.__super__.constructor.apply(this, arguments);
  }

  ViewController.prototype.initialize = function() {
    return this.name = "Switch IDE";
  };

  return ViewController;

})(View);
```

En cambio, con CoffeeScript, se resume a lo siguiente:

```coffeescript
viewController = class ViewController extends View
  initialize: ->
    @name = "Switch IDE"
```
