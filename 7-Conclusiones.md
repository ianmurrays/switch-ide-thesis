# Conclusiones

\label{section:conclusion}

A continuación se presentarán conclusiones del presente trabajo, con respecto a la solución lograda y conclusiones para trabajo futuro.

## Sobre la Solución

Se cree que la solución que se presenta en este documento podría ayudar a disminuir los tiempos de desarrollo de aplicaciones web, en cierta medida facilitando la tarea que más tiempo consume. Usando Switch IDE, se podría facilitar gran parte de esta tarea proveyendo al desarrollador con widgets utilizados comúnmente en el desarrollo de aplicaciones, como botones, formularios, tablas, entre otras.

<!--
## Sobre la Metodología

El haber dividido el proceso de construcción en dos fases permitió evaluar la efectividad de las herramientas escogidas antes de comenzar con la programación del aspecto más importante de la solución. La primera fase involucró la creación del backend de manera casi completa, el prototipado de la interfaz y la creación de una base para el desarrollo de la segunda etapa.

La división en dos fases fue acertada, y el orden del desarrollo de los diferentes componentes fue correcto, dado que de haber prototipado el editor de interfaces antes de comenzar con la base, se podría haber creado (accidentalmente) un editor completamente incompatible con la solución final. Ahora bien, el haber prototipado un editor de interfaces antes de comenzar con el desarrollo podría haber revelado dificultades que se podrían haber encontrado en la segunda etapa (aunque de todas formas se sabía que era posible desarrollar algo de esa índole dado que existían herramientas similares).
-->

## Sobre la Elección de Herramientas

Se cree que la elección de herramientas para el backend fue bastante acertada, pues no presentaron problemas durante el desarrollo del proyecto

<!--
Haber elegido Ruby y Sinatra para construir el backend fue una buena decisión. Permitió crear un backend simple, liviano y mantenible en poco tiempo. 

***YA Y?***
-->

En cuanto al frontend, en un principio se escogió Backbone para construir el frontend por su simplicidad, popularidad y por ser un framework liviano. Resultó ser una herramienta flexible y simple de usar, con una documentación^[http://backbonejs.org/] adecuada que permitió desarrollar con ella sin mayores problemas.

<!--
En cuanto al frontend, En un principio se escogió Backbone para construir el frontend por su simplicidad, popularidad y por ser un framework liviano. Hubo que considerar que el autor no poseía conocimientos con ninguno de estos frameworks inicialmente, por lo que la elección se vio sesgada hacia una herramienta fácil de aprender y con buena documentación y soporte (o sea, una comunidad activa).

Backbone probó ser un framework fácil de dominar y muy flexible. La presencia de varias librerías y extensa documentación en línea permitió dominar la herramienta en poco tiempo y crear una solución mantenible y legible. 
-->

## Trabajo Futuro
\label{section:future-work}

Una característica que podría disminuir los tiempos de prototipado sería la de poder "unir" elementos con acciones en el editor de vistas. Por ejemplo, al agregar un botón, el desarrollador podría conectarlo con el controlador de manera que al hacer click se ejecutara una determinada acción. Por ejemplo, en Xcode[@xcode] es posible presionar `CTRL` y arrastrar el componente hacia el código, generéndose una acción que se ejecutaría al presionar el componente (ver Figura \ref{figure:xcode-ctrl-drag}).

![En Xcode, al presionar la tecla `CTRL` y arrastrar un widget, es posible crear métodos directamente. \label{figure:xcode-ctrl-drag}](figures/xcode-ctrl-drag.png)

Una de las características que podría facilitar el trabajo de un desarrollador con esta herramienta es control de versiones con Git. Agregar esta característica no sería trivial, pero tampoco sería extremadamente difícil dado que existen librerías que lo facilitarían. Agregar esta funcionalidad permitiría a los desarrolladores mantener su código versionado y además permitiría la colaboración, por medio de repositorios compartidos. Agregar esta funcionalidad requeriría de interfaces que permitan al desarrollador seleccionar archivos para agregar al repositorio, crear nuevos "commits" (como se le conoce a los estados de código en versionamiento), descargar y subir cambios a repositorios remotos y poder ver el historial del repositorio.

Otra característica importante es la de no necesitar ensamblar el proyecto constantemente. Brunch (el ensamblador que se utilizó en este trabajo) permite levantar un proceso que observa cambios en los archivos y ensambla el proyecto bajo demanda. De poseer esta característica en la solución, el proceso de programar y probar los cambios podría tornarse más continuo y simple. Sin embargo, esto requeriría cambiar la forma en la que se levanta el servidor de pruebas, de manera de mantenerlo siempre en línea.

Una adición que se cree agilizaría el trabajo en proyectos con archivos grandes es la de no enviar el archivo completo cada vez que se guarden cambios. Este es el comportamiento actual y, si bien no se nota para archivos livianos, sí se sentiría al momento de guardar archivos relativamente grandes. Una posible solución sería enviar parches, es decir, guardar en el frontend el estado en el que se encuentra el archivo en el servidor, y, al momento de guardar cambios, enviar sólo las diferencias, optimizando la cantidad de información enviada. Por ejemplo, si se tuviera un archivo con 200 líneas de código y sólo se quisiera agregar 2 líneas con comentarios, sólo se tendría que transmitir aproximadamente un 1% del archivo.

En cuando a optimizaciones, si bien Javascript es un lenguaje con recolección de basura y el desarrollador no debería preocuparse del manejo de memoria, en Backbone es muy fácil comenzar a fugar ("leak", en inglés) objetos a memoria. Esto es porque la recolección de basura sólo elimina objetos de memoria cuando ya no existen referencias a éste. En Backbone, al haber tantos objetos dependiendo de otros y escuchando notificaciones de otros, aun cuando se haya eliminado una vista del documento, el objeto seguirá escuchando eventos de otros objetos y seguirá en memoria. Siendo este trabajo un proyecto de Backbone relativamente grande, la posibilidad de empezar a fugar objetos es alta. Existen algunas guías para mejorar el rendimiento de aplicaciones escritas en Backbone como la escrita por la empresa Paydirt[@paydirt-article], que convendría seguir y aplicar en este proyecto y que se cree ayudarían a mejorar su rendimiento.

En cuando a la edición de templates, en la solución propuesta no es posible cambiar propiedades de los diferentes widgets visualmente (como por ejemplo cambiar el color a un botón, o cambiar ciertas propiedades de una tabla). Se propone agregar un editor de propiedades que permita cambiar diferentes atributos de algún componente seleccionado, de manera de evitar que el desarrollador deba editar HTML directamente.

En la misma línea, se cree que cambiar el framework que se utiliza para el desarrollo por una conocida como Knockback[@knockback] podría facilitar el trabajo del desarrollador aún más al editar templates. Knockback es una combinación de dos frameworks: Backbone (la que se utiliza actualmente) y Knockout. Knockout es conocido por ofrecer "bindings", es decir, permite agregar atributos HTML a las vistas de manera que se actualicen automáticamente, sin necesidad de escribir código dentro de ellas. Como por ejemplo:

```html
<p>First name: <strong data-bind="text: firstName"></strong></p>
<p>Last name: <strong data-bind="text: lastName"></strong></p>
```

El fragmento anterior une las etiquetas `<strong>` con los atributos `firstName` y `lastName` del modelo asociado. Lo ventajoso es que es simple HTML y además las uniones son en tiempo real, lo que significa que cualquier cambio al modelo ocurrirá en la vista sin requerir ningún esfuerzo extra por parte del desarrollador.

Esto podría mejorar la experiencia del desarrollador al momento de diseñar vistas dado que no necesitaría escribir código (sólo HTML). Además, y más importante, esto permitiría mejorar el editor de interfaces de manera de que el usuario agregue "bindings" visualmente, como por ejemplo con menús contextuales, sin editar el código fuente de la vista.

Agregar este framework podría significar un esfuerzo mayor, aunque Brunch facilita esta tarea permitiendo crear "esqueletos" para proyectos con este framework. Exportar proyectos ya existentes no sería trivial, pero tampoco sería muy complejo, dado que Knockback simplemente combina ambos frameworks.