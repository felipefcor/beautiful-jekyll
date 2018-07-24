---
layout: post
title: Como aprender a hacer debugging
tags: javascript debugging 
---
## [Artículo de Julie Pagano](http://juliepagano.com/blog/2014/05/18/javascript-debugging-for-beginners/)
## [**Como aprender JavaScript -  Debugging - Pretraining Step 5**](http://MentoringJS.com)
Tutorial muy práctico de como hacer _debugging_ en javascript a través de navegadores web.

He hecho caso a la autora y he abierto varias ventanas de mi navegador para ir probando las cosas que comentaba.

- La primera herramienta que aparece es _alert_. Parece que ha sido importante en el pasado pero actualmente se ha quedado en desuso. Se utilizaba sobre todo para ver si se llegaba a una parte del código. Y además, una vez se llegaba a este _alert_, la ejecución del código se paraba.

```
// I want to know if I reach this part of the code.
alert("I am here!");
```

- Lo siguiente son los _developer tools_. Desde el propio navegador puedes lanzar código en la consola y hacer gran cantidad de comprobaciones y _debugging_.
Es muy interesante que haga ejercicios en "vivo" donde al pulsar un botón te aparece en la propia consola el error. Muy práctico.

Me sorprende las consulta a CSS, ya que no había visto antes esta opción.
```
// You can look up elements via css selectors
$$('h2');
// Or xpath
$x('//h2');
```
- Otro caso parecido a _alert_ es _console.log_ que simplemente lanza el mensaje por consola. En este caso, el programa continúa y no se para como con _alert_.
Otra diferencia es que _console.log_ permite sacar todo tipo de datos, mientras que _alert_ solo permite _strings_.

```
// I want to know if I reach this part of the code.
console.log("I am here!");
```

- Otra herramienta que comenta es _debugger_. Esta es una herramienta interactiva para hacer _debugging_ de código más complejo. Es interesante porque te permite interaccionar directamente con el código no desde la consola sino que se abre como una especie de consola nueva a toda página. Esta herramienta quizá es más difícil de utilizar para un _begginer_, por la complejidad que aporta.

```
// I want to start debugging here.
debugger;

```

- Librerías y código minimizado
En ciertos casos también es importante hacer _debugging_ de las librerías que se utilizan (en el tutorial utiliza Jquery), aunque no es habitual ya que dichas librerías vienen revisadas por sus creadores. En estos casos, para hacer este trabajo es mejor trabajar con las librerías sin minimizar ya que el código es más legible y tiene comentarios; mientras que para producción sí que es recomendable utilizar las librerías minimizadas(_jquery.min.js_)

- La última herramienta que muestra es como hacer _debugging_ de peticiones Ajax. Esta parte la he encontrado un poco más compleja ya que esta parte de Javascript aun no la he tocado demasiado.

- Para acabar, hace unas menciones a cómo hacer _debugging_ para _performance_ de Javascript y también para móviles. En ambos casos
<br>

Una cosa importante que creo que se debe remarcar es que en cada herramienta la autora pone enlaces a las webs de los _developers tools_ de [Chrome](https://developers.google.com/web/tools/chrome-devtools/?utm_source=dcc&utm_medium=redirect&utm_campaign=2016q3), que es la que utiliza ella para el tutorial. Ahí se puede profundizar en los conceptos y herramientas.

---
