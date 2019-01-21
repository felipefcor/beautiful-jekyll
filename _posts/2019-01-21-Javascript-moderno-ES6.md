---
layout: post
title: Javascript moderno. ES6
tags: [javascript, ES6, ECMAScript]
---
### [Aprende Javascript con MentoringJS - Step 12 ](http://MentoringJS.com)
Como el título de este artículo indica, voy a hablar de ES6 y como este relativamente nuevo paradigma ha cambiado por completo la escena del Frontend actual.

En primer lugar, [un poco de historia](https://es.wikipedia.org/wiki/ECMAScript). En 1995, Brendan Eich, programador que trabajaba en Netscape, creó _Mocha_. Ese lenguaje desembocó finalmente en la creación de Javascript.
Un año más tarde se creó la especificación para Javascript, ECMAScript.
Desde el lanzamiento en junio de 1997 del estándar ECMAScript 1, han existido las versiones 2, 3 y 5 (la 4 se abandonó​). En junio de 2015 se publicó la versión ECMAScript 6. Y la versión 7 está en camino.

De la versión 5 a la versión 6 ha habido algunos cambios importantes para la creación de aplicaciones complejas. Se podría decir que ES6 está en camino de sustituir a ES5, aunque ES5 tiene aún cuerda para rato.

Las características de ES6 que la comunidad ha marcado como más importantes serían:


**Var vs let/const**

En ES5 las variables se definían con _var_. Este tipo de asignaciones implicaban que el ámbito de acción (_scope_) de la variable se extendía más allá del bloque dónde se definía la misma, provocando posibles errores.

```
function sum() {
   for (var x = 0; x < 10; x++) {
        //Técnicamente, x debería quedase en este ámbito de bloque ya que se ha
        // definido aquí
   }

   //Pero resulta que es accesible desde fuera
   console.log(x); // 10
}

```
Para solventar esta situación ES6 introdujo dos nuevos tipos para asignar una variable: _let_ y _const_.

_Let_ funcionaría igual que _var_ pero está específicamente acotada al ámbito de bloque dónde se declara. En el ejemplo anterior el resultado del _console.log_ sería.
```
console.log(x); // x no ha sido definida  
```

Con _const_, en cambio, es posible definir una constante. Como su nombre indica, una constante no puede ser reasignada posteriormente.

```
const x = 1;
x = 2; // muestra un error ya que no es posible reasignar x

```
Hay que tener cuidado con _const_ ya que no es posible reasignarla pero sí que se puede modificar cualquier objeto o array al que apunta.
```
const lyrics = ['badger', 'badger', 'badger'];

lyrics.push('mushroom'). // No daría error y lo añadiría al array lyrics

```


Por todo ello, a partir de ES6, las buenas prácticas de programación con JavaScript recomiendan no utilizar _var_ y sí en cambio utilizar _let_ o _const_.

---

**Template literals**

Los _template literals_ (_strings_ delimitados con _backtics_ ` `) te permiten:

- Interpolar Javascript dentro de tu _strings_ usando ${}
- Escribir múltiples líneas de _strings_
- Hacer todo tipos de "locuras"

Antes de ES6 para realizar concatenaciones se tenía que hacer de la siguiente manera:

```
var name = 'Felipe';
var message = 'Hola ' + name + ',';

```

Con ES6 y _template literals_ se puede hacer de la siguiente manera:

```
var name = 'Felipe';
var message = `Hola ${name},`;

```

Como se puede ver, con ES5 las concatenaciones, sobre todo si son más largas, se convierten en complejas y difíciles de hacer y leer. Con los _template literals_ se solventa esa complejidad.

---


**Arrow fucntions**

En ES5 las funciones se podían expresar así:
```
const square = function(number) {
   return number * number;
}
```

Con _arrow_functions_ se puede cambiar dicha función y expresarla así.
```
const square = (number) => {
   return number * number;
}
```
Aquí nos deshacemos de la palabra clave _function_. Además, también es posible deshacernos de más código, como por ejemplo si la función solo tiene una línea se puede quitar el _return_ y los _curly braces_. Y si la función tiene un solo parámetro, nos podemos deshacer del paréntesis.
```
const square = number => number * number;
```

Una de las características de las _arrow_functions_ es que hereda el _this_ del contexto en el que se utilizan. Dentro de las funciones típicas el _this_ pertenecía al propio contexto de la función, por lo que si se quería utilizar para apuntar al contexto de la función principal se tenía que _bindear_. En cambio, con _arrow_functions_ el _this_ sigue perteneciendo al ámbito de la función principal, por lo que no es necesario _bindearlo_.

---

**Bucle for ... of**

For-of es un tipo de bucle que en ES6 reemplaza a los bucles for-in y forEach() y soporta el nuevo protocolo de iteración.

For-of se puede utilizar con objetos iterables (_arrays_, _strings_, _maps_, etc.)

```
const iterable = ['a', 'b'];
for (const x of iterable) {
    console.log(x);
}

// Output:
// a
// b
```


**Métodos arrays**

En ES6 nos encontramos con varios métodos para trabajar con _arrays_.

En primer lugar, _array.prototype.findIndex_. Este _array_ nos dará el primer ítem de una _array_ que encaja con el que se ha dado como predicado en la función.

También es posible devolver el objeto que se está buscando en vez del índice con  _array.prototype.find_.

Otros nuevos métodos de _arrays_ serían _array.prototype.fill_, _array.prototype.copyWithin_ y _array.map_, del que hablaremos a continuación.


**Método Array.map**

Hay varios métodos para trabajar con _arrays_. Uno de los más usados es _array.map_. Con este método puedes recorrer listas en vez de utilizar un bucle y una de sus características es que genera un par de elementos arrays cada uno con su key/valor. Esto tiene la ventaja de que el _array_ que se _mapea_ se queda intacto ya que se genera un nuevo _array_ igual que es con el que se trabaja.

Un ejemplo sería el siguiente. Tenemos un _array_:

```
const colors = ['red', 'yellow', 'black'];

```
Utilizamos el método _map_ para recorrer ese _array_ y extraer otro _array_ con los mismos ítems:

```
const items = colors.map(function(color){
  return  color;
  })
```

Si utilizamos la _arrow_function_ el código puede quedar más limpio:

```
const items = colors.map(color => color);
```


**Destructuring**

_Destructuring_ es una expresión que te permite extraer propiedades de un objeto o ítems de un array:

```
const address = {
   street: '123 street',
   city: 'Londres',
   state: 'UK'
};

```

En ES5 para acceder a las propiedades de un objeto se hacía de la siguiente manera:

```
const street = address.street;
const city = address.city;
const state = address.state;
```

Con ES6 se puede hacer de la siguiente manera:
```
const { street, city, state } = address;

```
O si solo queremos acceder a una sola propiedad:
```
const { state } = address;

```

Con _arrays_ se haría de la misma manera. En ES5 la sintaxis era:
```
const names = ['Juan', 'Smith'];
const firstName = values[0];
const lastName = values[1]; `

```
En ES6:
```
const values = ['Juan', 'Smith'];
const [firstName, lastName] = values;

```


**Spread operator**

En ES5 la manera que teníamos para concatenar _arrays_ era de la siguiente manera:
```
const first=[1,2,3]
const second=[4,5,6]

const both = first.concat(second);
```
Con el _spread operator_ de ES6 se podría hacer lo mismo de la siguiente manera:
```
const both = [...first, ...second]
```
Esto lo que hace sería "esparcir" los ítems de un array. Con ello se puede hacer muchas operaciones, como la de concatenar del ejemplo anterior. También se podría añadir un ítem en el medio de manera más sencilla comparada con ES5.
```
const both = [...first,'a',...second]
```
También se podría clonar un array:
```
const clone=[...first];
```

Este operador también se utiliza para los objetos:
```
const first = {name: 'Felipe'};
const second = {lastName: 'Fernández'};

const both = {...first, ...second, location: 'World'}
```

El ejmplo anterior concatenaría dos objetos y le añadiría una nueva propiedad, _location_



**Classes**

Las clases de ES6 son _azúcar sintáctico_ (syntactical sugar) sobre la herencia basada en prototipos típica de JavaScript. La sintaxis de clases no introduce un nuevo modelo de herencia orientado a objetos en Javascript.

Como se ha dicho, están pensadas para trabajar con objetos. Por ejemplo, si tenemos un objeto persona con una propiedad nombre y un método escribir, se podría utilizar la sintaxis de clase para encapsular este objeto y poder crear nuevos objetos persona de manera más sencilla y eficaz.

```
const person = {
  name :"Juan",
  write () {
    console.log("write");
  }
}
```

Este objeto transformado en clase quedaría tal y como:
```
class Person{
  constructor (name){
    this.name =name;
  }

  write(){
    console.log("write");
  }
}
```

Con esto podríamos crear un nuevo objeto persona siguiendo la sintaxis de _constructor functions_ de ES5:
```
const person = new Person ('Jose');
```
La mayor funcionalidad de las clases es que puedes implementar un método en un solo sitio (dentro de la clase). Esto ayuda a la refactorización y a la búsqueda de posibles bugs.


**Imports/exports**

En ES6 puedes tener el concepto de modularidad de forma nativa en JavaScript.
Siguiendo el ejemplo de las clases, puedes tener en tu código muchos ficheros con clases independientes. Con _import_ y _export_ es posible importar y exportar dichos módulos(clases) y usarlos en otros archivos. Esto permite tener una estructura más limpia y mantenible al tenerlo todo separado.


**Promises**

Las _promises_ son una alternativa a las _callbacks_ para mostrar el resultado de una computación asíncrona.

---

Los recursos que he utilizado para realizar este artículo han sido:

- [Un rápido vistazo a ES6](http://jamesknelson.com/es6-the-bits-youll-actually-use/), de James K Nelson

- [4 características modernas de Javascript](https://programmingwithmosh.com/javascript/essential-modern-javascript-features/), de Mosh Hamedani

- [Javascript para desarrolladores React](https://www.youtube.com/watch?v=NCwa_xi0Uuc), un vídeo de Mosh Hamedani

- [Si has aprendido Javascript (ES6) "de oído"](https://www.youtube.com/watch?v=ytpqRmkiAkQ), un vídeo de Lemoncoders.

- [Exploringjs](http://exploringjs.com/es6/). Un libro online con mucha información.
