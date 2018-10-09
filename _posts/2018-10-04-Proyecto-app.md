---
layout: post
title: Prototipado de una app para móvil hecha con Ratchet
tags: [html, css, javascript, Ratchet, app]
---
### [Aprende Javascript con MentoringJS - Step 9 ](http://MentoringJS.com)

En este artículo voy a mostrar el prototipado (_mockup_) de una aplicación para móvil de un centro de Fisioterapia.

En primer lugar, como su nombre indica, esto es un prototipado o _mockup_ por lo que las imágenes que voy a mostrar son solo unos primeros acabados o ideas de cómo será la aplicación que voy a hacer. Esto implica también que la aplicación la iré mejorando, adaptando y puliendo utilizando nuevas herramientas y que todo el proceso de acabado se podrá ver a través de mi [blog](https://felipefcor.github.io/) y de mi página de [Github](https://github.com/felipefcor/Proyecto-app).

En segundo lugar, recalcar que para hacer este prototipo he utilizado una herramienta muy interesante e intuitiva llamada [Ratchet](http://goratchet.com/getting-started/).
Esta herramienta utiliza mucho html, css y javascript. Para mi prototipo he utilizado mayormente los dos primeros.

En su web se pueden ver ejemplos de aplicaciones así como pequeñas partes de código con su explicación y funcionamiento.

Sin más, paso a explicar la aplicación.

**Página principal**

![Index](https://raw.githubusercontent.com/felipefcor/Proyecto-app/master/img/index.png "Página principal")

La página principal contiene 3 partes, la cabecera, una imagen y las opciones de la _app_.

En la cabecera hay dos iconos a ambos lados. El de la izquierda es el de buscar. Cuando se pincha sobre él accedemos a un área de búsqueda en la aplicación. El de la derecha es una opción de ajustes de la _app_.

En la parte central tenemos el resto de opciones. En primer lugar, una imagen que puede ser corporativa. Debajo de esta imagen está el menú principal de la aplicación, donde podemos encontrar los servicios y especialidades que se ofrecen; una sección sobre los profesionales del centro; una sección desde donde pedir cita; un apartado con un blog y noticias que se generan; por último, hay una sección de imágenes en las que añadir imágenes variadas. secciones.

En cada sección hay un icono a la derecha en el que[ se accede a otra página](http://goratchet.com/one.html#push) con más información.


**Búsqueda en la página principal**

![Buscar](https://raw.githubusercontent.com/felipefcor/Proyecto-app/master/img/buscar.png "Buscar")

Tanto esta página como la de ajustes son unos _modales_ tal y como se indica en [Ratchet](http://goratchet.com/components/#modals), por lo que al pinchar en los iconos la página se mueve hacia arriba y aparece una nueva.

En la página de búsqueda la primera opción es el de las búsquedas más comunes. Aquí deberían aparecer entre 1 y 5 búsquedas más repetidas con sus botones para seleccionar y con un cuadro debajo dónde aparezca la información una vez seleccionado el botón.

Debajo hay un cuadro mayor para realizar la búsqueda que se desee y un botón de buscar.

También hay un icono en la parte superior para salir de dicha página y volver a la página principal.


 **Ajustes en la página principal**

![Ajustes](https://raw.githubusercontent.com/felipefcor/Proyecto-app/master/img/ajustes.png "Ajustes")

En la parte de ajustes hay 4 opciones.
La primera es la del idioma, con un botón al lado al que si se pincha se entrará en otra página con una opción para seleccionar el idioma de la aplicación.

La segunda opción sería una de sonido activado con un botón deslizable.

La tercera opción es la de cerrar sesión,también con un botón deslizable. Además, está por defecto activo porque se supone que estamos dentro de la _app_.


**Idiomas en la página de ajustes**

![Idioma](https://raw.githubusercontent.com/felipefcor/Proyecto-app/master/img/idiomas.png "Idiomas")

En la parte de idioma solo hay dos partes.
La primera es la cabecera, con el título de la página y un icono para volver a la página anterior.
Debajo, varios botones con los idiomas más comunes y un botón de seleccionar.


**Servicios y especialidades**

![Servicios](https://raw.githubusercontent.com/felipefcor/Proyecto-app/master/img/servicios.png "Servicios")

En esta página encontramos la cabecera típica de navegación de todas las páginas con el título y el icono de volver a la sección anterior.

**Como la cabecera es la misma en casi todas las páginas que vienen a continuación, no voy a poner la explicación en cada una, sino que con esta explicación anterior quedaría ya explicado**

Debajo está el cuerpo de la página que se compone de mini secciones iguales con todos los servicios ofertados.

En cadad mini sección hay una imagen relacionada con el servicio, el servicio en sí y una breve explicación del tipo de especialidad. Además, hay un [icono de acceso a otra página](http://goratchet.com/one.html#push) con más información sobre el servicio. Por último, en la parte inferior de esta mini sección está el precio por sesión.


**Ejemplo tipo de servicio**

![Servicios1](https://raw.githubusercontent.com/felipefcor/Proyecto-app/master/img/ganchos1.png "Especialidad")

![Servicios2](https://raw.githubusercontent.com/felipefcor/Proyecto-app/master/img/ganchos2.png "Especialidad")

Bajo la cabecera hay una barra a modo de título llamado _Características_. Debajo, una explicación más detallada del servicio o técnica. Por último hay un [_slider_](http://goratchet.com/one.html#sliders) de imágenes, como se puede ver en las dos imágenes de más arriba.



**Nosotros**

![Nosotros](https://raw.githubusercontent.com/felipefcor/Proyecto-app/master/img/nosotros.png "Nosotros")

Bajo la cabecera hay varios botones pequeños con los trabajadores del centro. [Si pinchas en uno aparece más información del mismo justo debajo](http://goratchet.com/one.html#segmentedControls).

Posteriormente, hay un apartado de localización con un mapa incorporado.
En la parte inferior hay un teléfono y algún tipo de acceso a redes sociales.


**Cita**

![Cita](https://raw.githubusercontent.com/felipefcor/Proyecto-app/master/img/cita.png "Cita")

Bajo de la cabecera la idea es que saliesen dos semanas en dos tablas, con los días laborales separados de dos separadores indicando la semana.

En cada día aparecería las horas que quedan libres y un [icono para acceder a otra página](http://goratchet.com/one.html#push) en la que reservar.


**Pedir cita**

![Pedir cita](https://raw.githubusercontent.com/felipefcor/Proyecto-app/master/img/lunes.png "Pedir cita")

Bajo la cabecera encontramos un día en una tabla dividida en dos mitades, por la mañana y por la tarde. Dentro de cada mitad están las horas y un botón al lado indicando si está ocupado o se puede reservar.


**Blog**

![Blog](https://raw.githubusercontent.com/felipefcor/Proyecto-app/master/img/blog.png "Blog")

Bajo la cabecera encontramos el título del _post_ del blog. Debajo el contenido y, en la parte inferior, la fecha de publicación.


**Galería**

![Imágenes1](https://raw.githubusercontent.com/felipefcor/Proyecto-app/master/img/imagenes1.png "Galería")

![Imágenes1](https://raw.githubusercontent.com/felipefcor/Proyecto-app/master/img/imagenes2.png "Galería")

Bajo la cabecera encontramos un _slider_ con imágenes que no caben en otra parte de la aplicación.


Bien, de momento este es el prototipo de la aplicación. Poco a poco iré mejorándolo añadiendo y eliminando cosas. Se podrá seguir en mi blog y en mi página de github.
