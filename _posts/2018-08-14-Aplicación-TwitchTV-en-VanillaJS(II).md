---
layout: post
title: Aplicación Twitch TV en VanillaJS (II)
tags: [javascript, jquery, vanillajs, ajax, requests, xmlhttprequest, twitchtv, DOM]
---
### [Aprende Javascript con MentoringJS - Step 7 ](http://MentoringJS.com)
En este artículo voy a realizar una explicación de la aplicación [Twitch TV](https://www.twitch.tv/) hecha con [VanillaJS](http://vanilla-js.com/) que se se puede ver en [mi repositorio](https://github.com/felipefcor/twitchtv) Iré explicando cada parte de la misma en detalle.

Este artículo es la segunda parte de un proceso que me ha hecho llegar a la aplicación que voy a explicar. La primera parte fue bastante dura pero me ha permitido descubrir y afianzar algunos conocimientos básicos que, seguramente, seguiré utilizando en un futuro. Debo decir también que para esta nueva versión debo dar las gracias a @jordigomper por su inestimable ayuda. Para comprender los errores de la primera versión, para comprender mejor la aplicación y para su ayuda para algunas partes en las que estaba perdido. Gracias Jordi.


El primer paso es realizar un fork del repositorio de la aplicación Twitch TV de [Rocio M. Sebastian](https://github.com/rocio-m-sebastian/twitchtv). En este repositorio la aplicación está hecha en jQuery, tanto las peticiones html y relación con la API de Twitch, como toda la parte de relación con el DOM.

La aplicación que voy a presentar aquí se relaciona tanto con la API como con el DOM con VanillaJS en vez de jQuery.

- #### Declaración inicial de variables

```
var channels = ["freecodecamp", "NBA", "OgamingSC2", "cretetion", "storbeck", "habathcx", "RobotCaleb", "noobs2ninjas", "LVPes3"];
var game;
var status;

```
Aquí se declara un array con los canales que se quieren mostrar
Y se declaran dos variables más que se utilizarán en una de las funciones importantes de la aplicación.

### Primera parte:

- #### Función _getChannelInfo_

```
channels.forEach(function(channel) {

        function makeURL(type, name) {
            return 'https://api.twitch.tv/kraken/' + type + '/' + name + '?client_id=7e0leu1c5jgpvj7iviwcij03omfiqt';
        }

        var xmlhttpStreams = new XMLHttpRequest();
        var url = makeURL("streams", channel);
        xmlhttpStreams.onreadystatechange = function() {
            if (xmlhttpStreams.readyState == 4 && this.status == 200) {
                var data = JSON.parse(xmlhttpStreams.responseText);
                var status, game;

                if(data.stream === null) {
                    status = 'offline';
                    game = 'offline';
                }
                else if(data.stream === undefined) {
                    status = 'offline';
                    game = 'not found';
                }
                else {
                    status = 'online';
                    game = data.stream.game;
                }

                var xmlhttpChannels = new XMLHttpRequest();
                var url = makeURL("channels", channel);
                xmlhttpChannels.onreadystatechange = function() {
                    if (xmlhttpChannels.readyState == 4 && this.status == 200) {
                        var data = JSON.parse(xmlhttpChannels.responseText);
                        var logo = null;
                        var name = null;
                        var description = '';

                        if(data.logo !== null){
                            logo = data.logo;
                            }
                        if(data.name !== null) {
                            name = data.name;
                            }
                        if(status === 'online') {
                            description = data.status;
                            }

                        // crea un elemento HTML
                        var div = document.createElement("div");

                        // Introduce información dentro del element HTML
                        div.innerHTML = '<div class="row ' + status + '" id="canal"><div class="col-xs-2 col-md-2" id="icon"><img src="' +
								          logo + '" class="logo"></div><div class="col-xs-5 col-md-3" id="name"><a href="' +
								          data.url + '" target="_blank">' + name + '</a></div><div class="col-xs-5 col-md-7" id="streaming">'+
								          game + '<span class="hidden-xs">' + description + '</span></div></div>';

                        // busca un element dentro del HTML con una ID concrteeta
                        var display = document.getElementById("display");

                        // introduce un elemento dentro de otro elemento
                        if(status === 'online') display.prepend(div);
                        else display.appendChild(div);
                     }
                };
                xmlhttpChannels.open("GET", url, true);
                xmlhttpChannels.send();

            }
        };
        xmlhttpStreams.open("GET", url, true);
        xmlhttpStreams.send();


    })
}
```

En esta primera parte hay varias partes diferenciadas, que voy a diseccionar para poder explicarlas mejor:

##### Parte 1
```
channels.forEach(function(channel) {}
```

Con el método [_forEach_](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Objetos_globales/Array/forEach) conseguimos ejecutar la función indicada(_channel_) una vez por cada elemento el array _channels_.

##### Parte 2
```
function makeURL(type, name) {
            return 'https://api.twitch.tv/kraken/' + type + '/' + name + '?client_id=7e0leu1c5jgpvj7iviwcij03omfiqt';
        }
```
La función _makeURL_ nos sirve para hacer la petición a la API de Twitch pasándole dos parámetros, el tipo (_type_) y el nombre (_name_). La función devuelve la url de la API de Twitch sumándole los dos parámetros y un ID de cliente que es el cliente público que te da la aplicación de Twitch una vez te das de alta en la [parte de desarrolladores de Twitch](https://dev.twitch.tv/docs/authentication).


##### Parte 3
```
var xmlhttpStreams = new XMLHttpRequest();
var url = makeURL("streams", channel);
xmlhttpStreams.onreadystatechange = function() {
            if (xmlhttpStreams.readyState == 4 && this.status == 200) {
                var data = JSON.parse(xmlhttpStreams.responseText);
                var status, game;

                if(data.stream === null) {
                    status = 'offline';
                    game = 'offline';
                }
                else if(data.stream === undefined) {
                    status = 'offline';
                    game = 'not found';
                }
                else {
                    status = 'online';
                    game = data.stream.game;
                }

xmlhttpStreams.open("GET", url, true);
xmlhttpStreams.send();
```
En esta parte se declara un [nuevo objeto _XMLHttpRequest_](https://www.w3schools.com/xml/dom_httprequest.asp) que se almacena en la variable _xmlhttpStreams_. Este objeto _XMLHttpRequest_ sirve para enviar peticiones HTTP de manera asíncrona a un servidor y [hace de mandar solicitudes HTTP algo muy fácil.  Solo crea una instancia del objecto, abre una URL, y envía la solicitud.  El estatus HTTP y el contenido del resultado, estan disponibles en el objecto cuando se termina la transacción](https://developer.mozilla.org/es/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest).

También se declara otra variable, _url_ con la llamada a la API de Twitch utilizando la función _makeURL_ antes declarada y pasándoles dos parámetros, el primero en este caso _streams_. En este parte de código se llama a los _streams_ para ver qué canales están retransmitiendo en streaming.

Posteriormente, aparece toda la manipulación del estado de la petición y de si los datos se envían y reciben correctamente. Para comprobar el estatus de la petición HTTP se crea una función de la instancia _xmlhttpStreams_ llamada _onreadystatechange_.
En esta función se comprueba que el estado de la petición y que el estatus devuelto por la misma sean correctos y no de error: _xmlhttpStreams.readyState== 4 && this.status == 200_.

Después se crea una variable _data_ donde se almacena la respuesta de la petición HTTP. Como dicha respuesta se hace en contenido JSON ya que Twitch lo hace así, [se debe _parsear_ la respuesta para convertirlo en contenido Javascript manejable posteriormente en nuestra aplicación](https://www.w3schools.com/js/js_json_parse.asp).

A continuación se declaran dos variables que se van a utilizar para almacenar el contenido de ciertas características que vamos a extraer de los canales que están en _streaming_, _data_ y _status_.

Dentro del if que comprueba el estado de la petición se crean otros if/else para extraer los datos de la respuesta a la petición y asignar dichos resultados a varias variables, tales como _game_ y _status_.Si la variable _stream_ es igual a "null", se establecen dichas variables en offline. Si, en cambio, la respuesta es "undefined", la variable _game_ se pone en "not found" y _status_ en online. Por último, si _stream_ no es "null" ni "undefined", es decir, será que está en ese mismo momento "online" se coge la variable _game_ dentro de stream y se añade a _game_ y, además, se pone _status_ en "online".

La última parte es para indicar cómo se hace la petición al servidor para extraer los datos. En este caso, se hace llamando a dos funciones, _request.open_ y _request.send_. En la primera se indica que será un petición GET, se indica una URL (que está en la almacenada en la variable _url_) y por último se añade la opción true que quiere decir que la petición será asíncrona. En la función _send_ se indica que se envíe la petición usada por GET.

##### Parte 4
```
var xmlhttpChannels = new XMLHttpRequest();
var url = makeURL("channels", channel)
xmlhttpChannels.onreadystatechange = function() {

        if (xmlhttpChannels.readyState == 4 && this.status == 200) {
            var data = JSON.parse(xmlhttpChannels.responseText);
            var logo = null;
            var name = null;
            var description = '';

                   if(data.logo !== null){
                           logo = data.logo;
                           }
                   if(data.name !== null) {
                           name = data.name;
                           }
                   if(status === 'online') {
                           description = data.status;
                           }

};

xmlhttpChannels.open("GET", url, true);
xmlhttpChannels.send();

}
```
La primera parte de esta sección de código es igual a la anterior, hasta que llega a la declaración de las variables _logo_, _name_ y _description_. Tan solo cambia que ahora pedimos datos de los canales a la API de Twitch en vez de pedir datos de los streams.

Estas tres variables se declaran, las dos primeras como "null" y la última vacía ya que vamos a utilizarlas y rellenarlas a continuación.

En los ifs siguientes se interactua con los datos conseguidos de la petición a la API. Se indica que si el logo y el nombre no son igual a "null" el contenido de estos se asigna a las variables _logo_ y _name_. Si el estatus es _online_, se le asigna dicho status a la variable _description_.

La última parte es para indicar cómo se hace la petición al servidor para extraer los datos. En este caso, se hace llamando a dos funciones, _request.open_ y _request.send_. En la primera se indica que será un petición GET, se indica una URL (que está en la almacenada en la variable _url_) y por último se añade la opción true que quiere decir que la petición será asíncrona. En la función _send_ se indica que se envíe la petición usada por GET.


##### Parte 5
```
var div = document.createElement("div");

div.innerHTML = '<div class="row ' + status + '" id="canal"><div class="col-xs-2 col-md-2" id="icon"><img src="' + logo + '" class="logo"></div><div class="col-xs-5 col-md-3" id="name"><a href="' +  data.url + '" target="_blank">' + name + '</a></div><div class="col-xs-5 col-md-7" id="streaming">'+ game + '<span class="hidden-xs">' + description + '</span></div></div>';

var display = document.getElementById("display");

                  if(status === 'online'){
                     display.prepend(div);
                  }
                  else {
                    display.appendChild(div);
                  }
```

Esta sección de código está justo a continuación de la parte 4 y la he diferenciado de esta porque trata de un tema diferente a hacer una petición a un servidor, extraer y enviar datos al mismo.

Esta sección es la que se encarga de añadir al DOM el contenido obtenido a través de todas las peticiones a la parte de streams y canales de la API de Twitch.

Primero de todo se crea un elemento HTML llamado div.

A continuación se introduce información dentro del elemento HTML utilizando el método de VanillaJS _innerHTML_. [La propiedad _innerHTML_ establece(como en este caso) o devuelve el contenido HTML de un elemento, en este caso el de div](https://www.w3schools.com/jsref/prop_html_innerhtml.asp). En este _div.html_ se añaden diversas clases, como _row_, _canal_ y dos atributos _id_ y _streaming_ configurándolos con ciertas especificaciones CSS para crear la parte visual de imágenes de la aplicación.  Además, se le añaden todas las variables creadas anteriormente como _status_, _logo_, _name_, _game_ y _description_.

Acto seguido, se busca un elemento dentro del HTML con una ID concreta (display) y se almacena en la variable _display_.

Por último, se introduce un elemento dentro de otro elemento siempre y cuando el status sea igual a "online". En el caso de serlo, [se añade con el método _prepend_](https://developer.mozilla.org/en-US/docs/Web/API/ParentNode/prepend) y el listado a mostrar que hay dentro de _div_. Con esto se quiere ordenar el listado de canales y mostrar los que están online primeros. En el caso de que el status no sea igual a "online", se añaden justo debajo con el [método _appendChild_](https://developer.mozilla.org/en-US/docs/Web/API/ParentNode/append).





### Segunda parte:

#### Carga de contenido principal al DOM

Para cargar contenido al DOM se utiliza un [disparador de eventos con el evento _DOMCotentLoaded_](https://felipefcor.github.io/2018-07-02-C%C3%B3mo-modificar-el-DOM-con-Javascript/) y llamando a una [función callback](https://zellwk.com/blog/callbacks/?ck_subscriber_id=237916348), que será la función principal _getChannelInfo_ que hace las peticiones HTML.
```
document.addEventListener("DOMContentLoaded", function(){
      getChannelInfo();
});
```



### Tercera parte:



#### Pulsar los botones

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

-----------------
