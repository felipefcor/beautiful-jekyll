---
layout: post
title: Cómo funciona Internet y qué significa programar
tags: [Internet, DNS, navegadores, browsers, programar]
---
## [**Aprende Javascript con MentoringJS - Pretraining Step 3**](http://MentoringJS.com)

En la gran red hay gran cantidad de información acerca de **cómo funciona Internet**. A continuación voy a señalar algunos enlaces interesantes que me han servidor a mi para conocer y reconocer cómo es el funcionamiento de Internet.

* #### [History of the Internet][link1]
[link1]: https://www.youtube.com/watch?v=9hIQjrMHTv4

Para empezar, como en cualquier disciplina, hay que empezar por lo más básico: la creación.

En el vídeo se explica, esquemáticamente, cómo nació **Internet** a partir de diversas redes de ordenadores en países diferentes, y con objetivos diferentes. A pesar de estas diferencias, existía un origen o motivo común, comunicarse a través de ordenadores que se encontraban lejos físicamente.

A mi entender, el objetivo inicial se mantiene en la actualidad, ya que Internet es poder conectarse a un sitio lejano y conseguir información del mismo.

* #### [Andrew Blum: ¿Qué es Internet realmente?][link2]
[link2]:https://www.youtube.com/watch?v=XE_FPEFpHt4

Una charla muy interesante que baja **Internet** del cielo (_cloud_) a la tierra. En el vídeo explica, de una forma muy esclarecedora, como Internet es un sistema físico de cables creado por humanos y con las mismas virtudes y defectos que cualquier creación humana.

También es importante entender todo esto para que la gente no piense en Internet como algo etéreo y _mágico_ que solo unos iniciados pueden entender. Todo es más sencillo de entender, al menos conceptualmente.

* #### [How the Internet Works in 5 Minutes][link3]
[link3]:https://www.youtube.com/watch?v=7_LPdttKXPc

Vídeo interesante para hacerte una idea más resumida de cómo funciona **Internet** cuándo alguien pone una web en el navegador de su ordenador. En el vídeo se explica el proceso físico que siguen las conexiones (web, correo, etc.) desde un ordenador de una casa hasta llegar a un servidor.

Muy gráfico y no demasiado difícil de entender. Para mi, la parte más difícil de entender ha sido la que hablaba del enrutamiento y cómo se trasmiten los paquetes a través de los enrutadores. También quizá la parte que habla del _cable_ de Internet, que es al que se conectan todos los ISP y servidores.

* #### [DNS Explained][link4]
[link4]:https://www.youtube.com/watch?v=72snZctFFtA

La parte del DNS es quizá la más difícil de entender del mundo **Internet**, no a nivel conceptual sino a nivel de complejidad.
A mi modo de ver, básicamente, existen servidores (o sea ordenadores) con información específica de nombres de dominios y las IP asignadas a estos. Pero para gestionar esto se necesitan diversos niveles y que haya uno al menos (_root_) que sepa siempre a dónde hay que ir para encontrar la IP actual de un dominio en concreto.

El sistema de dominios es el gran "_olvidado_" ya que nadie piensa en este cuando piensa en Internet. A pesar de ello, es parte importante de cómo funciona Internet y si no existiese, la gran red sería menos **usable** y de otra manera totalmente diferente.

----

Además de **Internet**, también hay buenos documentos sobre cómo funciona la navegación en Internet y qué significa programar.

#### [20 things I learned about browsers and the web][link5]
[link5]:http://www.20thingsilearned.com/en-US/home

  +  La **_nube_**. En la actualidad, todo el mundo habla de la nube, el _cloud_. Todo está flotando en Internet.
  A parte de ser un concepto poco clarificador, como ya he comentado arriba, también tiene otra vertiente más personal. Al guardarlo todo en servidores _online_ tienes la capacidad de trabajar con los mismos datos desde sitios y dispositivos diferentes y también tiene un añadido de seguridad, ya que los datos no se guardan en tu ordenador. Pero esto tiene una contrapartida a nivel de privacidad: datos privados almacenados en servidores de terceros y con claúsulas de privacidad ligeras que pueden provocar situaciones cómo las que se han vivido en la actualidad con Facebook.

  +  **HTML5**. Después de un tiempo en el mundo de Internet vas conociendo ciertos conceptos o tecnologías. Unas las conoces mejor que otras. En concreto, con HTML5 me ha sorprendido saber la mejora que ha provocado a nivel de usabilidad (poder insertar vídeos) y seguridad (ya no hacen faltan programas de terceros como plugins para meter vídeos).

  +  **Navegación**. Las actualizaciones del navegador, como en cualquier software, son fundamentales para cumplir ciertos objetivos: mejor seguridad y mejor navegación; pero también para los programadores: más y mejor innovación. La parte negativa, si se puede llamar así, es que como toda tecnología, esto cumple con el _principio de obsolescencia_ ya que requiere de mejores ordenadores y más potentes para ver webs con los navegadores más actualizados.

  + La **seguridad**. Por un lado, las _cookies_, que son herramientas que ayudan a la navegación personalizada de los usuarios, aunque también ponen en riesgo la privacidad. Los navegadores también implementan nuevas medidas para mejorar la seguridad respecto a _malware_ y _phising_. Esto es muy importante porque estos ataques maliciosos son cada vez más frecuentes.

  + **Rapidez de carga en navegadores**.
  Un dato interesante es que, cada vez más, los datos de imágenes, vídeos y javascript son más grandes en las webs y que, a pesar de comprimirse, es posbile que esto ralentice la carga. Sobre todo pasaría en dispositivos antiguos o software antiguo.
  Hay una técnica que se explica que es muy interesante, la de _resolución previa de DNS_. Antes de la carga de la web, el navegador busca todos los enlaces de una web y los traduce en direcciones IP en segundo plano. Cuando se va a una parte de la web, el navegador lo tiene cacheado y sirve la web rápidamente. Esta técnica tiene defectos, como por ejemplo si no conoces que se hace esto, tienes que saber que en ciertos casos hay que borrar caché o refrescar el navegador para que coja la nueva parte de la web o la misma en el caso de haber habido un cambio de alojamiento web.

  + **Código abierto** Esta es la parte final y la que, para mi, tiene mayor importancia actualmente. El mundo del software de código abierto es muy amplio e incluiye programas como navegadores, servidores web como Apache, hasta sistemas operativos como los basados en **Linux**.
  El sistema de código libre es el ejemplo de cómo los seres humanos pueden hacer lo que se propongan si se hace de manera conjunta y altruista. Básicamente, es sacar lo mejor del ser humano en sociedad.


* #### [What is programming][link6]
[link6]:https://www.lynda.com/Programming-Foundations-tutorials/What-programming/83603/90430-4.html


Muy interesante la idea de que programar es descomponer en pequeñas piezas de instrucciones para que un ordenador haga lo que tú quieras que haga.
En esta manera de ver las cosas se le da importancia al acto creativo de hacer una cosa descomponiéndola y tener la habilidad de hacerlo de manera ágil y sencilla. Esto es más importante que el lenguaje de programación que se utiliza para programar, que sería simplemente el método para expresar esas ideas con una sintaxis concreta.

----
