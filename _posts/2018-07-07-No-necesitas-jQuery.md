---
layout: post
title: ¡No necesitas jQuery! 
tags: [javascript, jquery, ajax, requests, xmlhttprequest]
---
### **[Aprende Javascript con MentoringJS - Step 7](http://MentoringJS.com)**
Esto es una traducción del artículo [You Don't Need jQuery!](https://blog.garstasio.com/you-dont-need-jquery/about/) de Ray Nicholus.


#### **Peticiones ajax**

Muchos programadores que han aprendido desarrollo web a través de las lentes de jQuery probablemente piensan que jQuery está haciendo algo mágico cuando se invoca al método _$.ajax_.

Eso no podría estar más lejos de la realidad. Todo el trabajo pesado lo hace el navegador vía el objeto _XMLHttpRequest_. El ajax de jQuery simplemente se envuelve en el _XMLHttpRequest_. Las peticiones ajax no son muy difíciles si se usa el soporte que hay en el navegador, como verás en un momento. Incluso las peticiones de origen-cruzado son simples.

#### **1. GETting**
#### **2. POSTing**
#### **3. Codificar URLs**
#### **4. JSON**


<br>
#### **1. GETting**


Empecemos con una conocida pero simple petición. Necesitamos preguntar al servidor por el nombre de una persona, y dar a esa persona única un ID. Ese ID único debe ser incluido como parámetro en la consulta en la URI, con una carga útil vacía, como es común para las peticiones _get_. Se debe generar una alerta con el nombre del usuario o un error si la petición falla.

**jQuery**

Hay varias formas de iniciarse con las peticiones _get_ en ajax usando la API de jQuery. Una tiene que ver con el método _get_, que es el método simplificado para ajax del tipo de _get_. Simplemente utilizaremos el método ajax de aquí en adelante por su consistencia
```
$.ajax('miservicio/nombredeusuario', {
    data: {
        id: 'unicoid'
    }
})
.then(
    function success(nombre) {
        alert('El nombre de usuario es ' + nombre);
    },

    function fail(data, status) {
        alert('Peticion fallida. Se devuelve el status ' + status);
    }
);

```


**Objeto nativo _XMLHttpRequest_**
```
var xhr = new XMLHttpRequest();
xhr.open('GET', 'miservicio/nombredeusuario?id=unicoid');
xhr.onload = function() {
    if (xhr.status === 200) {
        alert('El nombre de usuario es ' + xhr.responseText);
    }
    else {
        alert('Peticion fallida. Se devuelve el status ' + xhr.status);
    }
};
xhr.send();

```

El de arriba es un ejemplo de Javascript nativo que funcionaría en Internet Explorer 7. Incluso en la versión 6 tan solo cambiando _XMLHttpRequest_() por _new ActiveXObject("MSXML2.XMLHTTP.3.0")_. Nuestro ejemplo nativo parece fácil de seguir y bastante intuitivo de escribir. ¿Por ello, para qué usar aquí jQuery? ¿Cuál es el beneficio?.

<br>


#### **2. POSTing**


Ampliemos un poco el ejemplo anterior. Ahora que ya tenemos el nombre completo del usuario, vayamos más allá y cambiémoslo. Podemos dirigirnos de nuevo a este usuario por el ID. Necesitamos hacer una petición _POST_ a nuestro servidor para ese particular usuario e incluir el nombre del usuario dentro del cuerpo de la petición como una cadena codificada en la URL. El servidor devolverá el nombre actualizado en su respuesta, por lo que nosotros  lo revisaremos para comprobar que todo sea correcto.
El método correcto para este tipo de casos es _PATCH_, pero hay ciertos problemas con _PATCH_ y otros métodos no tradicionales en navegadores antiguos (como Internet Explorer 8). Por ello, tan solo usaremos _POST_, aunque el código es idéntico en cada caso, con la excepción del nombre del método.


**jQuery**
```
var nuevoNombre = 'Juan Perez';

$.ajax('miservicio/nombredeusuario?' + $.param({id: 'unicoid'}), {
    method: 'POST',
    data: {
        nombre: nuevoNombre
    }
})
.then(
    function success(nombre) {
        if (nombre !== nuevoNombre) {
            alert('Algo fue mal. El nombre ahora es ' + nombre);
        }
    },

    function fail(data, status) {
        alert('Peticion fallida. Se devuelve el status ' + status);
    }
);

```

**Objeto nativo _XMLHttpRequest_**
```
var nuevoNombre = 'Juan Perez',
    xhr = new XMLHttpRequest();

xhr.open('POST', 'miservicio/nombredeusuario?unicoid');
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
xhr.onload = function() {
    if (xhr.status === 200 && xhr.responseText !== nuevoNombre) {
        alert('Algo fue mal. El nombre ahora es  ' + xhr.responseText);
    }
    else if (xhr.status !== 200) {
        alert('Peticion fallida. Se devuelve el status ' + xhr.status);
    }
};
xhr.send(encodeURI('nombre=' + nuevoNombre));
```


Parece bastante claro que la manera de enviar la petición de jQuery es mucho más elegante. Hace una parte del trabajo por ti. ¿Pero, vale la pena su elegancia sabiendo la dependencia que genera? Si tu te encuentras a gusto con la relativa simpleza del objeto _XMLHttpRequest_, la respuesta probablemente será que no.


<br>


#### **3. Codificar URLs**

jQuery aporta una función que toma un objeto y lo devuelve dentro de una cadena codificada en una  URL.
```
$.param({
    key1: 'alguna cosa',
    'key 2': 'otra cosa'
});
```

Esto está bien, pero se puede hacer algo parecido de manera más dinámica y sin jQuery.
La API web ofrece dos funciones con cadenas codificadas en la URL: _encodeUri_ y _encodeURIComponent_. Una función que construye en su soporte nativo una funcionalidad igual a la de _$.param_ y que no es terriblemente difícil.
```
function param(object) {
    var cadenaCodificada = '';
    for (var prop in object) {
        if (object.hasOwnProperty(prop)) {
            if (cadenaCodificada.length > 0) {
                cadenaCodificada += '&';
            }
            cadenaCodificada += encodeURI(prop + '=' + object[prop]);
        }
    }
    return cadenaCodificada;
}
```

Por supuesto, el método de jQuery es mucho más elegante. Pero esto es, en mi humilde opinión, una de las pequeñas cosas dónde jQuery puede mejorar tú código. ¿Pero no vas a utilizar solo jQuery por su método _$.param_,verdad?

<br>


#### **4. JSON**

Ahora sabemos como comunicarnos con una API que espera JSON y devolverlo en una respuesta. Digamos que necesitamos actualizar alguna información de un usuario concreto. Una vez el servidor procese nuestra actualización, devolverá toda la información actual (después de la actualización) sobre el usuario en su respuesta. El método correcto para este tipo de petición es PUT, por lo que vamos a revisarlo.

**jQuery**
```
$.ajax('miservicio/usuario/1234', {
    method: 'PUT',
    contentType: 'application/json',
    processData: false,
    data: JSON.stringify({
        nombre: 'Juan Perez',
        edad: 34
    })
})
.then(
    function success(infoUsuario) {
        // infoUsuario será un objeto JavaScript que contiene propiedades como
        // nombre, edad, direccion, etc
    }
);
```


El código de arriba es bastante feo. jQuery está roto en el la sección ajax en diferentes niveles. Es, además, bastante confuso enviar en jQuery cualquier otra cosa que no sea peticiones ajax triviales, en mi experiencia. El módulo de jQuery en ajax está enfocado como aplicación para peticiones _x-www-form-urlenconded_. Cualquier otra codificación requerirá algo más de trabajo.

Lo primero que necesitamos decirle a jQuery es que deje los datos solos. Entonces, debemos tornar el objeto Javascript en JSON. ¿Por qué jQuery no puede hacer esto por nosotros utilizando contentType? No estoy seguro.
Si el servidor devuelve un Content-Type apropiado en la respuesta, el código exitoso debe ser pasado como objeto Javascript representando el JSON devuelto por el servidor.

**Web API**
```
var xhr = new XMLHttpRequest();
xhr.open('PUT', 'miservicio/usuario/1234');
xhr.setRequestHeader('Content-Type', 'application/json');
xhr.onload = function() {
    if (xhr.status === 200) {
        var infoUsuario = JSON.parse(xhr.responseText);
    }
};
xhr.send(JSON.stringify({
    nombre: 'Juan Perez',
    edad: 34
}));
```

El código de arriba funcionaría en IE8 y superiores. Pero puede que sea necesario dar soporte para navegadores antiguos. En ese caso, tan solo añádelo en json.js para paliar la falta de soporte JSON en IE7 y anteriores versiones.

<br>
