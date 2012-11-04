# Conclusiones

A continuación se presentarán conclusiones del presente trabajo, en respecto a la solución lograda, su efectividad, la metodología empleada, características que no se pudieron agregar (y las respectivas razones), y conclusiones para un trabajo futuro.

## Sobre la Solución

La solución que se presenta en este documento permite disminuir los tiempos de desarrollo de soluciones web, facilitando la tarea que más tiempo consume. El prototipado y construcción de vistas en aplicaciones de cualquier tipo siempre es la más tediosa, sobre todo en el ambiente web donde todo debe hacerse por código. Usando Switch IDE, se facilita gran parte de esta tarea proveyendo al desarrollador con componentes utilizados comúnmente en el desarrollo de aplicaciones, como botones, formularios, tablas, entre otras.

Sin embargo, posee algunas desventajas. Cada vez que se quiere probar algún cambio realizado en el programa, hay que reensamblar el proyecto, lo que toma tiempo. Esto, en todo caso, puede solucionarse y se discute en la Sección \ref{section:future-work} del presente capítulo. Otra desventaja que presenta la solución es que agregar lógica a los templates (como se hace normalmente en desarrollo web), es difícil. Por ejemplo, si se desea mostrar un botón sólo en caso de que alguna variable sea verdadera en el controlador, se debe agregar código a la vista y eso hace que el editor visual muestre este código. Soluciones a este problema se discuten también en la Sección \ref{section:future-work}.

## Sobre la Metodología

El haber dividido el proceso de construcción en dos fases permitió evaluar la efectividad de las herramientas escogidas antes de comenzar con la programación del aspecto más importante de la solución. La primera fase involucró la creación del backend de manera casi completa, el prototipado de la interfaz y la creación de una base para el desarrollo de la segunda etapa.

La división en dos fases fue acertada, y el orden del desarrollo de los diferentes componentes fue correcto, dado que de haber prototipado el editor de interfaces antes de comenzar con la base, se podría haber creado (accidentalmente) un editor completamente incompatible con la solución final. Ahora bien, el haber prototipado un editor de interfaces antes de comenzar con el desarrollo podría haber revelado dificultades que se podrían haber encontrado en la segunda etapa (aunque de todas formas se sabía que era posible desarrollar algo de esa índole dado que existían herramientas similares).

## Características que Faltaron

***NO SÉ SI AGREGAR ESTO EN LA MEMORIA***

La principal característica que se omitió fue la de poder "unir" elementos con acciones en el editor de vistas. Por ejemplo, al agregar un botón, la idea sería conectarlo con el controlador de manera que al hacer click se ejecutara una determinada acción. En Xcode es posible presionar `CTRL` y arrastrar el componente hacia el código, y automáticamente se genera una acción que se ejecutaría al presionar el componente. Ver Figura \ref{figure:xcode-ctrl-drag}.

![Al presionar la tecla `CTRL` y arrastrar, es posible crear métodos directamente. \label{figure:xcode-ctrl-drag}](figures/xcode-ctrl-drag.png)

Otro feature que no se agregó es el de archivar los proyectos. Archivar significa básicamente ensamblar el proyecto y dejarlo listo para su implementación en un servidor. La idea de esta característica es ensamblar y comprimir el proyecto en un archivo zip para su posterior descarga.

## Trabajo Futuro
\label{section:future-work}

Una de las características que sería ideal agregar a una futura versión es la de control de versiones con Git. Agregar esta característica no sería trivial pero tampoco sería extremadamente difícil dado que existen librerías que lo facilitarían. Agregar esta funcionalidad permitiría a los desarrolladores mantener su código versionado y además permitiría la colaboración, por medio de repositorios compartidos.

Agregar esta funcionalidad requeriría de interfaces que permitan al desarrollador seleccionar archivos para agregar al repositorio, crear nuevos "commits" (como se le conoce a los estados de código en versionamiento), descargar y subir cambios a repositorios remotos y poder (al menos) ver el historial del repositorio.

Otra característica importante que se discutió antes es la de no necesitar ensamblar el proyecto constantemente. Brunch (el ensamblador que se utilizó en este trabajo) permite levantar un proceso que observa cambios en los archivos y ensambla el proyecto bajo demanda. Eso hace que el proceso de programar y probar los cambios sea mucho más continuo y simple. ***Tal vez hablar más de esto?***

Un cambio que no sería menor, y que está relacionado con la edición de vistas, es la de cambiar el framework que se utiliza para el desarrollo por una conocida como Knockback^[referencia]. Knockback es una combinación de dos frameworks: Backbone (la que se utiliza actualmente) y Knockout. Knockout es conocido por ofrecer "bindings", es decir, permite agregar atributos HTML a las vistas de manera que se actualicen automáticamente, sin necesidad de escribir código dentro de ellas. Como por ejemplo:

```html
<p>First name: <strong data-bind="text: firstName"></strong></p>
<p>Last name: <strong data-bind="text: lastName"></strong></p>
```

El fragmento anterior une las etiquetas `<strong>` con los atributos `firstName` y `lastName` del modelo asociado. Lo ventajoso es que es simple HTML y además las uniones son en tiempo real, lo que significa que cualquier cambio al modelo ocurrirá en la vista sin requerir ningún esfuerzo extra por parte del desarrollador.

Esto mejoraría la experiencia del usuario al momento de diseñar vistas dado que no necesitaría escribir código (sólo HTML). Además, y más importante, esto permitiría mejorar el editor de interfaces de manera de que el usuario agregue "bindings" visualmente, con menús contextuales, sin editar el código fuente de la vista.

Agregar este framework requeriría cierto esfuerzo, aunque Brunch facilita esta tarea permitiendo crear "esqueletos" para proyectos con este framework. Exportar proyectos ya existentes no sería trivial, pero tampoco sería muy complejo, dado que Knockback simplemente combina ambos frameworks.