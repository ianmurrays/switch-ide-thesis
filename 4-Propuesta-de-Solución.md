# Propuesta de Solución

Para solucionar el problema identificado, se propone una herramienta web, basada en *Backbone* y *Ruby*, que permita editar una aplicación web completa en el mismo navegador. Sus principales características serían:

- Dar una estructura a las aplicaciones que se desarrollen con Backbone
- Facilitar la creación de vistas mediante un editor que permita arrastrar los diferentes componentes y ahorrar el tiempo gastado en ello
- Implementar atajos de teclado de manera similar a cómo lo haría una IDE de escritorio
- Permitir ensamblar y probar la aplicación de manera similar a cómo lo haría un programa nativo

La herramienta no puede funcionar sólo de lado de cliente (exclusivamente en el navegador), dado que algunas funciones deben ser ejecutadas en el servidor, como por ejemplo la compilación de los archivos Javascript y levantar una instancia de un servidor para probar lo que el usuario esté desarrollando. Por esto, es necesario implementar este proyecto en dos partes distintas, pero dependientes.

<!--Salto de linea aca??-->

Por un lado, se tiene el servidor, que tendrá la tarea de manipular los archivos de cada proyecto que el usuario cree y desarrolle. Además, tendrá la tarea de ejecutar ciertos comandos necesarios para la creación y prueba de los proyectos, que simplemente no es posible ejecutar en el navegador. El hecho de que exista un "backend" (como se le llamará de ahora en adelante), permite además dar lugar a futuras características imposibles de llevar a cabo de otra forma, como por ejemplo soporte para repositorios Git, que deben ser manejados en el servidor exclusivamente.

Por otro lado, está el cliente. El cliente es básicamente el "frontend" (como se le llamará de ahora en adelante). Es la interfaz gráfica y es lo que interactúa con el usuario, en el navegador. La mayor parte del trabajo recaerá en esta parte, dado que es lo que el usuario ve y utiliza para trabajar. Además, es la parte del proyecto que contendrá el editor de vistas, lo que requerirá un esfuerzo no mínimo para funcionar.

## Elección de Herramientas para el Backend

Para desarrollar el backend se escogió el lenguaje de programación *Ruby*. Las razones para la elección de este lenguaje son varias: la familiaridad que tiene el autor con éste; su simplicidad para desarrollar tareas relativamente complejas en otros lenguajes, y la infinidad de librerías disponibles para facilitar diferentes tareas (como por ejemplo *Grit*, una librería para manejar repositorios Git directo desde Ruby)^[cita?].

Para desarrollar el backend no se utilizará Ruby puro. Lo que se quiere desarrollar en el lado del backend es básicamente una API (Application Programming Interface), que el frontend utilizará parar funcionar correctamente. Existen varias formas de programar APIs en Ruby, donde las más populares son Ruby on Rails y Sinatra. 

Ruby on Rails ha comenzado a ser muy popular en estos últimos años por ser un framework extremadamente completo. Facilita enormemente una infinidad de tareas que lo hacen una herramienta ideal para todo tipo de proyectos web. Sin embargo, al ser una herramienta tan completa, agrega bastante *overhead*^[cómo defino esto?]. Además, es una herramienta ideal para crear proyectos grandes y muy complejos, y dado que el backend de el presente trabajo no requerirá tanta complejidad, se convierte en una alternativa no tan ideal para este trabajo.

Sinatra podría considerarse el hermano menor de Ruby on Rails. Es un framework muchísimo más simple, y, por ende, mucho más liviano. Inicia más rápido y, en general, se desempeña mejor que su contraparte. Tiene la desventaja de no poseer tantas facilidades para desarrollar sitios complejos y grandes, pero dado que el backend de este proyecto no requerirá tanto trabajo, es la opción ideal.

El backend requerirá una base de datos para poder guardar información de los usuarios y proyectos. Se pensó en dos alternativas: MySQL y MongoDB. La primera es una base de datos convencional, poderosa y muy popular. La segunda es una base de datos de la familia conocida como "NoSQL", es decir, no es una base de datos tradicional. Permite almacenar "documentos", o, más específicamente, objetos del estilo JSON. Además, no acepta consultas SQL, si no que las consultas se realizan utilizando métodos directamente en la base de datos.

Las ventajas que tiene MongoDB sobre MySQL son varias: es un sistema de bases de datos rápido y liviano, soporta escalamiento horizontal fácilmente, e incluso permite almacenar archivos en él usando una tecnología llamada GridFS^[http://www.mongodb.org/display/DOCS/GridFS].

En términos de uso, existen librerías en Ruby para interactuar con ambas bases de datos. Para MySQL, es posible usar ActiveRecord^[link!], un ORM^[Object Relational Mapper] muy popular y muy poderoso que permite hacer consultas a la base de datos sin utilizar SQL. Para MongoDB, existe Mongoid^[link!], un ODM^[Object Document Mapper] que provee una interfaz muy similar a ActiveRecord.

Dadas las ventajas que provee MongoDB y la posibilidad de utilizar un ODM muy similar a ActiveRecord, se decidió utilizar MongoDB. Afortunadamente, en caso de querer cambiar el sistema de base de datos, no se requerirían muchos cambios dadas las similitudes de las librerías utilizadas.

## Elección de Herramientas para el Frontend

Dado que la solución que se propone es una herramienta para desarrollar aplicaciones en Backbone utilizando Twitter Bootstrap, lo lógico es desarrollar esta solución usando las mismas herramientas. Se escogió Backbone dada su alta popularidad. Esto asegura que el framework es bastante sólido y que existen variadas librerías y herramientas estables para él. Por otro lado, si bien Backbone es un framework poderoso, es bien simple, lo que da más libertad sobre como afrontar diferentes problemas.

Ahora, Backbone es un framework que no da una estructura a las aplicaciones que se desarrollan con él. A diferencia de Ruby on Rails, por ejemplo, la tarea de estructurar la aplicación en carpetas o módulos depende completamente del desarrollador. En este punto se asemeja mucho más a Sinatra que a Ruby on Rails. Esto puede considerarse una desventaja, dado que sistemas complejos tienden a crecer y desordenarse bastante si no se aplica una estructura desde un principio. Por esto, es que se decidió utilizar Brunch^[poner referencia a esto!]. Brunch es un ensamblador de aplicaciones ("application assembler" en inglés). Básicamente, basándose en un esqueleto, organiza aplicaciones en carpetas. Soporta diferentes frameworks y lenguajes, desde Backbone a Knockout^[referencia!], usando Javascript o Coffeescript^[referencia]. Además de entregar una estructura, las ensambla, es decir, toma todos los archivos y los junta en uno sólo de manera de optimizar la aplicación cuando esté en producción. Es más, incluye un servidor web de desarrollo, que detecta cambios en los archivos y reensambla todo el sitio de manera de poder hacer pruebas más rápida y fácilmente.

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

Por último, para el diseño y estructura de la interfaz, se decidió utilizar *Twitter Bootstrap*. Bootstrap es un conjunto de componentes y un sistema de estructurado para páginas web, que hace la tarea de crear y diseñar un sitio muy simple. Trae consigo una enorme cantidad de componentes (botones, barras de progreso, etc.), además de facilitar el diseño de formularios y componentes similares muy comunes al momento de crear aplicaciones web. No sólo se utilizará Twitter Bootstrap para crear el frontend, sino que también se utilizará para el diseñador de interfaces, dado que tiene todas las ventajas descritas anteriormente.
