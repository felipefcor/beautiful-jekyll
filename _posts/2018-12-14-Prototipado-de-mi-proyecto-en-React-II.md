---
layout: post
title: Prototipado de mi proyecto en React II
tags: [html, css, javascript, node, npm, React]
---
### [Aprende Javascript con MentoringJS - Step 11 ](http://MentoringJS.com)
Seguimos con la segunda parte del artículo sobre el prototipado de mi proyecto con React. [La primera parte se puede encontrar aquí](https://felipefcor.github.io/2018-12-14-Prototipado-de-mi-proyecto-en-React-I/).

En la primera parte lo dejé con el componente _AboutUs_ en el que se añade toda la información sobre los trabajadores y la clínica. El siguiente archivo es el de las citas.

**Appointment.js**

Este componente tiene dos clases, una que tiene la función de ser una plantilla y la otra que es la principal y la que se exporta desde este archivo.

El archivo comienza con los tres _imports_ habituales:
```
import React, { Component } from 'react';
import NavBar from './NavBar';
import Styles from './Styles'
```

La clase plantilla es la siguiente:
```
class AppointmentTemplate extends Component {

  render() {


      const days =  [
      'Lunes',
      'Martes',
      'Miércoles',
      'Jueves',
      'Viernes'
    ]

    const hoursMorning =  [
      '9am',
      '10am',
      '11am',
      '12am',
      '13pm',
      '16pm',
      '17pm',
      '18pm',
      '19pm'

    ]



    const stylesList = {
      container: {
        textAlign: 'center'
      }
    }


    const {children, week, day} = this.props
    let result;



    if (week) {

      result =  (
                 <div className="card">
                    <ul className="list-unstyled list-group ">

                      <Styles title="true">
                        {children}
                      </Styles>

                      {days.map(day =>
                      <li
                      key={day}
                      style = {stylesList.container}
                      className="list-group-item"
                      >

                      <h2>{day}</h2>


                      <button className="btn btn-lg btn-secondary">
                      <span className="badge badge-light">3</span>
                      </button>
                    </li>) }

                 </ul>

               </div>
         )
      }

      else  if (day) {

          result = (

              <div className="card">

              <ul className="list-unstyled list-group ">

                      <Styles title="true">
                        {children}
                      </Styles>

                      {hoursMorning.map(hour =>
                      <li
                        key={hour}
                        style = {stylesList.container}
                        className="list-group-item"
                      >
                        <h2> { hour } </h2>
                        <button className="btn btn-lg btn-danger">
                        <span className="badge badge-light">Ocupado</span>
                        </button>
                      </li>) }

                </ul>
            </div>
          )

      }


    return (
          <div>
            {result}
          </div>


     );
  }
}

```

En la parte del _render_ se declaran dos arrays con los días y las horas. Además, se declara una constante de estilos para añadirla en el listado posterior (este es el único sitio en el que no se utilizan los estilos del componente _Styles_).

Además, se añaden tres constantes que se utilizarán como props: children, week, day; y también otra variable que será la que se renderice al componente principal de este artículo.

Posteriormente, se hace una estructura condicional en la que, en función de si se quiere renderizar los días o las horas, se pasará un contenido JSX u otro. El contenido de la variable _result_ es un mapeo de los dos arrays (en función del parámetro que se especifique como _prop_ en la clase principal al llamar a esta clase). El contenido es básicamente los días u horas con un botón con cierta información dentro (las horas disponibles y si está ocupado o no).

_Clase principal Appointment_
```

    render(){


      return(
        <div>
          <NavBar left="true" title= "Pedir Cita" icon="backward"  />

          <AppointmentTemplate week= "true">
            Semana actual
          </AppointmentTemplate>

           <AppointmentTemplate week ="true">
            Semana siguiente
          </AppointmentTemplate>

          <AppointmentTemplate day ="true">
            Día
          </AppointmentTemplate>


        </div>
      )
    }
}

```

En esta clase se devuelve básicamente tres veces el componente plantilla anterior modificándolo con la _prop_ que se quiere mostrar, si es la semana o el día y también con el texto que se pasa como _children_.

Además, en la parte superior se añade el componente _Navbar_ con la barra de navegación y el icono específico.


El resultado quedaría así:

![Cita](https://raw.githubusercontent.com/felipefcor/Proyecto-app/master/img/img-react/appointment1.png "Cita")

![Cita](https://raw.githubusercontent.com/felipefcor/Proyecto-app/master/img/img-react/appointmen2.png "Cita")


**Blog.js**

En este componente se muestra la información del blog de la clínica.
```
import React, { Component } from 'react';
import NavBar from './NavBar';
import Styles from './Styles'


export default  class Blog extends Component {

    render() {

         const post = [
            {
                content: "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
                date: "14 abril"
            },
            {
                content: "Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat",
                date: "21 noviembre"
            }

       ]



        return (



            <div>

              <NavBar left="true" title= "Blog" icon="backward"  />


              <div className="card">

                  <Styles title="true">
                     Terapias manuales con ayuda de agujas
                  </Styles>


                   {post.map(posts =>
                    <li
                    key={posts}

                    className="list-group-item"
                    >

                    <h3>{posts.content}</h3>
                    <h4>{posts.date}</h4>


                  </li>
                  ) }


              </div>


            </div>

        )
    }
}
```

Como se puede ver, la estructura es muy similar a otros componentes. Los tres _imports_ iniciales y en el contenido, un array de objetos con todos los posts a renderizar. Esa renderización se hace con un mapeo del array sacando un listado y con ciertos estilos. Además, se pasan dinámicante otras partes de los objetos (contenido y fecha).

El resultado quedaría así:

![Blog](https://raw.githubusercontent.com/felipefcor/Proyecto-app/master/img/img-react/blog.png "Blog")


**Gallery.js**

Este archivo muestra la galería de imágenes.
```
import React, { Component } from 'react';
import NavBar from './NavBar';
import Styles from './Styles'


export default class Gallery extends Component {

    render (){
    let urls = [

        "https://images.unsplash.com/photo-1501869150797-9bbb64f782fd?ixlib=rb-0.3.5&s=a48f318b6b41b2e1c8343f43c66df31d&auto=format&fit=crop&w=1534&q=80",
        "https://images.unsplash.com/photo-1512290923902-8a9f81dc236c?ixlib=rb-0.3.5&s=f3ba719b26240ac066b0a645c9387d87&auto=format&fit=crop&w=1500&q=80",
        "https://images.unsplash.com/photo-1514672013381-c6d0df1c8b18?ixlib=rb-0.3.5&s=2d6c4262761e367e15cc4c0675591fad&auto=format&fit=crop&w=1500&q=80"
        ]


        return (
          <div>
               <NavBar left="true" title= "Imágenes y vídeos" icon="backward"  />

               <div className="card">

                  {urls.map(url =>
                  <li>
                    <Styles
                        imageIndex ="true"
                        src={url}
                    >
                  </Styles>
                  </li>


                  )}

               </div>
          </div>
        )
}
}

```

El contenido es sencillo. Un array de imágenes y un mapeo de ese array para sacar el listado de las imágenes con ciertos estilos cogidos del componente _Styles_. Además, en la parte superior se añade el componente _Navbar_ con la barra de navegación y el icono específico.

El resultado quedaría así:

![Galería](https://raw.githubusercontent.com/felipefcor/Proyecto-app/master/img/img-react/images.png "Galería")

---

Finalmente, hemos llegado a los dos componentes personalizados que se utilizan en todos los otros componentes y que son los que creía que, claramente, eran los mejores candidatos a tener un componente reutilizable.

**Navbar.js**

Este archivo tiene una sola clase. El principio tiene dos _imports_ concretos, ya que se tiene que llamar a los iconos de Font Awesome.

```
import React, { Component } from 'react';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome'
```
El contenido principal es:
```
export default class NavBar extends Component {


    font = icon =>  {

        return (

            <FontAwesomeIcon  icon ={icon} />
            )

    }

    font = icontwice =>  {

        return (

            <FontAwesomeIcon  icon ={icontwice} />
            )

    }



    render() {

        const styles = {
            container: {
                fontSize: 40,
                fontWeight: 'bold',
            }
          }

        const {icon, title, icontwice} = this.props
        const {left, right} = this.props
        let iconside;



        if (left) {

            iconside = (

               <nav className="navbar navbar-light bg-light">

                    <div>
                        {this.font(icon)}
                    </div>

                    <h1
                     style = {styles.container}>
                     {title}
                    </h1>

                    <div></div>
                </nav>

            )
        }

        else if (right) {

            iconside= (

                <nav className="navbar navbar-light bg-light">

                    <div></div>

                    <h1
                     style = {styles.container}>
                     {title}
                    </h1>

                    <div>
                       {this.font(icon)}
                    </div>

                </nav>
            )
        }

        else {

            iconside= (

                <nav className="navbar navbar-light bg-light">

                    <div>
                        {this.font(icontwice)}
                    </div>

                    <h1
                     style = {styles.container}>
                     {title}
                    </h1>

                    <div>
                        {this.font(icon)}
                    </div>

                </nav>
            )

        }

        return (

            <div>
              {iconside}
            </div>
         );
    }
}


```
En este  componente me encontré con el problema de que solo se podía llamar a _Navbar_ con un icono. Si se llamaba con dos diferentes (como en Home) daba error. Para solventarlo tuve que crear dos funciones (en _arrow_function_) _font_ con un parámetro diferente _icon_ e _icontwice_. En función de cual se pasase se devolvía un cierto contenido JSX. Como se puede ver, es el mismo. Pero lo hice así porque si se llamaba dos veces a _icon_ daba error, como he comentado anteriormente.

Dentro de la parte del _render_ se declaran seis constantes y una variable. Las constantes son una de estilos y 5 que se utilizaran como _props_ más adelante. La variable es la del resultado que se pasa en el _return_.

Posteriormente hay una estrucutra condicional que renderiza cierto contenido en función de si en la llamada a este componente se hace con la especificación de que el icono debe ir a la izquierda (left), derecha (right) o a ambos. También hay otro _prop_ con el título que se pasará al llamar a _Navbar_. Lo que devuelven las tres opciones es contenido JSX con varios _divs_. Uno vacío o con el icono, otro con el título y el otro vacío o con el icono.

Finalmente se devuelve el resultado de los condicionales.


**Styles.js**

Este componente muestra los estilos de una manera similar a como lo hace _Navbar_, ya que se utiliza una estructura condicional para mostrar lo que se indica como _prop_ al llamar al componente _Styles_.
```
import React, { Component } from 'react';

export default class Styles extends Component {

    render() {

        const {imageIndex, title, imageServices, imageCenter, src, list, children} = this.props

        let result;




        if (imageIndex){

            result = (

                <img
                 style={styles.imageIndex}
                 src = {src}
                ></img>

                )
            }

        else if (title) {

            result = (

                <h3 style={styles.title}>
                {children}
                </h3>


               )

        }
        else if (imageServices)
                           {

            result = (
                <div>
                <img
                style={styles.imageServices}
                src = {src}
                 >
                </img>
                </div>

               )

        }

        else if (imageCenter)
                           {

            result = (


                <img
                 style={styles.imageCenter}
                 src = {src}
                ></img>


               )

        }

        else if (list) {
            result = (
                <h2
                style= {styles.list}>

                </h2>
            )
        }



        return (


            <div>{result}</div>




         );


    }

}

let styles = {
    imageIndex: {
        display: 'block',
        width: '35%',
        marginLeft: 'auto',
        marginRight: 'auto',
        marginTop: 20,
        marginBottom: 20
    },
    title: {
        textAlign: 'center',
        fontWeight: 'bold'
    },
    imageServices: {
        display: 'block',
        width: '80%',
        marginLeft: 'auto',
        marginRight: 'auto',
        marginTop: 20,
        marginBottom: 20

    },

    imageCenter: {
        display: 'block',
        width: '20%',
        marginLeft: 'auto',
        marginRight: 'auto',
        marginTop: 10,
        marginBottom: 10
        },

    list: {
        textAlign: 'center'
    }
  };

```
Como se puede ver, este componente es muy similar al anterior.

Tiene un _import_ solo, el de React.

Después ya está la parte de renderizar con las constantes que se pasarán como _props_ y la variable que se utilizará para almacenar el resultado y renderizarlo.

Después entra la parte condicional, en la que, en función del estilo que se llame o se pase, el resultado será uno u otro. El contenido que se pasará depende de lo que se quiera mostrar, por ejemplo estilos en una imagen o en un _h3_. En función de eso, se devolverá código JSX que llaman a los estilos declarados en la parte inferior.

Como se puede ver en los condicionales, lo que devuelve cada _if_ es diferente. Este es un punto que ha sido dificultoso ya que alguna vez se tenía que pasar una imagen, otra un _h2_ o _h3_. En la imagen, la parte difícil ha sido la de pasar como _prop_ un atributo del tag _img_, mientras que en el _h3_ he utilizado la opción _children_ para que al llamarlo se renderice lo que se añade como texto.

---

Hasta aquí el prototipado de mi aplicación React. En un futuro, se revisará esta aplicación y se añadirán los efectos necesarios para hacer de la web una página dinámica.

¡Saludos a todos!
