---
layout: post
title: Haciendo un clon de Netflix
tags: [html, css, sass, javascript, ES6, React, promises, fetch]
---
### [Aprende Javascript con MentoringJS - Step 13 ](http://MentoringJS.com)
En este artículo voy a explicar cómo he realizado un clon de Netflix.

Para empezar, comentar que he seguido el [proyecto de Full Stack React](https://www.fullstackreact.com/react-daily-ui/003-landing-page/).

El resultado se puede ver en [Surge desplegado](http://clon-de-netflix-felipefcor.surge.sh/).

<a href="http://clon-de-netflix-felipefcor.surge.sh/">http://clon-de-netflix-felipefcor.surge.sh/</a>

En ese tutorial se explican partes de la aplicación pero no todas. Voy a ir comentando las partes que se explican y qué problemas he ido encontrando y cómo los he solucionado.

Para empezar se hace un pequeño resumen de cómo será la aplicación y de los componentes más importantes que va a tener la misma. Va a haber 3 componentes con estado que serán _App_, _TitleList_ y _ListToggle_.

El componente principal, _App_, contendrá la lógica de la caja de búsqueda de películas y series de Netflix y también recibirá otro componente con ciertos datos, _TitleList_.

El componente _TitleList_, contendrá la conexión con la API de datos que en este caso será la API de [The Movie DB API](https://www.themoviedb.org/) y no la nativa de Netflix. Esta conexión se hará con el método de JavaScript _fetch_ y trabajando con [promesas](https://felipefcor.github.io/2019-01-21-Javascript-moderno-ES6/), una de las nuevas características de ES6.

**Componente App**

Este es el componente principal. Contiene la caja de búsqueda que muestra el resultado cuándo el usuario introduce una búsqueda. Este _input_ se va a utilizar como [_input controlado_](https://www.fullstackreact.com/react-daily-ui/003-landing-page/#controlled-vs-uncontrolled-inputs), es decir, un _input_ con el atributo _value_. Si no se añadiese ese atributo, React no podría gestionar el estado de los cambios que se introdujesen en la caja ya que el resultado se almacenaría en el DOM.

Además del _value_ se añaden otros atributos que van a gestionar los eventos, que serán _onChange_ y _oneKeyup_.

El primero será el encargado de cambiar el estado. Para cambiar el estado de lo que se introduce en la caja primero se debe añadir un estado inicial, en este caso _searchTerm_ y desde la función que llama el evento _onChange_, modificarlo con la opción _this.setState_.

El segundo evento, _oneKeyup_ será el encargado de gestionar cuando el usuario apriete una tecla del teclado en la caja de búsqueda. En ese caso, se llamará a la función _handleKeyUp_ dónde se comprobará si esa tecla es la tecla "Enter". Si es así, se actualizará el estado de otra propiedad _searchUrl_ que es la encargada de generar la _url_ de búsqueda.

```
class App extends Component {

  state = {

      searchTerm:'',
      searchUrl:''
  }



   handleChange = (event) => {
    this.setState({searchTerm: event.target.value})


  }

    handleKeyUp = (event) => {
    const apiKey = '03ecceac5993bcd054fbc7d617df741a';

   if (event.key === 'Enter' && this.state.searchTerm !== '') {
    let searchUrl = "search/multi?query=" + this.state.searchTerm + "&api_key=" + apiKey;
    this.setState({searchUrl:searchUrl});
  }
  }  

  return (
      <div className="App">


          <input  type="search"  placeholder="Search for a title..." value={searchTerm} onChange={this.handleChange} onKeyUp={this.handleKeyUp} />
    <div>
    )
```

Al realizar este código me encontré con ciertos problemas con el atributo _value_ ya que la función que llama el evento _onChange_ no se explica y en todos los ejemplos de _inputs_ de React se utiliza el _value_ como atributo y como propiedad. Yo me basé en los de [React Express](http://www.react.express/input_handling) que me parecen muy claros. Como aquí la propiedad a cambiar era _searchTerm_ me costó un poco entender que debía poner en el _event.target_ el _value_ y no _searchTerm_. Finalmente, al cambiar de _e_ a _event_ caí en la cuenta de que el segundo _value_ era el gestionado por el evento.

El segundo problema fue en al función _handleKeyUp_ y concretamente en cómo introducir la API key. Para ello tuve que crear una constante y modificar el _this.apiKey_ del original. Además, tuve que darme de alta en The Movie DB. Primero, hay que leer la [documentación de la API](https://www.themoviedb.org/documentation/api). Una vez te das de alta, para poder sacar tu _Api key_ se debe seguir los pasos que se indican en el [_Getting Started_](https://developers.themoviedb.org/3/getting-started/introduction). Con esto logras tu llave que ya puedes utilizar en tu aplicación.

Con todo esto ya tenemos una caja de búsqueda que cambia el estado y también la creación de una _url_ con la _query_ y _Api key_ correctas para hacer la petición a la API de The Movie DB.



**Componente _TitleList_**


En este componente se muestran series y películas de unas 5 secciones. Para conseguir la información se usa la API de The movie DB y se hace utilizando el[ método de JavaScript _fetch_](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch). Este método sustituye a la API _XMLHttpRequest_ y es más limpio y evita el _callback hell_ utilizando promesas.

En este momento, el tutorial introduce un poco al concepto de promesas sobre las peticiones AJAX asíncronas. La explicación que hacen se puede ver [aquí](https://www.fullstackreact.com/react-daily-ui/003-landing-page/#what-is-a-promise).

Básicamente, las promesas tienen tres estados: pendiente, logrado y rechazado.
Otra cosa importante es que las promesas tienen dos métodos importantes: _then_ y _catch_. El método _then_ se llama cuando la operación ha sido satisfactoria, mientras que el método _catch_ es el que se llama para controlar los errores. La sintaxis sería la siguiente:
```
api().then(function(result){
    return api2();    
}).then(function(result2){
    return api3();
}).catch((err)=>{
  return something    
});
```
En este caso, la _api_ sería _fetch_.

El método _fetch_ toma dos parámetros, la URL que se está llamando (o el objeto al que se llama) y las opciones del objeto y después devuelve una promesa como objeto.

Para crear esta llamada con el objeto _fetch_ me he encontrado algunos problemas. Primero, el código del tutorial no está completo. Segundo, en el tutorial la estructura es diferente ya que no se especifica cómo hacerlo. Al no hacerlo, yo he creado un componente nuevo independiente del principal. En este componente he ido poniendo las piezas que el tutorial indicaba pero ajustándolo a mi estructura. En el caso del método _fetch_ la estructura ha sido la siguiente:
```
fetch(requestUrl)
        .then((response)=>{ return response.json();})
        .then((responseData)=>{
          this.setState({data : responseData});})
        .catch(err=>{
       // console.log("fetch error" + err);
        });

```

Esta promesa se añade en una función llamada _loadContent_ que será llamada en el método _componentDidMount_. Este [método de ciclo de vida](http://www.react.express/lifecycle_api) se invoca una vez, después de que el renderizado ocurra por primera vez.

La función _loadContent_ quedaría tal como sigue:
```
const apiKey = '03ecceac5993bcd054fbc7d617df741a';
     let requestUrl = 'https://api.themoviedb.org/3/' + this.props.url + '&api_key=' + apiKey;

     fetch(requestUrl)
       .then((response)=>{ return response.json();})
       .then((responseData)=>{
         this.setState({data : responseData});})
       .catch(err=>{
      // console.log("fetch error" + err);
       });
```
A la petición _fetch_ con la promesa se le añade una constante _apiKey_ y la creación de la _url_ de petición con dicha key y también con una _prop_ que se recibirá al llamar al componente _TitleList_. A este componente se le llama desde el componente _App_, tal y como sigue:
```
        <TitleList title="Top TV picks for Jack" url='discover/tv?sort_by=popularity.desc&page=1' />
        <TitleList title="Trending now" url='discover/movie?sort_by=popularity.desc&page=1' />
        <TitleList title="Most watched in Horror" url='genre/27/movies?sort_by=popularity.desc&page=1' />
        <TitleList title="Sci-Fi greats" url='genre/878/movies?sort_by=popularity.desc&page=1' />
        <TitleList title="Comedy magic" url='genre/35/movies?sort_by=popularity.desc&page=1' />
```

Como se ve, se llama al componente _TitleList_ con una _prop_ de título con el contenido y también una _prop_ url con una parte de la url de petición que se hará a la API de The Movie DB. La otra parte se le añadirá en la función _loadContent_.

En el método _componentDidMount_ se llamaría a esta función _loadContent_:
```
componentDidMount () {
      if(this.props.url !== ''){
        this.loadContent();
        this.setState({
            mounted:true,
            data: this.state.data
          });
      }    
```

En este método se comprueba que la _url_ pasada como _prop_ no esté vacía y, en ese caso, llamar a la función _loadContent_ y acto seguido actualizar el estado de las dos propiedades.

En este componente hay otro método de ciclo de vida, _componentWillReceiveProps_. Este método se utiliza para actualizar los cambios que se reciban desde el componente _App_. En este método se utiliza la segunda propiedad de estado del componente _App_, _searchUrl_. El método quedaría así:
```
componentWillReceiveProps(nextProps){
      if(nextProps.url !== this.props.url && nextProps.url !== ''){
        this.setState({
          mounted:true,
          url:nextProps.url
        },()=>{
          this.loadContent();
        });

      }
    }
```
Este método recibe una _prop_. La lógica que tiene es que compara si se ha actualizado la _url_ a buscar (se ha cambiado la búsqueda por otra) o si está vacía. Si se cumplen esos dos casos cambian las propiedades del estado de este componente y, acto seguido, lanza de nuevo la función _loadContent_ para mostrar el contenido buscado.



**Componente _ListToggle_**

Este componente es un container de icono dentro de otro componente, _Item_. Ese componente es un componente hijo del componente _TitleList_. A pesar de ser un componente simple y muy específico, contiene estado y gestiona lógica. La lógica del componente _ListToggle_ es dual y sirve para indicar si nos gusta o no una serie o película. Tanto para marcarlo como desmarcarlo.

Para poder cambiar en los dos diferentes estilos del icono, se crea un estado en este componente con una propiedad _toogled_ que tiene un valor booleano.
```
state = {
        toggled:false
     }
```

En el renderizado se crea un _div_ con un atributo de evento _onClick_. También se añade otro atributo _data-toogled_ que consigue el valor de la propiedad del estado, _toogled_. Este atributo HTML5 proviene de _data_ ya que se puede utilizar solo _data_ o el mismo _data_ como prefijo y añadirle cualquier cosa detrás con -. Además, este tipo de atributos se utilizan como sustituto de _id_ o _class_ para poder estilar el contenedor _div_.

```
<div onClick={this.handleClick} data-toggled={this.state.toggled} className="ListToggle" />

```
Para gestionar el evento _onClick_ se llama a la función _handleClick_ donde lo que se hace es cambiar el booleano _toogled_ de _true_ a _false_:
```
handleClick = () => {
        if(this.state.toggled === true) {
            this.setState({ toggled: false });
          } else {
            this.setState({ toggled: true });
          }
      }
```


**La parte CSS con Flexbox**

En esta parte se explica el estilado de la clase _ListToggle_. Dentro de la misma se explica un poco porqué se usan dos tipos de iconos diferentes dependiendo de si el usuario pincha en el mismo o no.

<br>

**Hasta aquí el código que aparece en el tutorial y que he ido revisando y reformulando, transformando algunas partes con sintaxis antigua (ES5) a nueva sintaxis de clases (ES6). Además, también he intentado separar en diferentes componentes el contenido. En el tutorial estaba casi todo el contenido dentro del componente _App_.**

---

**A partir de ahora voy a explicar lo que he ido viendo directamente del código de github de la aplicación y lo que he ido añadiendo y cómo lo he hecho para acabar de pulir la aplicación**
<br>

Después de todo esto, en el propio tutorial tengo ciertas partes de código funcionando, sobre todo el buscador. No me acaba de funcionar el mostrar todas las películas y series de la API aunque sí que está bien enlazada ya que por consola veo los objetos que se reciben.

Además, ya no hay más información en el tutorial para seguir creando el clon por lo que voy al código a coger dicha información.

Para empezar, veo que en el componente _App_ incluyen varias clases. Yo las añado como componentes individuales para separarlo del componente y fichero principal. También se añaden clases de CSS nuevas. Para empezar creo dos nuevos componentes: _Logo_ y _Navigation_:
```

export default class Logo extends Component {

    render() {
        return (
        <div id="logo" className="Logo">
        <svg version="1.1" width="300" height="81.386726" id="svg3262">
            <g transform="translate(-384.28572,-428.81172)" id="layer1">
                <g transform="matrix(2.5445375,0,0,2.5445375,1157.1714,-1457.8678)" id="g3235">
                    <path d="m -203.09972,771.41415 c 1.6425,0.15875 3.2825,0.33 4.92,0.5075 l 3.615,-8.92625 3.43625,9.74875 c 1.76375,0.22125 3.525,0.4525 5.2825,0.695 l -6.02375,-17.09625 6.02625,-14.88 -5.10375,0 -0.0525,0.0725 -3.255,8.03875 -2.8575,-8.11125 -5.03875,0 5.2025,14.7625 -6.15125,15.18875 z" id="path3015" style={{fill:'#b81d24', fillOpacity: 1, fillRule:'nonzero', stroke:'none'}} />
                    <path d="m -206.91147,771.06478 0,-29.60125 -5.0375,0 0,29.18625 c 1.68125,0.12875 3.36125,0.26875 5.0375,0.415" id="path3019" style={{fill:'#b81d24', fillOpacity: 1, fillRule:'nonzero', stroke:'none'}} />
                    <path d="m -244.7486,769.4089 c 1.36,0 2.7175,0.01 4.07375,0.0213 l 0,-10.875 6.05125,0 0,-4.63125 -6.05125,0 0,-7.825 6.96875,0 0,-4.63625 -12.02625,0 0,27.95 c 0.3275,0 0.655,-0.004 0.98375,-0.004" id="path3023" style={{fill:'#b81d24', fillOpacity: 1, fillRule:'nonzero', stroke:'none'}} />
                    <path d="m -260.3881,769.69191 c 1.6775,-0.06 3.3575,-0.11 5.04,-0.15125 l 0,-23.44125 4.7075,0 0,-4.63625 -14.45625,0 0,4.63625 4.70875,0 0,23.5925 z" id="path3035" style={{fill:'#b81d24', fillOpacity: 1, fillRule:'nonzero', stroke:'none'}} />
                    <path d="m -298.91059,772.81378 0,-17.63625 5.96375,16.92375 c 1.83375,-0.20625 3.67125,-0.4 5.5125,-0.5825 l 0,-30.055 -4.8325,0 0,18.2675 -6.43625,-18.2675 -0.2075,0 -4.8325,0 0,31.98375 0.03,0 c 1.5975,-0.22125 3.19875,-0.43125 4.8025,-0.63375" id="path3039" style={{fill:'#b81d24', fillOpacity: 1, fillRule:'nonzero', stroke:'none'}} />
                    <path d="m -269.95297,746.09903 0,-4.63625 -12.0275,0 0,24.9125 0,4.6375 0,0.004 c 3.99125,-0.345 7.99625,-0.63375 12.0175,-0.86875 l 0,-0.004 0,-1.33625 0,-3.3 c -2.325,0.135 -4.645,0.29125 -6.96,0.46375 l 0,-7.415 6.05125,0 0,-4.63375 -6.05125,0 0,-7.82375 6.97,0 z" id="path3051" style={{fill:'#b81d24', fillOpacity: 1, fillRule:'nonzero', stroke:'none'}} />
                    <path d="m -223.72272,765.2864 0,-23.82375 -5.05875,0 0,23.605 0,4.63625 0,0.005 c 4.02375,0.1475 8.0325,0.35375 12.0275,0.6125 l 0,-0.006 0,-1.4975 0,-3.13875 c -2.31875,-0.15 -4.64125,-0.28 -6.96875,-0.3925" id="path3055" style={{fill:'#b81d24', fillOpacity: 1, fillRule:'nonzero', stroke:'none'}} />
                </g>
            </g>
        </svg>
        </div>  );
    }
}
```
Este componente es puramente representacional y devuelve una imagen (logo) en formato _svg_.

```
export default class Navigation extends Component {

    render() {
        return (
            <div id="navigation" className="Navigation">
             <nav>
                 <ul>
                    <li>Browse</li>
                    <li>My list</li>
                    <li>Top picks</li>
                    <li>Recent</li>
                 </ul>
            </nav>
          </div>
         );
    }
}
```
El componente _Navigation_, como su nombre indica, sirve para crear el menú de navegación de la web devolviendo un listado.

Respecto al CSS, el archivo _App.css_ del código de github de la web es mucho más extenso que el que hay en el tutorial. Lo copio en un archivo scss y lo proceso a _App.css_. Haciendo esto, tanto el logo, el menú de navegación como el _input_ de búsqueda se transforman por completo y adquieren una vista muy parecida a la imagen del tutorial. Además, aparecen efectos en todos estos apartados. En el logo aparece el efecto de blanquearse cuando se pasa por encima. En los apartado del menú, cuando se pasa el ratón por encima se ve un bloque rojo. El buscador se amplía al poner el ratón encima para buscar algo y el borde se pone rojo. También aparece una aspa para borrar el contenido que se ha puesto.

El siguiente componente es el del perfil de usuario, _UserProfile_ donde aparece el nombre y el logo:
```
export default class UserProfile extends Component {

    render() {
        return (
        <div className="UserProfile">
            <div className="User">
                <div className="name">Felipe</div>
                <div className="image"><img src="https://felipefcor.github.io/img/felipeavatar.jpg" alt="profile" /></div>
            </div>
        </div> );
    }
}
```
Este es otro componente puramente representacional que devuelve varios _div_ con el nombre del usuario y un enlace a la imagen de su logo.

Posteriormente he añadido la parte de la imagen del _background_ y la información de la serie Narcos con los botones de ver y añadir a la lista. Esto se hace en el componente _Hero_, que tiene dos clases, una para la parte visual y el otro para renderizar los botones:
```

export default class Hero extends Component {


    render() {
        return (
            <div id="hero" className="Hero" style={{backgroundImage: 'url(https://images.alphacoders.com/633/633643.jpg)'}}>
                 <div className="content">
                     <img className="logo" src="http://www.returndates.com/backgrounds/narcos.logo.png" alt="narcos background" />
                     <h2>Season 2 now available</h2>
                     <p>Plata o plomo </p>
                     <div className="button-wrapper">
                        <HeroButton primary={true} text="Watch now" />
                        <HeroButton primary={false} text="+ My list" />
                    </div>
                </div>
                      <div className="overlay"></div>
            </div>
         );
    }
}


class HeroButton extends Component {

    render() {
        return (
            <a href="#url" className="Button" data-primary={this.props.primary}>{this.props.text}</a>
          );
    }
}

```

En la clase _Hero_ se renderiza una imagen de _background_, un logo, y un poco de texto. Además, se le añaden dos botones a los que se le pasa texto como _prop_. Estos botones están en la clase _HeroButton_ que solo renderiza un elemento _a_ con una clase y dos _props_. Por cuestiones de legibilidad y coherencia esta última clase la he añadido en el mismo componente principal, aunque seguramente se podría haber creado uno nuevo.



**_TitleList_**

En este componente es donde hay que añadir bastante más contenido y que el tutorial no toca. Para verlo hay que ir al código. Además, es una de las partes más importantes porque añade la lógica de cómo se muestran las películas y series que se recogen de la API. El código añadido es el de la parte del renderizado, que ahora revisaré. También cambian pequeños detalles, como por ejemplo que la propiedad _data_ del estado inicial se asigna a un _array_ vacío.
```
var titles ='';
   if(this.state.data.results) {
     titles = this.state.data.results.map(function(title, i) {
       if(i < 5) {
         var name = '';
         var backDrop = 'http://image.tmdb.org/t/p/original' + title.backdrop_path;
         if(!title.name) {
           name = title.original_title;
         } else {
           name = title.name;
         }

         return (
           <Item key={title.id} title={name} score={title.vote_average} overview={title.overview} backdrop={backDrop} />
         );  

       }else{
         return (<div key={title.id}></div>);
       }
     });

   }

   return (
     <div ref="titlecategory" className="TitleList" data-loaded={this.state.mounted}>
       <div className="Title">
         <h1>{this.props.title}</h1>
         <div className="titles-wrapper">
           {titles}
         </div>
       </div>
     </div>
);
```

Esta parte del _return_ tiene algunas cosas en ES5, tales como la función mapeada que se guarda en la variable _titles_ y también la declaración de variables con _var_. Es por ello que lo he cambiado por una _arrow_function_ y utilizando la palabra clave _let_:
```

     let titles ='';

          if(this.state.data.results) {
            titles = this.state.data.results.map((title, i) => {
              if(i < 5) {
                let name = '';
                let backDrop = 'http://image.tmdb.org/t/p/original' + title.backdrop_path;
                if(!title.name) {
                  name = title.original_title;
                } else {
                  name = title.name;
            }

        return (
          <Item key={title.id} title={name} score={title.vote_average} overview={title.overview} backdrop={backDrop} />
          );  

        }  else {
          return (<div key={title.id}></div>);
        }

      });

    }

```

Respecto al código en sí, a continuación explico lo que hace.
En primer lugar se declara una variable vacía _titles_ que se utilizará posteriormente. En segundo lugar, se crea un condicional en el que si hay resultados en la comunicación con la API se sacarán ciertos datos. En primer lugar, se hace un mapeo de dichos resultados y se le pasan dos parámetros, el título e _i_. Este último sirve para hacer otro condicional y solo mostrar hasta 5 películas o series por categoría. Si hay menos de 5 se declaran varias variables más donde se almacena el nombre y la ruta a la imagen de _background_. Si no hay título en los datos recibidos de la API se asigna manualmente el título directamente de una propiedad del objeto recibido (_title_original_title_).
Si todo esto va bien, el condicional devuelve el componente _Item_ (que se explica un poco más abajo) con varios datos: el id, nombre, puntuación, resumen y ruta a una imagen de _background_ de cada película o serie. Si el primer condicional no funciona (el de recibir resultados) se devuelve tan solo el id.

Posteriormente, está el _return_ del componente _TitleList_. Este devuelve varias clases de CSS, un _H1_ con el título de la serie o película y contenido dinámico de todo lo que se ha almacenado en el condicional previo, es decir, lo que devuelve la variable _titles_.

Como he comentado anteriormente, falta otro pequeño componente, _Item_, que es importante ya que recibe los datos de otro componente _ListToggle_.
```
class Item extends Component {

  render() {
    return (
      <div className="Item" style={{backgroundImage: 'url(' + this.props.backdrop + ')'}} >
        <div className="overlay">
          <div className="title">{this.props.title}</div>
          <div className="rating">{this.props.score} / 10</div>
          <div className="plot">{this.props.overview}</div>
        <ListToggle />
      </div>
    </div>
     );
  }
}
```
Este componente devuelve en primer lugar varios estilos, incluidos unos específicos para la imagen de _background_. En la parte dinámica recibe tres _props_, el título, la puntuación (que divide entre 10) y el resumen.

Al final, recibe el componente _ListToggle_ que, a continuación, indicó qué hace.


**_ListToggle_**

Aquí se le añade código que en el tutorial no estaba. Es código en la parte que devuelve el componente y son dos líneas que añaden dos iconos, uno para que salga el icono del símbolo más y el otro para el de _check_.
```
<div onClick={this.handleClick} data-toggled={this.state.toggled} className="ListToggle">
        <div>
          <i className="fa fa-fw fa-plus"></i>
          <i className="fa fa-fw fa-check"></i>
        </div>
</div>
```


---
