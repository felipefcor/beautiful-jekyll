---
layout: post
title: Cómo modificar el DOM con Javascript
---

###  **[Aprende Javascript con MentoringJS - Step 7](http://MentoringJS.com)**

Modificar el DOM es un conocimiento básico y un conocimiento muy potente para un programador frontend.

>Antes de nada, voy a explicar brevemente qué es el DOM.

>El DOM es lo que conecta las páginas web con scripts o lenguajes de programación, en este caso con Javascript. El DOM es una interfaz de programación para los documentos HTML y XML que facilita una representación estructurada del documento y define de qué manera los programas pueden acceder, al fin de modificar, tanto su estructura, estilo y contenido.
Para más información se puede acudir [aquí](https://developer.mozilla.org/es/docs/Referencia_DOM_de_Gecko/Introducci%C3%B3n).

En esta entrada voy a traducir y resumir un [artículo muy interesante dónde se detalla como alterar el DOM con Javascript](https://zellwk.com/blog/js-in-dom/).

¿Qué se hace con el DOM?

Cuando se trabaja con el DOM, te encuentras con la necesidad de hacer una o más de las siguientes cosas:

+ **Seleccionar elementos HTML**
+ **Añadir y eliminar _event listeners_**
+ **Añadir y eliminar clases**
+ **Añadir, cambiar y eliminar atributos**
+ **Añadir y eliminar elementos HTML**

<br>

#### **1. Seleccionando elementos HTML**

Conocer como se seleccionan elementos HTML es el primer paso antes de hacer cualquier otra cosa con el DOM. Para ello, solo es necesario conocer dos métodos: _querySelector_ y _querySelectorAll_.

**querySelector**

_querySelector_ ayuda a seleccionar un elemento HTML.

```
document.querySelector(selector)

```
Se puede seleccionar un elemento por su id, clase o incluso por la etiqueta querySelector.

Teniendo el siguiente código:

```
<div id="primero">ID</div>
<div class="clase-increible">Class</div>
<p>A tag</p>
```

Para seleccionar el elemento por su id, se añade antes del id un #
```
document.querySelector('#primero').
```
Para seleccionar el elemento por su clase, se añade antes de la clase un .
```
document.querySelector('.clase-increible').
```
Para seleccionar el elemento por su etiqueta, simplemente se escribe la etiqueta como el selector.
```
document.querySelector('p')
```

_querySelector_ es muy poderoso y también permite hacer selecciones complicadas encandenando ids, clases, etc., aunque no es del todo recomendable hacerlo ya que normalmente es innecesario. También se puede seleccionar elementos dentro de elementos, con lo que se reduce el tiempo necesario para buscar un elemento profundo. Para hacer eso, tienes que añadir un espacio entre las clases, ids o etiquetas.
Tenemos el código siguiente:
```
<div class="container">
  <div class="item-profundo">Inner item!</div>
</div>
```
Y seleccionamos el elemento dentro del elemento con:
```
let innerItem = document.querySelector('.container .item-profundo')
```
De manera alternativa, si ya has seleccionado un elemento con _querySelector_ también puedes usar ese elemento para utilizar otra llamada con _querySelector_:
```
let container = document.querySelector('.container')
let itemProfundo = container.querySelector('.item-profundo')
```


**querySelectorAll**


_querySelectorAll_ es un método que te ayuda a seleccionar múltiples elementos.
```
let allELements = document.querySelectorAll(selectors);
```
selectors, en este caso, tiene la misma sintaxis que _querySelector_. La única excepción es que se puede realizar múltiples selecciones separándolas con una coma (,).

```
<div class="cosa">Una cosa</div>
<div class="cosa">Otra cosa</div>
<div class="otra-cosa">Y otra cosa</div>
```
Aquí está la parte importante.


_querySelectorAll_ devuelve un [NodeList](https://developer.mozilla.org/en-US/docs/Web/API/NodeList)
Si estás trabajando con navegadores modernos, puedes utilizar elementos individuales con la instrucción
```
Nodelist.forEach
let todasCosas = document.querySelectorAll('.cosa, .otra-cosa')
todasCosas.forEach(el => {/* hacer algo con el elemento */})
```
Si estás trabajando con navegadores antiguos, necesitas convertir el NodeList en un Array antes de pasarlo por el bucle forEach. La manera más sencilla de hacerlo es usando _Array.from ()_.
```
let todasCosas = document.querySelectorAll('.cosa, .otra-cosa')
let todasCosasArray = Array.from(todasCosas)

De manera alternativa:
todasCosasArray.forEach(el => {/* hacer algo con el elemento */})
```
<br>

#### **2. Añadir y eliminar _event listeners_**

Los _event listeners_ permiten a tu Javascript realizar una acción cuando se dispara un evento. Así es como tu sabes cuando un usario ha interactuado con el DOM. Un ejemplo de esto es cuando el usuario pincha en un botón.
Para gestionar este tipo de eventos tan solo es necesario conocer dos métodos: _addEventListener_ y _removeEventListener_

**Añadir _event listeners_**

Para añadir este tipo de eventos, primero se debes seleccionar tu elemento HTML, después llamar al método _addEventListener_ con dos parámetros:
```
let cosa = document.querySelector('.cosa')
cosa.addEventListener(evento, callback)
```
_evento_ es el nombre del evento que quieres que escuche. Estos eventos ya vienen predeterminados en las especificaciones. [Aquí hay una lista](https://developer.mozilla.org/en-US/docs/Web/Events) a mano con los tipos de eventos más comunes que seguramente querrás.

_Callback_ es la función que hace lo que tu quieres siempre que el evento es lanzado. Contiene un parámetro: el objeto evento.

**Eliminar _event listeners_**

Para eliminar este tipo de eventos se debe llamar al método _removeEventListener_ pasándole dos parámetros –el tipo evento y el _callback._
```
cosa.removeEventListener('click', callback)
```
Normalmente, solo se necesita eliminar un _event listener_ después de que la tarea haya finalizado. Por ello, es común encontrar el método _removeEventListener_ dentro de la llamada a addEventListner.
```
cosa.addEventListener('click', callback)

function callback () {
  console.log('cosa has sido pinchada!')
  // elimina el oyente de eventos
  thing.removeEventListener('click', callback)
}
```
<br>

#### **3.Añadir y eliminar clases**

- Para añadir una clase, se usa _element.classList.add('classname')_
- Para eliminar una clase, se usa _element.classList.remove('classname')_
- Para comprobar si una clase existe, se usa _element.classList.contains('classname')_

El siguiente código muestra el funcionamiento de un botón comprobando si una clase existe. Si existe la elimina, si no, la crea.
```
let boton = document.querySelector('boton')
let nav = document.querySelector('nav')

boton.addEventListener('click', toggleNav)

function toggleNav() {
  // Comprueba si nav contiene una clase abierta
  if (nav.classList.contains('abierta')) {
    // eliminar la clase abierta
    nav.classList.remove('abierta')
  } else {
    // añade la clase abierta
    nav.classList.add('abierta')
  }
}
```
<br>

#### **4.Añadir, cambiar y eliminar atributos**

Los atributos son una parte importante de los elementos HTML. A veces, es necesario extraer información de esos atributos para dar sentido a tu Javascript. Otras veces, puedes utilizar esos atributos para ayudar a hacer interfaces más accesibles.
Aquí hay una demostración del código anterior, escrita de un modo más accesible:
```
let boton = document.querySelector('boton')
let nav = document.querySelector('nav')
boton.addEventListener('click', toggleNav)
function toggleNav() {
  let abierta = nav.classList.contains('abierta')
  if (abierta) {
    nav.classList.remove('abierta')
    nav.setAttribute('aria-hidden', true)
    boton.setAttribute('aria-expanded', false)
  } else {
    nav.classList.add('abierta')
    nav.removeAttribute('aria-hidden')
    boton.setAttribute('aria-expanded', true)
  }
}
```

En este código han cambiado dos cosas:

- Se ha añadido _aria-expanded_ al _boton_ para decir a los _readers_ que el menú está expandido.
- Se ha añadido _aria-hidden_ a la _nav_ para evitar a los _readers_ leer el menú cuando este está escondido.

Así es como puedes extraer información de un atributo, editarlo o eliminarlo.

1. Para seleccionar un atributo, usa _getAttribute ('atrribute-name')_
```
boton.getAttribute('aria-expanded')
```
2. Para modificar/editar un atributo, usa _setAttribute('attribute-name', 'attribute-value')_
```
boton.setAttribute('aria-expanded', true)
```
3. Para eliminar un atributo, usa _removeAttribute('attribute-name')_
```
boton.removeAttribute('aria-expanded')
```
<br>

#### **5.Añadir y eliminar elementos HTML**

**Añadir elementos al DOM**

Se necesitan tres pasos para añadir texto en el DOM:

1. Crear un elemento HTML con _document.createElement_
```
let li = document.createElement('li')
```
2. Añadir contenido al elemento HTML poniendo _htmlProfundo_
```
li.htmlProfundo = '¡Hola mundo de nuevo!'
```
3. Añadirlo al DOM con _parentNode.prepend_ o _parentNode.append_
```
ul.append(li)
```

**Borrar elementos del DOM**

Para eliminar elementos del DOM necesitas llamar _parentNode.removeChild_. Este método toma como parámetro el elemento a borrar.
```
ul.removeChild(li)
```

No podemos simplemente decir que se borre el elemento li y esperar que Javascript conozca que lista debe eliminar. Es necesario decirle a nuestro Javascript cuál es el que, explícitamente, hay que eliminar. Si usas _querySelector_ para escoger qué elemento eliminar, este sería el método más sencillo de hacerlo:
```
let padre = document.querySelector('.padre')
let elaBorrar = document.querySelector('.elemento-a-borrar')
padre.removeChild(elaBorrar)

// O si no quieres escribir un querySelector separado
elaBorrar.parentNode.removeChild(elaBorrar)
```
En la demo de arriba no podemos hacerlo porque no hay manera de decirle cuál es el primer o último ítem con clases o ids.
En vez de eso, podemos usar _parentNode.children_ para conseguir un NodeList de los elementos dentro de ul, para después, usar el método Array para espcecifciar el elemento concreto a eliminar. Aquí está el código para eliminar el primer elemento hijo:
```
let lista = document.querySelector('ul')
removeFirst.addEventListener('click', e => {
if (lista.children.length) {
let primerNodo = lista.children[0]
list.removeChild(primerNodo)
}
})
```
