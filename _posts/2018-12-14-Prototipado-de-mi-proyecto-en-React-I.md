---
layout: post
title: Prototipado de mi proyecto en React
tags: [html, css, javascript, React]
---
### [Aprende Javascript con MentoringJS - Step 11 ](http://MentoringJS.com)
En esta serie de artículos voy a explicar el prototipado de mi proyecto de aplicación web pero basado en React y la descomposición en Componentes que he decidido hacer  y el por qué de ellos. Cualquier consejo o corrección es bienvenido.

Esta es una primera versión, con muchas horas detrás, pero obviamente no es la definitiva. **Es una aplicación en constante revisión y progresión.**

Como comentaba, este proyecto está basado en mi [anterior _mockup_ de aplicación móvil](https://felipefcor.github.io/2018-10-04-Proyecto-app/). Pero, en este caso se ha adaptado a una aplicación web con React.

Antes de empezar a explicar cómo he ido realizando esta aplicación, quería indicar que esta es una versión estática y sin efectos, que se dejan para una versión posterior.  Además, también quería indicar algunos recursos que me han ayudado en este proceso.

+ El [artículo de David Tang](https://medium.com/dailyjs/techniques-for-decomposing-react-components-e8a1081ef5da) sobre descomposición de Componentes
+ El [workshop de Leonardo García sobre React](https://github.com/leoasis/workshop-pensando-en-react)
+ Y sobre todo, de nuevo, [el tutorial de Devin Abbott React Express](http://www.react.express/)
+ Añadir también otros muchos recursos que he podido ir utilizando para hacer algunas concretas.

---

Empezando con el código, en primer lugar, se tiene que realizar la configuración del entorno para poder crear una aplicación React. En esto seguí los pasos que ya realicé en mi [último proyecto React sobre la revisión de React express](https://felipefcor.github.io/2018-10-26-Tutorial-React-express(I)).

Para crear e inicializar una app React y poder empezar a programar hacemos:
```
create-react-app kine-proyect
cd kine-proyect
npm start
```
Después, también he añadido varios paquetes que me han ayudado con los estilos y con los iconos. El primero ha sido [Bootstrap](http://getbootstrap.com/docs/4.1/getting-started/introduction/).
```
npm install bootstrap
```
El segundo ha sido [Font Awesome](https://fontawesome.com/?from=io). Este paquete tiene una instalación más específica. De todos modos, en su [propia web explican cómo hacerlo de manera sencilla](https://fontawesome.com/how-to-use/on-the-web/using-with/react).
```
npm i --save @fortawesome/fontawesome-svg-core \
npm i --save @fortawesome/free-solid-svg-icons \
npm i --save @fortawesome/react-fontawesome
```

Una vez se configura el entorno empezamos con la configuración de los archivos básicos, como el index, etc.

**Index.html**

El index es casi el mismo que trae React por defecto, tan solo cambiando el título.
```
<title>Kine Proyect</title>
 </head>
 <body>
   <noscript>
     You need to enable JavaScript to run this app.
   </noscript>
   <div id="app"></div>
  </body>
</html>
```


**Index.js**

```
import { render } from "react-dom";
import React from "react";
import App from './App';
import 'bootstrap/dist/css/bootstrap.css';
import { library } from '@fortawesome/fontawesome-svg-core'
import { faCog } from '@fortawesome/free-solid-svg-icons'
import { faSearch } from '@fortawesome/free-solid-svg-icons'
import { faForward} from '@fortawesome/free-solid-svg-icons'
import { faBackward} from '@fortawesome/free-solid-svg-icons'
import { faTimes} from '@fortawesome/free-solid-svg-icons'

library.add(faCog)
library.add(faSearch)
library.add(faForward)
library.add(faBackward)
library.add(faTimes)

render (<App />, document.querySelector("#app"));
```

En este index se añaden el _import_ para renderizar y el de _React_ y el componente principal _App_.

Además, se importan el archivo de CSS que trae Bootstrap y todos los iconos que se van a utilizar. Debajo de los _imports_ se añaden las librerías de cada icono. Todo esto se explica cómo hacerlo en la [propia documentación de Font Awesome](https://fontawesome.com/how-to-use/on-the-web/using-with/react).

Como se puede ver, los iconos son diferentes al del tutorial, ya que los he intentado adaptar a mi propia aplicación.

Por último, se renderiza el componente _App_ en el atributo _app_ del _div_ del index.


**App.js**

```
import React, { Component } from 'react';
import Index from './components/home';
import Services from './components/Services';
import AboutUs from './components/AboutUs';
import Appointment from './components/Appointment';
import Blog from './components/Blog';
import Gallery from './components/Gallery';


  export default class App extends Component {
    render() {
        return (
          <div>
             <Index />
             <Services />
             <AboutUs />
             <Appointment />
             <Blog />
             <Gallery />           
         </div>

      )
    }
  }

```

Este es el componente principal, desde dónde se hacen todos los _imports_ de los demás componentes.

Antes de seguir, voy a hacer una pequeña aclaración.

> Como se ve en este componente, hay 6 componentes diferentes que se exportan. A continuación iré explicándolos pero quiero hacer un inciso para explicar la estructura que he pensado hacer y que finalmente he hecho.

>Cada componente representa una "página". Es decir, el componente "Index" sería la página principal desde dónde se podría ir accediendo a las demás secciones, que serían la de los servicios ofrecidos; la que contiene información sobre los trabajadores; en la que podrías reservar cita; en la que podrías acceder al blog y la última a la galería de imágenes.

> Seguramente existe otra manera de realizar un proyecto React, como por ejemplo hacerlo todo en una página con la información mostrada en vertical. Pero basándome en el proyecto inicial y en la información que quería mostrar, finalmente he decidido realizarlo así.

> Huelga decir que el resultado no es el más vistoso posible, ya que aparece todo seguido y sin una lógica, pero eso intentaré ajustarlo cuando se añadan los efectos.

En la parte principal hay una clase _App_ que se exporta al componente _App_ anterior y que es la que renderiza todos los demás componentes.


**Home.js**

Como este componente contiene bastante información iré poniendo el código en pequeñas partes. Para empezar, los _imports_.
```
import React, { Component } from 'react';
import NavBar from './NavBar';
import Styles from './Styles'

```
En este componente se importa el componente React y otros personalizados. El primero es _Navbar_ que es un componente que renderiza la barra de navegación en todos los componentes. Más adelante, al revisar el propio componente, lo explicaré en detalle.

El segundo, es otro componente personalizado, pero este es sobre los estilos que he ido utilizando en todos los demás componentes.
```
export default  class Index extends Component {
    render() {
        const list = [
          { 'id': 1, 'name': 'Servicios y especialidades', 'url': './Services'},
          { 'id': 2, 'name': 'Nosotros', 'url': './AboutUs' },
          { 'id': 3, 'name': 'Pedir cita', 'url': './Appointment' },
          { 'id': 4, 'name': 'Blog', 'url': './Blog' },
          { 'id': 5, 'name': 'Galería', 'url': './Gallery' },
        ];
        let imgIndex= "https://images.unsplash.com/photo-1514672013381-c6d0df1c8b18?ixlib=rb-0.3.5&s=2d6c4262761e367e15cc4c0675591fad&auto=format&fit=crop&w=1500&q=80"

        return (     
            <div>
                <NavBar icontwice= "search" title= "Centro de Fisioterapia" icon="cog" />

                <div className="card">

                  <Styles
                  imageIndex ="true"
                  src={imgIndex}
                  >                              

                  </Styles>

                  <ul className="list-unstyled list-group ">
                    {list.map(option =>
                    <li
                      key={option}                                       
                      className="list-group-item list-group-item-primary"
                    >

                    <Styles title="true">
                        <h2>
                          <a href={option.url}>{option.name}</a>
                        </h2>
                    </Styles>


                    </li>) }
                  </ul>  

              </div>

                <Search  />
                <Settings  />
                <Language />                 

            </div>
           );
      }
}
```
Este es la clase principal y la que se exportará al componente _App_.
En la parte del _render_ se declaran un array de objetos y una variable. En el array se añaden todas las secciones de la aplicación, con su ID y con el acceso a su _path_ dentro de la aplicación. En la variable se añade la imagen de la página principal.

En la parte que devuelve la clase se añade el componente _Navbar_ con varios _props_
```
<NavBar icontwice= "search" title= "Centro de Fisioterapia" icon="cog" />
```
Este componente siempre se llama con varios _props_. Aquí hay tres, _icontwice_, _title_ e _icon_. El primero es para comprobar (con un condicional) si la página tiene uno o dos iconos (este tiene dos). El segundo es el título y el tercero es el tipo de ícono a mostrar.

Después se añade un _div_ con una clase de Bootstrap.
```
  <div className="card">
```
> Como en React las clases CSS se llaman con className y no con class, hay que adaptar el código de Bootstrap para cada ocasión.
En todos los componentes se utilizan clases bootstrap para adaptarlo a los estilos que vienen en esa librería.

A continuación, añado otro componente personalizado, _Styles_.
```
<Styles
imageIndex ="true"
src={imgIndex}
>                              

</Styles>
```
Este componente también acepta _props_. El primero, _imageIndex_ es un condicional que comprueba si lo que hay que renderizar es una imagen, un h3, etc. El segundo es el que renderiza la imagen pasada como variable anteriormente.

A continuación se hace un mapeo del array _list_ con el objeto _map_ y extrayendo otro array _option_. Se hace además con una _arrow_function_.

> En todos los componentes se utiliza esta misma lógica de mapeo sacando un listado y con una arrow function.

Tanto en el tag _ul_ como en el _li_ se pasan varias clases de Bootstrap.
Además, dentro del mapeo, en el tag _li_ se llama al componente _Styles_ para coger ciertos a mostrar en el _h2_.
```
<ul className="list-unstyled list-group ">
  {list.map(option =>
  <li
    key={option}                                       
    className="list-group-item list-group-item-primary"
  >

  <Styles title="true">
      <h2>
        <a href={option.url}>{option.name}</a>
      </h2>
  </Styles>


  </li>) }
</ul>  
```

Por último, se llama a los otros componentes que mostrarían el apartado de búsqueda (al cuál se accede desde el icono de la parte superior izquierda), al de ajustes (se accedería desde el icono de la parte superior derecha) y al de los idiomas, que se accedería desde dentro de Ajustes.
```
<Search  />
<Settings  />
<Language />                 

```

El resultado quedaría así:

![Home](https://raw.githubusercontent.com/felipefcor/Proyecto-app/master/img/img-react/home.png "Home")


Dentro de este archivo Home hay tres funciones más, una para la opción de búsqueda, la otra para los ajustes y la última para los idiomas.

_Función búsqueda_
```
function Search (){

       return(

         <div>

           <NavBar title= "Buscar" icon="times" right= "true" />


           <div className="card">
             <form>
                 <div className="form-group">
                   <input className="form-control " type="text" placeholder="Búsquedas más comunes" readOnly></input>
                 </div>
                 <div className="form-group">
                   <button type="button" className="btn btn-info btn-lg ">Contractura</button>
                   <button type="button" className="btn btn-info btn-lg ml-5 ">Animales</button>
                   <button type="button" className="btn btn-info btn-lg ml-5 ">Cita</button>
                   <button type="button" className="btn btn-info btn-lg ml-5 ">Ganchos</button>
                 </div>  
                 <div className="form-group">
                   <label htmlFor="exampleFormControlTextarea1">Busca lo que quieras</label>
                   <textarea className="form-control" id="exampleFormControlTextarea1"></textarea>
                 </div>

                 <div className="form-group">
                   <button className="btn btn-outline-success btn-block my-2 my-sm-0" type="submit">Buscar</button>
                 </div>

               </form>
           </div>      
         </div>

       )
     }

```

Estas función no tiene parámetros y devuelve código JSX. En esta, se pasa inicialmente el componente _Navbar_ para la barra de navegación con un icono y que se muestre a la derecha.

Después, hay un _div_ con la clase _card_ (la mayoría son así) en el que hay un formulario dentro con varias opciones: un input, varios botones, un text Area y un botón final de buscar.

El resultado quedaría así:

![Buscar](https://raw.githubusercontent.com/felipefcor/Proyecto-app/master/img/img-react/search.png "Buscar")


_Función ajustes_
```
function Settings () {

        let settings =  [
          'Idioma',
          'Sonido',
          'Cerrar sesión',

        ]

        function buttonsSettings (button) {

          if (button===settings[0]) {

            return ( <button
                className="btn btn-info ml-5" >Cambiar
              </button>
            );
          }
          else if (button===settings[1]){
            return (
              <button  className="btn btn-primary ml-5" data-toggle="button" aria-pressed="false" autoComplete="off">
              OFF
              </button>)
          }
          else if (button===settings[2]){
            return (
              <button className="btn btn-primary ml-5" data-toggle="button" aria-pressed="false" autoComplete="off">
              ON
              </button>)
          }
        }

        return(

            <div>

                <NavBar title= "Ajustes" icon="times" right= "true" />

                <div className="card">
                  <ul className="list-group list-group-flush">
                    {settings.map(setting =>
                      <li
                      key={setting}
                      className="list-group-item"

                      >
                        { setting }            
                        {buttonsSettings(setting)}
                      </li>) }

                  </ul>
                  <button className="btn btn-outline-success btn-block my-2 my-sm-0" type="submit">Guardar</button>

                </div>



            </div>


        )

      }
```

La función principal es una función simple sin parámetros. Dentro de ella se declara un array con las secciones que tendrá la sección ajustes. Dentro hay otra función con un parámetro que se le pasará como argumento en el _return_ final con el código JSX para discernir qué mostrar.

En esta función interna se hacen varios condicionales que, en función de lo que se reciba, devuelve cierto contenido. Esto se hace sobre todo para mostrar un botón específico en el mapeo. Es decir, al realizar el mapeo del array se mostrará un botón concreto al lado en función del parámetro que esté mapeando el objeto _map_ del array inicial.


El resultado quedaría así:

![Ajustes](https://raw.githubusercontent.com/felipefcor/Proyecto-app/master/img/img-react/settings.png "Ajustes")


_Función idiomas_
```
function Language (){

      let languages =  [
        'Español',
        'Català',
        'Galego',
        'Euskera',
        'English',
        'Française'

      ]

      return(

        <div>

            <NavBar left="true" title= "Idiomas" icon="backward"  />

            <div className="card">
                <ul className="list-group list-group-flush">
                  {languages.map(language =>
                    <li
                    key={language}
                    className= "list-group-item"
                    >

                    <Styles title="true">
                         {language}
                    </Styles>
                    </li>) }

                </ul>
                <button className="btn btn-outline-success btn-block my-2 my-sm-0" type="submit">Seleccionar Idioma</button>

              </div>

        </div>
      );
    }

```
Esta es una función simple con un array que se mapeará en el _return_ sacando un listado. Ahí se le ponen ciertos estilos (pasado a través del componnete _Styles_) y un botón final. Además, se añade el componente _Navbar_ al principio de todo con las  _props_ concretas para este componente.

El resultado quedaría así:

![Idiomas](https://raw.githubusercontent.com/felipefcor/Proyecto-app/master/img/img-react/language.png "Idiomas")


**Services.js**

Este componente es el que aglutina toda la parte de los servicios ofrecidos en la web, tanto la parte genérica como un ejemplo concreto que, posiblemente, se podría hacer un componente aislado como modelo para todos los demás servicios.

La clase principal sería la siguiente:

```
import React, { Component } from 'react';
import NavBar from './NavBar';
import Styles from './Styles'



export default class Services extends Component {



    render () {



        let imageUrl = 'https://picsum.photos/100/?random';

        const servicesContent= [
            {
                id: 1,
                title: "Tratamiento con ganchos y agujas",
                content: "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt",
                price: "30€"
            }    
           ,

           {
                id: 2,
                title: "Rehabilitación",
                content: "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt",
                price: "40€"

            },

           {
                id: 3,
                title: "Fisioterapia acuática",
                content: "labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat",
                price: "25"
           },

           {

                id: 4,
                title: "Fisioterapia asistida con animales",
                content: "labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat",
                price: "25"
            }


        ]

        return(

          <div>

                <NavBar left="true" title= "Servicios y especialidades" icon="backward"  />

                {servicesContent.map(service =>
                      <li
                      key={service.id}

                      className="list-group-item"
                      >
                       <Styles title="true">
                            {service.title}
                      </Styles>

                      <Styles
                            imageCenter ="true"
                            src={imageUrl}
                        >
                      </Styles>



                      <h3>{service.content}</h3>
                      <h4>{service.price}</h4>


                    </li>) }


            <ServicesExample />

          </div>

     )
  }
}
```

Al principio se hacen los _imports_ habituales, de React y de los dos componentes personalizados.

Dentro del _render_ se añade una variable de una web que [muestra imágenes aleatorias](https://picsum.photos).

Después se añade un array de objetos, un objeto por cada servicio.

En la parte del _return_ se añade el componente _Navbar_ con sus especificaciones para este componente y, posteriormente, se realiza un mapeo del array con _map_ sacando un listado. Ahí se le añaden varios estilos cogidos del componente _Styles_, uno para el título y otro para la imagen. Después se añaden otro contenido dinámico extraído de los objetos del array para mostrar el contenido del servicio y el precio específico para cada servicio mapeado.

Finalmente, se carga el componente de ejemplo que veremos a continuación.

El resultado quedaría así:

![Servicios](https://raw.githubusercontent.com/felipefcor/Proyecto-app/master/img/img-react/services.png "Servicios")


_Función servicio ejemplo_
```
function ServicesExample(){

   let imageService= 'https://www.efisioterapia.net/sites/default/files/imagen_cursos/curso-de-ganchos.jpg'

    return (

            <div>
                <NavBar left="true" title= "Técnica de ganchos y agujas" icon="backward"  />

                <div className="card">

                    <Styles title="true">
                       Características
                    </Styles>


                    <Styles
                        imageServices ="true"
                        src={imageService}
                     >
                    </Styles>

                    <h3>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. </h3>      
                    <h3>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. </h3>      
                </div>
            </div>
    )

  }
```

Esta función sería más simple ya que tiene una imagen y devuelve cierto contenido JSX. En este, se llamaría a los componentes _Navbar_ y _Styles_ y después se añadiría una explicación más detallada. Esta información se podría coger de algún archivo concreto o de algún array de objetos que serviría para esta función y para la clase principal del Componente.

El resultado quedaría así:

![Serviciosejemplo](https://raw.githubusercontent.com/felipefcor/Proyecto-app/master/img/img-react/serviceexample.png "Serviciosejemplo")



**AboutUs.js**
```
import React, { Component } from 'react';
import NavBar from './NavBar';
import Styles from './Styles'



export default class  AboutUs extends Component {

    render (){

            const workers = [
                {
                    id: 1,
                    name: "Don Juan Snow",
                    content: "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat"

                },
                {
                    id: 2,
                    name: "Doña Laura Casas",
                    content: "Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat",

                },
                {   
                    id: 3,
                    name: "Doña Rosa López",
                    content: "Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo"
                }
        ]


            let imageUrl= 'https://picsum.photos/100/?random'
            let imgLocalization ='https://cdn.vox-cdn.com/thumbor/6fuJjC-vr4m-Yaf4mkmU7ZfdNbY=/0x0:1161x713/1200x800/filters:focal(435x243:619x427)/cdn.vox-cdn.com/uploads/chorus_image/image/58662765/Screen_Shot_2018_02_12_at_9.34.36_AM.0.png'


            return (
                <div>
                    <NavBar left="true" title= "Nosotros" icon="backward"  />

                    <div className="card" >

                          {workers.map(worker =>
                            <li
                            key={worker.id}

                            className="list-group-item"
                            >
                            <Styles title="true">
                            {worker.name}
                            </Styles>

                           <Styles
                            imageCenter ="true"
                            src={imageUrl}
                            >
                            </Styles>
                             <h2>{worker.content}</h2>   

                            </li>) }



                     </div>

                     <div>

                        <Styles title="true">
                            Localización
                        </Styles>

                        <Styles
                            imageIndex ="true"
                            src={imgLocalization}
                            >                              

                        </Styles>


                        </div>

                        <div>
                        <h4>Teléfono: 6000000</h4>
                        <h4>Twitter: @nosotros</h4>
                        </div>

                    </div>
            )
     }
 }
```

Este componente es el que muestra la información de los trabajadores y de la clínica.

El componente tiene los tres _imports_ iniciales comunes a otros componentes.

Dentro de la clase se añade un array de objetos con toda la información de los trabajadores. Además, dos variables más con imágenes de los trabajadores y de la localización.

En la parte del _return_ se añade el componente _Navbar_ presonalizado. También se hace un mapeo del array y se devuelve en un listado con la información de cada trabajador: nombre, foto y explicación detallada. Ahí se le añaden los estilos establecido en _Styles_. Debajo de este mapeo se añade la información de la localización y también información de contacto y de redes sociales.

El resultado quedaría así:

![Nosotros1](https://raw.githubusercontent.com/felipefcor/Proyecto-app/master/img/img-react/us1.png "Nosotros1")
![Nosotros](https://raw.githubusercontent.com/felipefcor/Proyecto-app/master/img/img-react/us2.png "Nosotros2")

---

Hasta aquí la primera parte de este artículo. En breve estará el siguiente y último artículo sobre el prototipado de mi aplicación con React.
