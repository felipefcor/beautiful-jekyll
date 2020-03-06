---
layout: post
title: ¿Dominará el mundo Object Calisthenics?
tags: [java, Object Calisthenics, TDD, OOP, Design]
---

En [programación orientada a objetos](https://es.wikipedia.org/wiki/Programaci%C3%B3n_orientada_a_objetos)(OOP) existen varias reglas, principios y teorías que nos ayudan con el diseño de nuestras aplicaciones. Una de estas es Object Calisthenics, herramienta básica en nuestro objetivo de dominación mundial.

![Domination](/img/doctor-maligno.jpeg)

### ¿Qué es Object Calisthenics?

Es un conjunto de 9 reglas creadas por Jeff Bay en un artículo que formó parte de una antología. En el siguiente [enlace](https://www.cs.helsinki.fi/u/luontola/tdd-2009/ext/ObjectCalisthenics.pdf) se puede acceder al artículo concreto gracias a la difusión de la Universidad de Helsinki.

![Antologhy](https://imagery.pragprog.com/products/105/twa.jpg?1298589765)

Jeff Bay bautizó estas reglas como Object [Calisthenics](https://es.wikipedia.org/wiki/Calistenia) debido a que creía que podrían servir como práctica y entrenamiento con el objetivo de mejorar el diseño de las aplicaciones. 

Para crear estas 9 reglas, dicho autor se basó en algunos patrones de diseño en programación orientada a objetos:

- Cohesión (Clases que tienen una única responsabilidad)
- Bajo acoplamiento (Clases poco dependientes entre ellas)
- No duplicación (Don’t repeat yourself(DRY).Por lo general, hay que evitar el código duplicado)
- Encapsulación (Agrupar comportamientos en clases)
- Testeabilidad (Código que sea testeable)
- Legibilidad (Código que sea fácil de entender)
- Foco 

---

### Las 9 reglas

- Solo un nivel de indentación por método
- No usar la palabra reservada ELSE
- Mantén las entidades pequeñas
- Envolver los primitivos y strings en clases
- Envolver las colecciones en clases
- Un punto por línea
- No abreviar
- No más de dos variables de instancia por clase
- No getters ni setters

---

![Rules](https://i.pinimg.com/originals/31/a5/6c/31a56c6d1531d882d20c6281f29618b9.jpg)

A continuación voy a ir explicando los problemas que se generan si no se cumplen con estas reglas y qué posibles soluciones ofrece Jeff Bay para solventarlo.



#### 1. Solo un nivel de indentación por método

Cuando nos enfrentamos a código en el que hay bucles anidados podemos comprobar que es posible que nos cueste entender lo que hace ese método o clase. Además, también nos puede indicar que ese método tiene múltiples responsabilidades. 
Esto nos podría indicar que seguramente sería necesario extraer todo el código que comparta el mismo comportamiento en otro método, para que cada método tenga así una única responsabilidad y, de paso, se entienda mucho mejor al revisar de nuevo dicho código.

Un ejemplo de esta situación sería el siguiente, donde podemos ver varios niveles de indentación que no facilitan la lectura de código.

```java
class Board {
    public String board() {
        StringBuilder buf = new StringBuilder();

        // 0
        for (int i = 0; i < 10; i++) {
            // 1
            for (int j = 0; j < 10; j++) {
                // 2
                buf.append(data[i][j]);
            }
            buf.append("\n");
        }

        return buf.toString();
    }
}
```
<br>

#### 2. No usar la palabra reservada ELSE

Utilizar las cláusulas if/else de manera indiscriminada puede ser peligroso ya que puede generar una mala legibilidad del código y también puede provocar que solo se ejecute, o se promueva, una línea principal de ejecución con muy pocos casos. Esto se podría solventar realizando polimorfismo o, de una manera más sencilla, añadiendo returns prematuros. 

En el siguiente código podemos ver un ejemplo práctico de esta situación:

```java
public void login(String username, String password) {
    if (userRepository.isValid(username, password)) {
        redirect("homepage");
    } else {
        addFlash("error", "Bad credentials");

        redirect("login");
    }
}
```
<br>

#### 3. Mantén las entidades pequeñas

Todos nos hemos encontrado con clases y/o métodos larguísimos, con infinidad de código y hemos pensado: ¿Esto qué es? ¿Qué hace esta clase?. 
En estos casos, después de salir del asombro, nos quedamos pensando qué hace esa clase o método y hasta que pasan unos minutos u horas no lo acabamos de entender. 
Por eso, para mejorar la legibilidad y mantenibilidad de nuestro código es importante mantener las entidades lo más pequeñas, compactas y entendibles posibles para que el próximo que venga (incluso nuestro yo futuro) lo entienda lo antes posible.

<br>

#### 4. Envolver los primitivos y strings

Cuándo encontramos primitivos o Strings sueltos en nuestro código normalmente nos indica que puede haber algún problema. Esto es debido a que con este tipo de variables se puede expresar multitud de cosas y eso nos lleva a utilizarlos constantemente. Además, como este tipo de variables tienen sentido en un contexto, cuando se cambia de contexto pierden significado o, en el peor de los casos, pierden el sentido y nos confunden. 
Una posible solución a este tipo de problemas es encapsular este tipo de primitivos y Strings en clases o Value Objects.

En el código siguiente vemos una encapsulación de un primitivo en una clase. 
En este caso, se recibe un primitivo y si cumple una condición se guarda en dicho objeto. Al realizar la instancia de esa clase desde fuera de la clase se obtiene una mejor semántica.
Además, el objeto resultante tiene un significado inherente, no en un contexto determinado como antes tenía el String.

```java
class NaturalNumber {

    private int numberBaseTen;

    public NaturalNumber(int number) {

 if(number <0) {
throw new RuntimeException();
}

             this.numberBaseTen = number;
}
}

NaturalNumber number = new NaturalNumber(8);
```

<br>

#### 5. Envolver las colecciones en clases

Este caso es muy parecido al anterior, ya que al final una lista es otro tipo de variable muy parecida a primitivos y Strings. Además, se le añade que normalmente con las listas se realizan varias acciones como añadir y sacar elementos de dicha lista. Una posible solución sería similar a la anterior, encapsular la lista en una clase (llamada First Class Collection), con lo que ganaríamos en cohesión ya que se añade el comportamiento de dicha colección como métodos de la misma.

<br>

#### 6. Un punto por línea

En ocasiones podemos encontrar código con llamadas a métodos o clases de manera concatenada. Es cierto que queda limpio reducir varias llamadas en una misma línea, pero esto trae consigo varias problemáticas. La más importante es que podemos encontrar casos con un alto acoplamiento entre diferentes clases que haga muy difícil o imposible cambiar o refactorizar esas clases. Por todo ello, la solución a esta situación sería que una clase solo “hablase con sus amigos”, con la más cercana.

En el siguiente código vemos una llamada a un String desde una clase a través de otra. Con este formato, si se quiere cambiar el tipo de la variable *representation* se tendría que cambiar también el código de la clase Board. 
La solución sería que la clase *Location* tuviese un método que devolviese la operación y que *Board* solo hablase con la clase *Location*.

```java
class Location {
    public Piece current;
}
class Piece {
    public String representation;
}
class Board {
    public String boardRepresentation() {
        StringBuilder buf = new StringBuilder();

        for (Location loc : squares()) {
            buf.append(loc.current.representation.substring(0, 1));
        }

        return buf.toString();
    }
}

```

<br>

#### 7. No abreviar

Otro caso muy significativo que, por desgracia es muy frecuente, es el caso de los nombres de variables, métodos y clases abreviados. Cuando nos encontramos estos casos nos damos cuenta que es muy difícil de leer y entender ese código ya que necesitamos ver el código varias veces para entenderlo. 
Por todo ello, es altamente recomendable poner nombres con sentido, incluso si son muy largos. Además, si se hacen largos, nos dará visibilidad sobre si ese método o clase tiene más de una responsabilidad pudiendo así dividirlo en varios métodos o clases. Con esto mejoraríamos o cumpliríamos con el principio de única responsabilidad.

<br>

#### 8. No más de dos variables de instancia por clase

Cuando nos encontramos con una clase con muchas variables de instancia lo primero que detectamos es que esa clase es posible que haga muchas cosas. En este caso, el problema concreto que esto genera es que la clase no es cohesiva, que hace muchas cosas. 
Como el caso anterior, para solventar esta situación podemos empezar a separar responsabilidades y dividir las variables de instancia en varias clases. 

En el siguiente código podemos ver un ejemplo de clase con tres variables de instancia y como es posible solucionarlo descomponiéndolas en varias clases. 


```java
class Name {
 String first;
 String middle;
 String last;
}
```
Se podría descomponer en:
```java
class Name {
 Surname family;
 GivenNames given;
}
class Surname {
 String family;
}
class GivenNames {
 List<String> names;
}
```

<br>

#### 9. No getters ni setters


El último caso es el de getters y setters.
Cuándo programamos a nivel de orientación de objetos una de las cosas más importantes es que los objetos sean cohesivos y que no estén totalmente abiertos al exterior. El caso de los getters y setters en cierta manera incumple estas premisas ya que con estos dos tipos de funciones se intentan acceder al contenido de un objeto desde fuera e incluso modificarlo.
Una manera de evitar estas situaciones es la de pedir datos desde fuera y no preguntar directamente ([Tell don't Ask](https://pragprog.com/articles/tell-dont-ask)).

En el siguiente código vemos un ejemplo de dos métodos, uno con un *setter* y otro con un *getter*. Se puede observar que se pide información y se modifica el objeto desde fuera implicando una falta de control sobre el estado del propio objeto. 

 

```java

// Game
private int score;

public void setScore(int score) {
    this.score = score;
}

public int getScore() {
    return score;
}

// Usage
game.setScore(game.getScore() + 100);

```
---


En esta charla además de explicar las reglas de Calisthenics y algunos ejemplos, también utilicé como recurso de apoyo una Kata llamada Tic Tac Toe.

![Tictatoe](https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcRqE1uS5xvKNpIQZRSgCpUCIxp0wOhzOuf5SL4nfjAxwJtCS9l6)

Las normas de esta Kata son las siguientes:

- X siempre juega primero
- Los jugadores alternan una X y una O
- No se puede jugar en una posición ya - utilizada
- Hay ganador cuando hay tres X u O seguidos en horizontal, vertical o diagonal
- Si se han rellenado todas las posiciones y no hay ganador, hay empate

En la demo que hice expliqué las diferencias entre dos versiones de la misma Kata, una sin aplicar Object Calisthenics y otra aplicándolos. 

Adjunto un [enlace a mi github](https://github.com/felipefcor/tictactoe-kata) dónde se pueden ver las dos versiones de esta Kata con algunos comentarios a modo de ayuda/guía.

Este artículo es una revisión y ampliación de una presentación que realicé hace unas semanas en apprenticeship que estoy realizando con Cokaido en mi actual empresa. 

Esta presentación me sirvió para iniciarme en una parte importante del diseño y refactoring de código. También me sirvió para darme cuenta de que el diseño es una parte muy importante a la hora de programar y que solo con TDD no es suficiente para tener diseños entendibles, reusables y mantenibles a largo plazo.

A continuación encontraréis algunos enlaces que utilicé para hacer mi presentación y este artículo. 

[Artículo de William Durand](https://williamdurand.fr/2013/06/03/object-calisthenics/)

[Artículo de Fran Diéguez](https://www.frandieguez.dev/posts/object-calisthenics-write-better-object-oriented-code/)

Espero poder seguir posteando más artículos en los que explicar todos los conocimientos que voy adquiriendo, siempre con el objetivo de conquistar el mundo de la programación orientada a objetos  ;-)

![Mastering the world ](https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcTu6pOQIAkjWC1t1M3czKIrWEg2pHFkdqMQrzNOU27eTzC4Wpih)




