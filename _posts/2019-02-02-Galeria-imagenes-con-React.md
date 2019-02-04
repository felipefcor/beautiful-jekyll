---
layout: post
title: Galería de imágenes con React
tags: [javascript, ES6, React, css, sass, componentes, UI, deploy, surge]
---
### [Aprende Javascript con MentoringJS - Step 12 ](http://MentoringJS.com)
En este nuevo artículo voy a explicar cómo he realizado una galería de imágenes con React y CSS (Sass).

El resultado se puede ver desplegado en surge:

[http://react-gallery.surge.sh](http://react-gallery.surge.sh/)

[Repostiorio con el código de la galería de imágenes](https://github.com/felipefcor/Image-gallery-with-React)

Para realizar esta galería de imágenes con React me he basado en el [tutorial de Alex Devero](https://www.linkedin.com/pulse/learn-react-practice-create-stunning-image-gallery-alex-devero/). En este artículo voy a intentar diseccionar dicho artículo y explicar qué he modificado y cómo lo he hecho para crear mi propia galería de imágenes con React.

---

En primer lugar, voy a intentar explicar cómo he intentado llevar a cabo el _tutorial_ antes mencionado. En ese artículo la composición de la galería se hace con un index.html, un index.js y un style.css. Además de estos archivos, se llama a las librerías correspondientes (Bootstrap, FontAwesome, React). En principio con esta configuración la galería debería funcionar. Yo hice la primera versión tal y como ponía en el artículo pero nunca me llegó a funcionar. El motivo por el cuál no me funcionó no lo acabé de descubrir, aunque probablemente fuese por las versiones antiguas de las librerías.
> Actualización 4/2/18: Muy probablemente el motivo por el cuál no llegó a funcionar la primera versión fue porque no configuré Babel correctamente. En el artículo original se indica que se va a utilizar dicho transpilador, aunque no dice cómo.

En cualquier caso, tomé la decisión de realizar este proyecto con las herramientas nativas de React y utilicé [Create-react-app](https://github.com/facebook/create-react-app). Además, intenté aprovechar la ocasión para utilizar la estructura del proyecto que [revisé en un artículo anterior](https://felipefcor.github.io/2019-01-17-React-desde-dentro/) y también intentar _refactorizar_ el código con algunas modificaciones típicas de ES6 que [también revisé en otro artículo](https://felipefcor.github.io/2019-01-21-Javascript-moderno-ES6/). Esta última parte no he tenido que modificarla mucho ya que la gran parte del código estaba en ES6. De todos modos, como el artículo es bastante antiguo, sí que he podido hacer algunos pequeños cambios que me han parecido interesantes y me han servido de aprendizaje.

---

Para empezar, voy a ir poco a poco enseñando la estructura de la aplicación.

<img width="150" height="300" src="https://raw.githubusercontent.com/felipefcor/Image-gallery-with-React/master/structure.png">

Como se puede ver en la imagen, he creado dos carpetas en el directorio _src_, una llamada _components_ y otra _containers_. En la primera he puesto dos componentes sin estado (presentacionales), _GalleryImage.js y GalleryModal.js_. Mientras que en la segunda he puesto el componente principal _Gallery.js_ en el que se encuentra el estado de la aplicación y es el componente que se renderiza a _App.js_.

>Esta división está basada en la [idea de Dan Abramov](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0) comentada en un [artículo anterior](https://felipefcor.github.io/2019-01-17-React-desde-dentro/).

Además de esto, el archivo CSS principal (_Gallery.css_) está en la misma carpeta que su archivo homólogo.

Por último, hay un archivo index.js que está en la misma altura jerárquica que el directorio _src_ y hay otro directorio _public_ que es donde está el index.html.

---

Yendo al códgio, podemos comenzar de menor a mayor complejidad. Por eso, voy a comenzar con el index.html:
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="shortcut icon" href="%PUBLIC_URL%/favicon.ico" />
    <!-- Latest compiled and minified CSS -->
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.6.3/css/all.css" integrity="sha384-UHRtZLI+pbxtHCWp1t77Bi1L4ZtiqrqD80Kn4Z8NTSRyMA2Fd33n5dQ8lWUE00s/" crossorigin="anonymous">

    <meta
      name="viewport"
      content="width=device-width, initial-scale=1, shrink-to-fit=no"
    />
    <meta name="theme-color" content="#000000" />
        <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />

    <title>Gallery of images created with React and CSS</title>
  </head>
  <body>

    <div id="root"></div>


  </body>
</html>

```

Este archivo es la base del índex que la aplicación _create-react-app_ genera. Le he quitado algunas líneas para no hacerlo tan exhaustivo.

Lo importante de este archivo es que se llama a las librerías Bootstrap y FontAwesome.

>Un dato importante es que, para que funcione la aplicación hay que utilizar una versión antigua de Bootstrap (3.3.7) ya que muchas clases están adaptadas a esta versión.

>Otro apunte significativo es que, inicialmente intenté descargarme Bootstrap y FontAwesome con las herramientas que _npm_ ofrecía y que React adaptaba. Tras varias revisiones consideré que la complejidad de la aplicación aumentaba demasiado por lo que lo dejé tal y como está ahora.

Pasamos al archivo _index.js_:
```
import React from 'react';
import ReactDOM from 'react-dom';
import App from './containers/App';


ReactDOM.render(<App />, document.getElementById('root'));
```

En este índice se hacen tres importaciones, dos de React y una del componente App, que es el componente que renderiza toda la aplicación. En este archivo está la renderización al DOM.


Seguimos con el componente _App.js_:
```
import React, { Component } from 'react';
import Gallery from './Gallery';



export default class App extends Component {
  render() {
    return (
      <div >
         <Gallery   />

       </div>
    );
  }
}

```

Este componente tan solo tiene un cometido, renderizar el componente Gallery que es dónde **toda la aplicación converge**. Podría haber desechado este componente y renderizar en el index.js el componente _Gallery_, pero he decidido mantener la estructura de _create-react-app_.

---

**Parte CSS**

Antes de entrar en los tres componentes React quiero hacer un inciso y revisar la parte del CSS. Aunque esta parte no es la principal considero que es muy importante para el buen funcionamiento de la galería, tanto a nivel de UI general como a la hora de mostrar y esconder el _modal_ de las imágenes cuando se pincha sobre ellas.

Otro aspecto importante es que el CSS está hecho con Sass y procesado posteriormente a CSS. A continuación, empiezo a diseccionar el código Sass.

Para empezar se declaran unas variables del color y la transición para la clase _modal_close_. Y una tercera variable para el tamaño del borde de la clase _modal-body_.
```
// Main Sass file
// Variables
$black: #111;
$radius: 4px;
$transition: all .25s ease-in-out;
```

A continuación tenemos algunas correcciones para los elementos _html_ y _body_ para hacer que la galería sea a tamaño 100%. También se cambia la fuente y el tamaño de la tipografía.
```
html,
body {
  min-height: 100%;
  height: 100%;
}

html {
  font-size: 16px;
}

body {
  position: relative;
  font-size: 100%;
}

```

En relación al container de la galería, se añade _padding_ para separar dicho container y el contenido de la galería.

```
.gallery-container {
  padding-top: .9375rem;
}
```

Posteriormente se sigue estilando las _cards_. Se pone la posición en relativo y se espacian todas las _cards_ con _margin-bottom_.
```
.gallery-card {
  position: relative;
  overflow: hidden;
  margin-bottom: 1.875rem;
}
```
Se sigue estilando las imágenes _thumbnail_. Se ponen el máximo tamaño al 100% para que no se sobrepongan a las _cards_; después para mantener el ratio de la imágenes intacto se pone el _height_ en auto; y por útlimo, se hace el borde redondeado.
```
.gallery-thumbnail {
  max-width: 100%;
  height: auto;
  border-radius: $radius;
}
```

El siguiente punto es el estilado de los iconos de las _cards_.
```
.card-icon-open {
  display: block;
  position: absolute;
  top: 50%;
  left: 50%;
  font-size: 2rem;
  color: #fff;
  cursor: pointer;
  opacity: 0;
  transform: translate(-50%, -50%);
  transition: $transition;

  &:focus,
  &:hover {
    color: $black;
  }
}

.gallery-thumbnail:focus ~ .card-icon-open,
.gallery-thumbnail:hover ~ .card-icon-open,
.gallery-thumbnail ~ .card-icon-open:focus,
.gallery-thumbnail ~ .card-icon-open:hover {
  opacity: 1;
}
```
Aquí se utiliza un icono de FontAwesome y se añade en el medio del _thumbnail_. Para ponerlo en el medio se establece una posición con las propiedades _position_, _top_, _left_ y _trasnform_.

Inicialmente, el icono no se verá hasta que se pase el ratón por encima. Eso se consigue con las propiedades _display_ y _opacity_. Además, cuando se ponga encima del icono este cambiará de color de blanco a negro. Todo ello se configura para que sea sutil con la propiedad _transition_.

A continuación tan solo quedará la última parte, la que afecta a los estilos del _modal_, como hace la transición, el propio cuerpo del mismo y como se cierra.

Para empezar, la transición.
```
.modal-overlay {
  position: absolute;
  top: 0;
  left: 0;
  z-index: 10;
  width: 100%;
  height: 100%;
  background: rgba(21,21,21,.75);
}
```
Este _overlay_ muestra lo que hay detrás del modal al abrirse este. Convierte la pantalla en semitransparente al abrirse dicho modal.

En el estilo del cuerpo del modal se utiliza el modelo que se ha utilizado para expander el icono, es decir, la posición absoluta, izquierda y arriba al 50%, y la transformación  con _translate_. Se pone el _z-index_ más grande que en _overlay_ ya que sino este lo cubriría. Se pone la propiedad _overflow_ a escondido por si acaso y se redondea el borde.
```
.modal-body {
  position: absolute;
  top: 50%;
  left: 50%;
  z-index: 11;
  padding: 0;
  overflow: auto;
  max-width: 100%;
  max-height: 100%;
  border-radius: $radius;
  transform: translate(-50%, -50%);
}
```

Por último, el estilado del icono para cerrar el modal.
```
.modal-close {
  position: absolute;
  top: 0;
  right: 8px;
  font-size: 2rem;
  color: $black;
  transition: $transition;

  &:focus,
  &:hover {
    color: #fff;
  }
```
Se pone la posición del icono arriba a la derecha, de manera absoluta. Con un tamaño de 2rem y un color negro por defecto. Cuando se pone el ratón encima se pone blanco. Se establecen las transición para ese cambio.

---

**Parte React**

Llegado este punto toca adentrarse en el código React. Para ello, voy a comenzar con el componente más sencillo, _GalleryImage.js_.
```
import React, { Component } from 'react';

export default class GalleryImage extends Component {

    render() {
        return (

                <img className={this.props.className} src={this.props.src} alt={this.props.alt} />

        )
    }
}

```
Este es un componente puramente [presentacional](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0).

Hay una clase (aunque podría haber sido una función) que devuelve un _tag_ imagen con 3 _props_: una clase de CSS, otro para identificar a la imagen y una descripción de la imagen con el atributo _alt_.

El segundo componente, que también podríamos denominar presentacional, es el que gestiona cuando el modal está abierto, _GalleryModal.js_:
```
import React, { Component } from 'react';
import   '../containers/Gallery.css';


export default class GalleryModal extends Component {
    render() {

      if (this.props.show === false) {
        return null;
      }

      return(

        <div show={this.props.isOpen} className='modal-overlay' onClick={this.props.onClick} name={this.props.name}>

          <div className='modal-body'>

            <a className='modal-close' href='#url' onClick={this.props.onClick}>
              <span className='fa fa-times'></span>
            </a>

            <img src={this.props.src} alt='' />

          </div>

        </div>
      )
    }
  }

```

Para empezar, hay un _import_ más que el primer componente. Este _import_ hace referencia al archivo CSS principal ya que se llamará a algunas clases CSS para estilar el modal.

Adentrándonos en el código, lo primero que vemos es que hay un condicional en la parte del _render_:
```
if (this.props.show === false) {
  return null;
}
```
Este condicional es el encargado de mostrar o esconder el modal. Cuando _show_ es verdadero, el modal se muestra; y cuando es falso, el modal no se muestra. Aunque el condicional esté aquí, el que [gestiona el estado del modal es el componente padre](https://daveceddia.com/open-modal-in-react/).

En la parte del _return_ se devuelve varios elementos.

En primer lugar, un _div_ con 3 _props_ y una clase de CSS. Este _div_ servirá para la transición una vez se pinche en la imagen para abrirla y mostrar el _thumbnail_. El primer _prop_  sirve para gestionar el estado del modal cuando se llame a este componente. El segundo es para gestionar un evento y el tercero para asignarle un nombre.
```
      <div show={this.props.isOpen} className='modal-overlay' onClick={this.props.onClick} name={this.props.name}>
```

Dentro de este _div_ hay otro _div_ con una clase de CSS en la que se especifican los estilos del cuerpo del modal una vez abierto. Además, hay una etiqueta _a_ que se utiliza para el icono de aspa que sirve para cerrar el modal. En esa etiqueta se añade una clase CSS con sus estilos y un _prop_ con un evento.

Por último, hay una etiqueta _img_ con un prop de una imagen.

```
<div className='modal-body'>

  <a className='modal-close' href='#url' onClick={this.props.onClick}>
    <span className='fa fa-times'></span>
  </a>

  <img src={this.props.src} alt='' />

</div>
```
<br>
>Para acabar, debo añadir que con la estructura del artículo original me aparecían errores en la consola. Es probable que debido a la diferencia de versión de React.
El error más significativo ha sido el que hacía referencia a _isOpen_. Me daba continuamente error al asignar en el _div_ principal _isOpen_. Por ello, tras revisarlo, encontré [un artículo de Dave Ceddia que hablaba sobre crear un modal en React](https://daveceddia.com/open-modal-in-react/) y dónde se especificaba de una manera diferente, cambiándolo por show. Lo cambié y el error desapareció.


Por último, toca presentar el componente principal de la aplicación, _Gallery.js_.
Este componente es el que gestiona todo el estado por lo que, según la [dicotomía establecida por Dan Abramov](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)., sería un componte _Container_.

```
import React, { Component } from 'react';
import GalleryImage  from '../components/GalleryImage';
import GalleryModal from '../components/GalleryModal';

import    './Gallery.css';


 let imgUrls = [
    'https://source.unsplash.com/k3IogSsONd4/800x600',
    'https://source.unsplash.com/gThfDnqgfMw/800x600',
    'https://source.unsplash.com/_1x_x8Vtg2w/800x600',
    'https://source.unsplash.com/TFP_s4_jRuE/800x600',
    'https://source.unsplash.com/pElM4yerF5Q/800x600',
    'https://source.unsplash.com/sFsy8CKyQ5c/800x600',
    'https://source.unsplash.com/0WGucY1VHI0/800x600',
    'https://source.unsplash.com/1ciHU-qPifY/800x600',
    'https://source.unsplash.com/JZCJotPa96c/800x600',
    'https://source.unsplash.com/8X19catOuNI/800x600',
    'https://source.unsplash.com/_GDff35-Pa8/800x600',
    'https://source.unsplash.com/XYok1nBGvhk/800x600'
 ]



export default class Gallery extends Component{
    constructor(props) {
      super(props);

      this.state = {
        isOpen: false,
        url: ''
      }

  }

    render() {
      return(

        <div className='container-fluid gallery-container'>

          <div className='row'>
            {
              imgUrls.map((url, index) => {
                 return (
                 <div key={index} className='col-sm-6 col-md-3 col-xl-2'>

                    <div className='gallery-card'>

                      <GalleryImage  className='gallery-thumbnail' src={url} alt={'Image number ' + (index + 1)} />

                      <span className='card-icon-open fa fa-expand-arrows-alt' value={url} onClick={() => this.openModal(url)}></span>

                    </div>
                  </div>
                 )
               })
             }
          </div>

          <GalleryModal show={this.state.isOpen} onClick={this.closeModal} src={this.state.url} />

        </div>
      )
    }


    // Function for opening modal dialog
    // I change the function to a arrow functions to avoid to declare the binding in the constructor
    openModal = (url) => {

       this.setState({
         isOpen: true,
         url: url
       });
     }

    // Function for closing modal dialog
    // I change the function to a arrow functions to avoid to declare the binding in the constructor
    closeModal = () => {

       this.setState({
        isOpen: false,
        url: ''
      });
    }
  }

```

Para empezar, están los _imports_ habituales. El de React, otros dos a los otros dos componentes presentacionales y el último al archivo de estilos CSS.

>Respecto a este último, quería comentar que debido a que he adaptado este proyecto de ficheros "planos" Javascript a utilizar la herramienta _create-react-app_ me he visto obligado a rehacer o revisar cómo realizar los _imports_ a los módulos CSS. Para eso me ha ayudado mucho la [web oficial que tiene _create-react-app_   ](https://facebook.github.io/create-react-app/docs/adding-a-css-modules-stylesheet).

Después de los _imports_ se declara un _array_ con todas las imágenes que queremos mostrar en la Galería.

Acto seguido llegamos ya a la clase principal. En esta, se comienza declarando los métodos _constructor_ y _super_ con una _prop_ y un estado. En el método _state_ se declaran dos propiedades, _isOpen_ y _url_. Al inicializar la aplicación _isOpen_ es falso y _url_ está vacía. Esto sirve para no mostrar el modal al iniciar la aplicación.
```
export default class Gallery extends Component{
    constructor(props) {
      super(props);

      this.state = {
        isOpen: false,
        url: ''
      }

  }
```
> Respecto a los método constructor y super con props, en el artículo original se renderiza este componente pasándole una prop: Gallery imgUrls={imgUrls}.
Como en mi aplicación este componente lo paso sin props en el compomente App, se podrían borrar los métodos constructor y super y cambiar también el método state para quitarle el this. Quedaría así:

```
export default class Gallery extends Component{

      state = {
        isOpen: false,
        url: ''
      }
```




En la parte que devuelve el componente encontramos la parte principal de esta aplicación. Para empezar, tenemos un _div_ con dos clases de CSS, una de Bootstrap y otra de nuestro archivo CSS. Este _div_ sirve para establecer la estructura del container donde se mostrarán las imágenes.
```
<div  className='container-fluid gallery-container'>
```

Seguidamente, otro _div_ con otra clases de CSS Boostrap para seguir con la estructura, esta vez para las filas. Dentro de este _div_ es dónde se mapea el _array_ con la imágenes.

```
<div className='row'>
           {
             imgUrls.map((url, index) => {
                return (
```

En el nuevo _array_ que devuelve el mapeo se añade un _div_ con una clase Bootstrap para la parte _responsive_ de la aplicación. Dentro de este _div_ hay otro _div_ con otra classe CSS para los estilos de las partes de _cards_ de las imágenes.

Dentro de este _div_ se llama al componente de imágenes _GalleryImage_ con los tres _props_ que recibía. El primero, la clase de nuestro CSS _thumbnail_, el _prop_ src asignado dinámicamente a _url_ que es la cada una de las imágenes del mapeo del _array_ inicial; y el _prop_ _alt_ con la información de accesibilidad para cada imagen.

Dentro de la renderización de cada imagen se añade un _span_ dónde se añade el icono para expandir la imagen y abrir el modal con la misma imagen pero más grande, con sus respectivas clases y el evento _onClick_ llamando a la función que se encarga de gestionar la apertura del modal.  

```
<div key={index} className='col-sm-6 col-md-3 col-xl-2'>

    <div className='gallery-card'>

        <GalleryImage  className='gallery-thumbnail' src={url} alt={'Image number ' + (index + 1)} />

              <span className='card-icon-open fa fa-expand-arrows-alt' value={url} onClick={() => this.openModal(url)}></span>

    </div>
</div>
```

Después de esto, se llama al componente que gestiona el modal abierto con tres _props_. En primer lugar, se consulta si está abierto el modal, después se le pasa un evento llamando dinámicamente a la función para cerrar el modal y la última _prop_ es la que establece el estado de la propiedad _url_.
```
  <GalleryModal show={this.state.isOpen} onClick={this.closeModal} src={this.state.url} />
```

Por último, tenemos las dos funciones que se encargan de cambiar el estado del modal, o bien mostrándolo o bien escondiéndolo.
```
openModal = (url) => {

       this.setState({
         isOpen: true,
         url: url
       });
     }

closeModal = () => {

       this.setState({
        isOpen: false,
        url: ''
      });
    }
  }
```

> En el artículo original estas dos funciones son funciones al uso ya que el autor las bindea en el constructor. Yo las he cambiado a _arrow_function_ para que no sea necesario bindearlas y así también poder quitar los métodos constructor y super de la clase.

---
<br>

Hasta aquí esta revisión de una galería con React y CSS. Ha sido una muy buena experiencia en la que he aprendido mucho. También he visto las posibilidades que tiene React para generar aplicaciones de este estilo con un código muy compacto y con una gran posibilidad de reutilización de componentes y escalabilidad de la aplicación.

<br>
