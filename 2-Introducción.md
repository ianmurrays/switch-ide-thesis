# Introducción

<!-- Start numbering with arabic numbers now -->

\pagenumbering{arabic}

<!-- This numbering thing has to go here in order for it to work ok, otherwise it just stays with roman numbers -->

En el contexto del desarrollo de aplicaciones web, existen dos grandes "corrientes". Por una parte, es posible desarrollar aplicaciones completamente de lado de servidor. Esto significa que la aplicación procesa todos los datos y genera todo lo que el usuario ve de forma remota. Esta forma de desarrollar ha sido así durante muchos años y sigue siendo una forma muy utilizada. Por otra parte, últimamente, con el crecimiento de la comunidad de Javascript, y la constante mejora en rendimiento de los navegadores modernos, se ha popularizado la idea de llevar gran parte de la lógica de negocio y el procesamiento de datos al cliente. Los navegadores modernos tienen cada vez más capacidad de ejecutar procesos rápida y eficientemente, lo que tiene varias ventajas, como por ejemplo, se aliviana la carga en los servidores, lo que permite poder soportar a muchos más usuarios simultáneos, y las aplicaciones se desempeñan mucho mejor, dado que se disminuye el retardo que hay en transmitir datos entre el cliente y el servidor. Esto último es muy importante al momento de crear aplicaciones web. Si bien toda página web, sin importar su naturaleza, debería ser lo más rápida y responsiva posible, las aplicaciones web deben serlo por sobre todo. Al fin y al cabo, están intentando imitar el comportamiento de aplicaciones nativas, pero a su vez alivianando la carga a la que se somete un desarrollador al momento de crear aplicaciones compatibles con una infinidad de dispositivos y sistemas operativos distintos.

Desarrollar aplicaciones web versus desarrollar aplicaciones nativas (que son ejecutables directamente en el computador, sin necesidad de un navegador) tiene varias ventajas, siendo quizás la más importante que no es necesario escribir el programa para diferentes plataformas, dado que la mayoría de los navegadores modernos funcionan en una gran variedad de dispositivos. Además, con el crecimiento del estándar HTML5 (que al momento de escribir el presente documento aún se encuentra en proceso de convertirse en un estándar final), es posible aprovechar muchas características de los dispositivos (en algunos casos incluso es posible usar los acelerómetros de los teléfonos móviles inteligentes). Es más, muchos juegos han sido portados a la web, utilizando WebGL y tecnologías similares.

Ahora bien, desarrollar aplicaciones web también tiene sus desventajas. Es cosa de ver Xcode o Visual Studio, donde ambas herramientas son un entorno completamente integrado para desarrollar aplicaciones. Desde escribir código a crear formularios y diferentes tipos de vistas, ambas herramientas (y otras similares) entregan una experiencia casi inigualable al desarrollador. Al desarrollar aplicaciones y sitios web en general, no es posible encontrar herramientas que se asemejen lo suficiente a las mencionadas (y que sean de código abierto) como para considerarse una alternativa viable. La mayoría de los entornos de desarrollo para web permiten previsualizar lo que el desarrollador codifica, pero no le permiten ahorrar tiempo al momento de realizar tareas tan necesarias como codificar la interfaz de una aplicación.

Es este último aspecto el que se considera como un problema actualmente en el mundo del desarrollo web. Si bien no es difícil codificar interfaces de usuario al momento de crear aplicaciones web, es una tarea que consume tiempo y para la cual sí existen herramientas muy buenas en el mundo del desarrollo de aplicaciones nativas (como las ya mencionadas Xcode o Visual Studio). 

## Objetivos

En este trabajo se propone la creación de un entorno de desarrollo integrado, al que se le llamará **Switch IDE**, que permita el desarrollo de aplicaciones web facilitando la creación de interfaces de manera similar a las herramientas descritas anteriormente.

## Estructura de este Trabajo

El trabajo se estructurará como sigue:

- En el Capítulo \ref{section:state-of-the-art} se revisará primero el estado del arte, analizando las herramientas que actualmente intentan dar solución al problema identificado, además de frameworks y otro tipo de utilidades que mitigan de cierta forma el problema pero sin darle una completa solución.
- Luego, en el Capítulo \ref{section:solution-proposal} se propondrá una solución al problema identificado.
- Se construirá y documentará la creación de la solución planteada en el Capítulo \ref{section:construction}.
- Se analizarán los resultados utilizando métricas que se describirán en el Capítulo \ref{sections:results}.
- Finalmente, en el Capítulo \ref{section:conclusion} se presentarán conclusiones del trabajo junto con ideas para posible trabajo futuro.