---
layout: post
title: React desde dentro. Componentes, estructura y características
tags: [javascript, React, componentes, UI]
---
### [Aprende Javascript con MentoringJS - Step 12 ](http://MentoringJS.com)
En este artículo voy a intentar explicar ciertos aspectos de React que he ido aprendiendo durante este tiempo. Para ello, me voy a centrar en varios artículos que me han servido para afianzar las bases de React.

- [React Practice Components](https://daveceddia.com/react-practice-components/)
- [How to structure your React project](https://daveceddia.com/react-project-structure/)
- [React Aha moments](https://tylermcginnis.com/react-aha-moments/)

<br>

Del [primer artículo](https://daveceddia.com/react-practice-components/) se puede extraer una lógica basada en Componentes a la hora de programar en React.

Esta lógica sigue ciertas directivas que hay que poner en práctica:

- **Dividir la UI en componentes pequeños y sencillos**

Para realizar esto, una opción es realizar prácticas de componentes simulando las partes de UI de Facebook o Twitter. En esos casos, se debe hacer un trabajo de separación de contenidos bien mentalmente o bien dibujándolo. Una vez hecho eso, se pueden pasar props con el contenido que deberían tener.

<img width="400" height="100" src="https://daveceddia.com/react-practice-components/card-simple.png">

- **Hacer uso de litas de arrays**

En React, al contrario que otras librerías o frameworks, para iterar un array se puede hacer solo con JavaScript. Además, este tipo de iteraciones son muy comunes para mostrar listas de cosas en tus UI con React.

- **Componer con _children_**

Pasar contenido entre componentes es en React más fácil que en otras librerías. Para ello, se puede pasar contenido a través de un componente anidado como prop "children". Esto permite construcciones de diferentes tipos de UI, como tablas, listas, etc.

----

El artículo anterior explica ciertas directivas para empezar a programar en React y seguir unas buenas prácticas desde bien principio. Pero, al hacerse más complejo el proyecto pueden surgir dudas sobre cómo estructurar el árbol de directorios en tu dispositivo.

Esto es lo que se trata en el [segundo artículo](https://daveceddia.com/react-project-structure/) el cuál me ha parecido muy interesante y práctico.

Para empezar, el autor del artículo, [Dave Ceddia](https://daveceddia.com/), nos explica que no hay una manera única de estructurar los proyectos aunque sí que hay ciertos lugares comunes. Él nos explica su manera de hacer y la lógica que tiene; además, considera que hay algunos puntos que hacen decantar la balanza hacia su manera de hacer las cosas.

- **Componentes Presentacionales vs Containers**

Esta [idea de Dan Abramov](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0) es la que guía la base de su estructura.

La diferencia entre estos dos tipos de componentes es, _grosso modo_, que los componentes presentacionales podrían ser los más enfocados a la parte visual de tus UI mientras que los containers serían los que trabajarían más con los datos. Otra diferencia sería que los primeros no tendrían estado y los segundos sí (normalmente).

Con esta distinción se consigue una mejor separación de aspectos (_concerns_) a tratar por cada componente y una mejor reusabilidad de los mismos.

El autor del artículo utiliza esta diferenciación para estructurar sus proyectos. Su propuesta sería la siguiente:

- Dentro de la carpeta src (que es donde van todos los archivos) añade dos carpetas, una llamada componentes (para los presentacionales) y otra containers.

- Además, añade un archivo (o carpeta dependiendo de los archivos que hagan falta) para las conexiones con las apis.

- También pone una carpeta para las imágenes.

- El archivo index.js va en este mismo nivel de jerarquía.

- Por último, añade una carpeta cajón de sastre llamada _utils_.

<img width="300" height="300" src="https://daveceddia.com/images/suggested-structure.png">

Esta es la estructura que recomienda el autor, por su sencillez y funcionalidad.
Personalmente, me parece una buena estructura y que cumple los objetivos que se le piden.

En el artículo también se habla de configuración de los _paths_ para los _imports_ en tus archivos y que eso se puede configurar u personalizar en el archivo package.json. Tanto para hacer _imports_ personalizados como para hacer imports para tus tests. Al final también comenta un añadido de estructura si en tu proyecto utilizas Redux.


---


Cuando ya estás de lleno metido en el mundo React puedes sentirte algo abrumado por la cantidad de temas y contenido que se tiene que dominar para crear aplicaciones. Para intentar superar este estado lo aconsejable es meterte en el barro y hacer aplicaciones en React. Empezar con aplicaciones sencillas y poco a poco ir aumentando la complejidad. Pero, además, también es muy importante ir leyendo contenido que otras personas han ido creando en su propio camino de aprendizaje. Este sería el caso del último artículo que voy a comentar, que habla de los [momentos Ahá en React](https://tylermcginnis.com/react-aha-moments/).

En este artículo el autor desarrolla ciertos puntos que cree que, al dominarlos, maximizarían los posibles momentos _ahá_ en React. Estos momentos serían esos momentos de lucidez en los que, de repente, todo tiene sentido y empiezas a entender el tema tratado.


- **Las UI son la Vista que devuelve una función que acepta varios datos: _(fn(d)=V)_**

Esta idea resume el punto de vista del autor, Tyler McGinnis, sobre las interficies de usuario (UIs).

React sigue la misma lógica que Javascript con las funciones, pero en vez de aceptar parámetros aceptan props. Y en vez de devolver un valor, devuelve una representación de tu UI (una Vista).


- **Las UI en React se crean con composición de funciones y JSX**

JSX es más que un lenguaje _markup_ tipo HTML.

JSX, en primer lugar, es una abstracción de una función que devuelve un objeto representado del DOM. Después de esto, React es capaz de comprobar en el DOM si algo ha cambiado y, si ha sido así, renderizarlo al DOM directamente.

En segundo lugar, como JSX es JavaScript, éste tiene todos los beneficios del segundo, es decir: la composición, el _linting_, el _debugging_, etc., y también el comportamiento declarativo de HTML.


- **Los componentes no se tienen que corresponder con los nodos del DOM**

La idea general que se tiene sobre los componentes en React es que reciben un input y devuelven algo de UI. Pero esta idea no es ajustada, según el autor, ya que existen muchos tipos de componentes que no se comportan así y no por ello dejan de ser componentes. Por ejemplo, los que reciben datos de sonidos o los que devuelven otros componentes o funciones.

Por todo ello, no hay que cerrarse a esta idea que nos encierra en un solo comportamiento sino tener la mente más abierta y practicar todo tipo de creaciones con los componentes React.


- **Estado compartido entre componentes**

En React existe una situación compleja cuando un estado es compartido por varios componentes. Desde el principio de React no estaba claro dónde poner este estado. Esta situación hizo pensar a la comunidad React y finalmente desembocó en la creación de Redux. De todos modos, en React también se puede resolver esta situación buscando al padre más cercano a los dos componentes que comparten estado y que el padre controle el estado compartido. Tanto esta solución como la ofrecida por Redux tiene pros y contras.


- **En React no es necesaria la herencia, con la composición es suficiente**

React siempre se ha acercado a la programación funcional desde bien principio. Desde versiones tempranas dejó de dar soporte los _Mixins_ y se decantó por la composición. Con la composición se puede conseguir los mismos objetivos que con la herencia (y _Mixins_).


- **Componentes presentacionales y containers**

Como en el anterior artículo, en este se hace mención a esta distinción original de Dan Abramov.

Las diferencias más grandes entre estos dos tipos de componentes serían:

- Los componentes presentacionales reciben datos a través de _props_ y son responsables de como las cosas se ven. Los containers, en cambio, tienen estado, ciclo de vida y son responsables de como las cosas funcionan.

- Esta distinción permite una mejor reusabilidad de los componentes presentacionales.

- También ayuda a entender mejor la estructura de la aplicación.

- Permite cambiar la implementación de un componente sin preocuparse de la UI.

- El estado es inconsistente por naturaleza, por lo que separando los componentes se logra encapsular la complejidad en un componente específico sin que afecte a otros.


---
