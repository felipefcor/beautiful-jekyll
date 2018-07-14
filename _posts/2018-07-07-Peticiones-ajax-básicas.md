---
layout: post
title: Peticiones Ajax básicas
---

### **[Aprende Javascript con MentoringJS - Step 7](http://MentoringJS.com)**

<br>
Esto es una traducción de un [artículo de David Walsh](https://davidwalsh.name/XMLHttpRequest) acerca de este tipo de peticiones.

Hay un gran número de tareas comunes frontend que muchos de nosotros no hemos tocado en profundidad, APIs engorrosas debido a que nuestros queridos frameworks de Javascript nos han proporcionado maneras más sencillas de trabajar con ellas. Esto lo trata Walsh en _[How JavaScript Event Delegation Works](https://davidwalsh.name/event-delegate)_, _[Do a Basic HTTP Request with Node.js](https://davidwalsh.name/nodejs-http-request)_.

También he hecho algunas notas sobre cuestiones de bajo nivel relacionadas con APIs. La siguiente es _XMLHttpRequest_, la API con la cuál realizamos nuestras llamadas AJAX!

**Recuperando el objeto XHR**

A diferencia de la mayoría de APIS, conseguir el nuclio de un componente conlleva bastante trabajo desde que Internet Explorer solía utilizar un componente ActiveX para trabajar con AJAX.
```
var peticion;
if (window.XMLHttpRequest) { // Mozilla, Safari, ...
  request = new XMLHttpRequest();
} else if (window.ActiveXObject) { // IE
  try {
    peticion = new ActiveXObject('Msxml2.XMLHTTP');
  }
  catch (e) {
    try {
      peticion = new ActiveXObject('Microsoft.XMLHTTP');
    }
    catch (e) {}
  }
}
```

El código es feo, ¿pero eso es lo que se espera que haya en bambalinas, no?

**Haciendo una petición**

Hacer una petición requiere llamar a dos funciones:
```
request.open('GET', 'https://davidwalsh.name/ajax-endpoint', true);

request.send(null);
```

La llamada _open_ define el tipo de petición (get, post, etc.) y el método _send_ ejecuta la petición. ¡Bastante fácil! Añadir encabezados personalizados también es fácil.
```
request.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
```

**Peticiones _Callbacks_**

Esta claro que hacer peticiones es de alguna manera inútil si no se es capaz de utilizar el resultado, y aquí hay dos maneras de gestionar una _callback_.
```
//  cambia estado
request.onreadystatechange = function() {
        if(request.readyState === 4) { // hecho
                if(request.status === 200) { // completo
                        console.log(request.responseText)
                }
        }
};

// addEventListener
function callbackFn(e) {
        // gestionar cada evento
}
request.addEventListener("progress", callbackFn, false);
request.addEventListener("load", callbackFn, false);
request.addEventListener("error", callbackFn, false);
request.addEventListener("abort", callbackFn, false);
```

Escoge cualquier método que prefieras aunque el método _addEventListener_ es probablemente el más elegante.

Esta es mi sencilla introducción a crear peticiones AJAX simples con la API nativa _XMLHttpRequest_. Para más información sobre tests AJAX, tales como enviar información a través de un formulario, revisa la [siguiente web](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest).

---
