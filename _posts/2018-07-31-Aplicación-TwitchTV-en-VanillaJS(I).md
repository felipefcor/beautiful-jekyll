---
layout: post
title: Aplicación Twitch TV en VanillaJS (I)
tags: [javascript, jquery, vanillajs, ajax, requests, xmlhttprequest, twitchtv, DOM]
---
### [Aprende Javascript con MentoringJS - Step 7 ](http://MentoringJS.com)
> Aviso para navegantes:

> Este programa es una primera versión de una aplicación TwitchTV. La aplicación hace peticiones a la API de Twitch y te devuelve los canales que están online y offline.

> Es una primera versión por dos motivos: no es funcional (no muestra de manera correcta los canales online y offline) y la manera en la que se ha realizado no es del todo correcta al utilizar bucles for y no forEach por ejemplo. Eso hace que, al ser peticiones asíncronas a la API de Twitch, los bucles for ejecuten de manera no ordenada y solapando variables. Esto, al final, es lo que hace que no sea funcional

> En el siguiente artículo del blog se puede ver la segunda versión de la aplicación con todas las correcciones pertinentes

En este artículo voy a realizar una explicación de la aplicación [Twitch TV](https://www.twitch.tv/) hecha con [VanillaJS](http://vanilla-js.com/) que se se puede ver en [mi repositorio](https://github.com/felipefcor/twitchtv) Iré explicando cada parte de la misma en detalle.

El primer paso es realizar un fork del repositorio de la aplicación Twitch TV de [Rocio M. Sebastian](https://github.com/rocio-m-sebastian/twitchtv). En este repositorio la aplicación está hecha en jQuery, tanto las peticiones html y relación con la API de Twitch, como toda la parte de relación con el DOM.

La aplicación que voy a presentar aquí se relaciona tanto con la API como con el DOM con VanillaJS en vez de jQuery.

Por último, comentar que esta aplicación me ha costado muchas horas de trabajo e incluso momentos de frustración. Por suerte no he empezado de cero, ya que aunque esté en jQuery, es más fácil transformar algo que no crearlo de cero.

- #### Declaración inicial de variables

```
var channels = ["freecodecamp", "NBA", "OgamingSC2", "cretetion", "storbeck", "habathcx", "RobotCaleb", "noobs2ninjas", "LVPes3"];
var game;
var status;

```

Aquí se declara un array con los canales que se quieren mostrar
Y se declaran dos variables más que se utilizarán en una de las funciones importantes de la aplicación.

- #### Función _getChannelInfo_

**Primera parte:**
```
for (i = 0;i < channels.length;++i) {

       var xmlhttp = new XMLHttpRequest(),
       url = 'https://api.twitch.tv/kraken/streams/'+ channels[i] + '?client_id=7e0leu1c5jgpvj7iviwcij03omfiqt';

       xmlhttp.onreadystatechange = function() {

            if (this.readyState == 4 && this.status == 200) {
            var streamers = JSON.parse(this.responseText);

                if (streamers.stream === null) {
                    game = "Offline";
                    status = "offline";
                } else if (streamers.stream === undefined) {
                    game = "Not found";
                    status = "offline";
                } else {
                    game = streamers.stream.game;
                    status = "online";
                };
            }    
    }
    xmlhttp.open('GET', url, true);
    xmlhttp.send();
}
```
Se utiliza un bucle for para recorrer el array _channels_.

Dentro del bucle se declara un [nuevo objeto _XMLHttpRequest_](https://www.w3schools.com/xml/dom_httprequest.asp) que se almacena en la variable _xmlhttp_. Este objeto _XMLHttpRequest_ sirve para enviar peticiones HTTP de manera asíncrona a un servidor y [hace de mandar solicitudes HTTP algo muy fácil.  Solo crea una instancia del objecto, abre una URL, y envía la solicitud.  El estatus HTTP y el contenido del resultado, estan disponibles en el objecto cuando se termina la transacción](https://developer.mozilla.org/es/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest).

También se declara otra variable, _url_ con la llamada a la API de Twitch. En este bucle se llama a los _streams_ para ver qué canales están retransmitiendo en streaming. Además, se añade al final de la llamada un ID de cliente que es el cliente público que te da la aplicación de Twitch una vez te das de alta en la [parte de desarrolladores de Twitch](https://dev.twitch.tv/docs/authentication).

La respuesta de esta llamada es un contenido en JSON que se almacena en la variable _url_.

Posteriormente, aparece toda la manipulación del estado de la petición y de si los datos se envían y reciben correctamente. Para comprobar el estatus de la petición HTTP se crea una función de la instancia _xmlhttp_ llamada _onreadystatechange_.
En esta función se comprueba que el estado de la petición y que el estatus devuelto por la misma sean correctos y no de error: _this.readyState == 4 && this.status == 200_.

Después se crea una variable _streamers_ donde se almacena la respuesta de la petición HTTP. Como dicha respuesta se hace en contenido JSON ya que Twitch lo hace así, [se debe parsear la respuesta para convertirlo en contenido Javascript manejable posteriormente en nuestra aplicación](https://www.w3schools.com/js/js_json_parse.asp).

Dentro del if que comprueba el estado de la petición se crean otros if/else para extraer los datos de la respuesta a la petición y asignar dichos resultados a varias variables, tales como _game_ y _status_.Si la variable _stream_ es igual a "null", se establecen dichas variables en offline. Si, en cambio, la respuesta es "undefined", la variable _game_ se pone en "not found" y _status_ en online. Por último, si _stream_ no es "null" ni "undefined", es decir, será que está en ese mismo momento "online" se coge la variable _game_ dentro de stream y se añade a _game_ y, además, se pone _status_ en "online".

La última parte es para indicar cómo se hace la petición al servidor para extraer los datos. En este caso, se hace llamando a dos funciones, _request.open_ y _request.send_. En la primera se indica que será un petición GET, se indica una URL (que está en la almacenada en la variable _url_) y por último se añade la opción true que quiere decir que la petición será asíncrona. En la función _send_ se indica que se envíe la petición usada por GET.


**Segunda parte:**

```
for (i = 0;i < channels.length;++i) {

      var xmlhttp = new XMLHttpRequest(),
      url = 'https://api.twitch.tv/kraken/channels/'+ channels[i] + '?client_id=7e0leu1c5jgpvj7iviwcij03omfiqt';

      xmlhttp.onreadystatechange = function() {

        if (this.readyState == 4 && this.status == 200) {

          var channel1 = JSON.parse(this.responseText);

          var logo = channel1.logo != null ? channel1.logo : "https://dummyimage.com/50x50/ecf0e7/5c5457.jpg&text=0x3F",
          name = channel1.display_name != null ? channel1.display_name : channel1,
          description = status === "online" ? ': ' + channel1.status : "";

          html = '<div class="row ' + status + '" id="canal"><div class="col-xs-2 col-md-2" id="icon"><img src="' +
          logo + '" class="logo"></div><div class="col-xs-5 col-md-3" id="name"><a href="' +
          channel1.url + '" target="_blank">' + name + '</a></div><div class="col-xs-5 col-md-7" id="streaming">'+
          game + '<span class="hidden-xs">' + description + '</span></div></div>';

         document.getElementById('display').innerHTML += html;
        }
    }
    xmlhttp.open('GET', url, true);
    xmlhttp.send();
}
}
```

La primera parte de este bucle es igual a la del anterior, tan solo cambia que en vez de _streams_ en la url se buscan los canales _channels_. Esta petición también devuelve contenido JSON con diferentes valores, que serán los que se utilizarán en el cuerpo del if para extraer el logo, el nombre, etc., de los canales.


En la parte central del if se declara una variable llamada _logo_ donde se almacena el logo, el nombre,  y la descripción. Si estos datos no son iguales a "null" se establece el varor de cada de ellas.  

También se declara otra variable llamada _html_ en la que se añaden diversas clases, como _row_, _canal_ y dos atributos _id_ y _streaming_ configurándolos con ciertas especificaciones CSS para crear la parte visual de imágenes de la aplicación.  Además, se le añaden todas las variables creadas anteriormente como _status_, _logo_, _name_, _game_ y _description_.

La última parte de este if es añadir la variable _html_ al DOM a través del atributo _display_ que hay en el archivo index.html y utilizando el método de VanillaJS _innerHTML_. [La propiedad _innerHTML_ establece(como en este caso) o devuelve el contenido HTML de un elemento, en este caso _display_.](https://www.w3schools.com/jsref/prop_html_innerhtml.asp)

- #### Carga de contenido principal al DOM

Para cargar contenido al DOM se utiliza un [disparador de eventos con el evento _DOMCotentLoaded_](https://felipefcor.github.io/2018-07-02-C%C3%B3mo-modificar-el-DOM-con-Javascript/) y llamando a una [función callback](https://zellwk.com/blog/callbacks/?ck_subscriber_id=237916348), que será la función principal _getChannelInfo_ que hace las peticiones HTML.
```
document.addEventListener("DOMContentLoaded", function(){
      getChannelInfo();
});
```

- #### Pulsar los botones

La última sección de la aplicación es generar el código que gestionará cuando se pulsen los botones de la parte superior: "all""online", "offline".

```
var selector = document.getElementsByClassName('selector');
var numSelect = selector.length;
var i;

    for (i = 0 ; i < numSelect; i++) {

      selector[i].addEventListener('click', callback);

    }

function callback () {

      var allChannels = document.querySelectorAll('#canal');
      var onChannel = document.querySelectorAll('.online');
      var offChannel = document.querySelectorAll('.offline');
      var status = this.getAttribute('id');

              if (status==="all"){

                allChannels.forEach( function (selector) {
                  selector.classList.remove("hidden");
                })


              }
              else if (status==="online") {
                  onChannel.forEach( function (selector) {
                    selector.classList.remove("hidden");
                   })
                  offChannel.forEach( function (selector) {
                    selector.classList.add("hidden");
                })
                }
              else if (status==="offline"){
                  onChannel.forEach( function (selector) {
                    selector.classList.add("hidden");
                  })
                  offChannel.forEach( function (selector) {
                    selector.classList.remove("hidden");
                })
              }

      selector.removeEventListener('click', callback);

};
```
En la primera parte de esta sección se crean tres variables, _selector_, _numSelect_ e _i_. La variable _selector_ almacena la clase "selector" que se ha seleccionado con la opción de _document.getElementsByClassName_. La variable _numSelect_ almacena la longitud de la variable _selector_ y la variable _i_ se utiliza para el bucle for.

Dentro del for se hace una llamada al evento _click_ con una función llamada _callback_, que es donde se añade todo el código.

##### Función _callback_:

Se declaran 4 variables para seleccionar el ID _canal_ y las clases _online_ y _offline_ que se han creado antes en el anterior bucle for. Además, se crea una variable _status_ seleccionando todos los atributos _id_ que hay declarados en el HTML.

Una vez se han declarado esas variables se utilizan con varios if-else.

Si el status del ID seleccionado coincide con _all_ se le pasa un bucle _forEach_ a la variable de todos los canales y se elimina la clase _hidden_ de todos ellos. Esa clase está declarada en el bootstrap.

Si, en cambio, el status es _online_ se le pasa otro bucle a los canales que estaban online según la clase .online y se elimina la clase _hidden_. Lo mismo pero a la inversa con los canales _offline_. Así se logra que se vean los canales online y se escondan los offline.

El último else-if es igual que el anterior pero a la inversa, en función de si el status es _offline_.

Por último, se elimina el el evento _click_.


