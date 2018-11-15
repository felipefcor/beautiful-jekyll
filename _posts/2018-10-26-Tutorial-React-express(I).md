---
layout: post
title: Aprendiendo React. Siguiendo el tutorial de React.express
tags: [React, node.js, npm, webpack, babel, javascript, html, css]
---
### [Aprende Javascript con MentoringJS - Step 10 ](http://MentoringJS.com)
En este artículo voy a mostrar todos los ejercicios del tutorial de [react.express](http://www.react.express/) y voy a analizar todos los ejemplos explicando qué significa cada parte de código.

> Para realizar este artículo he utilizado varios tutoriales y fuentes con los que empezar a conocer y familiarizarme con React.
- La primera, obviamente, la [documentación oficial de React](https://reactjs.org/).
En esta documentación hay una introducción [teórica](https://reactjs.org/docs/hello-world.html) y otra más [práctica](https://reactjs.org/tutorial/tutorial.html).
- La segunda ha sido un tutorial con contenido más [conceptual](https://github.com/reactjs/react-basic).

<br>
Para empezar, es necesario realizar la instalación (o _setup_) de todas las herramientas necesarias para llevar a cabo los ejercicios en tu propio ordenador. Es cierto que actualmente hay muchas opciones de programar, inclusor React, a través de webs específicas sin necesidad de instalaciones previas. Bajo mi punto de vista, para aprender y sentirse cómodo con las tecnologías utilizadas es mucho mejor realizarlo en _local_.

En primer lugar, hay que instalar [_nodejs_](https://nodejs.org/es/). Como dicen en su web, node.js es:
> Un entorno de ejecución para JavaScript construido con el motor de JavaScript V8 de Chrome.

Con el entorno node.js instalado se podrá utilizar el sistema de paquetería específico [_npm_](https://www.npmjs.com/) para instalar los paquetes necesarios para poder desarrollar en React.

En el tutorial de [react.express](http://www.react.express/) nos dan dos opciones.

1. La más sencilla es utilizar _npm_ para instalar el paquete específico [_create-react-app_](https://reactjs.org/docs/create-a-new-react-app.html). Con esta instalación es muy sencillo establecer el entorno de desarrollo necesario para poder pasar a programar rápidamente.

	Para instalar _create-react-app_ con _npm_ se tendría que realizar los siguientes comandos:
```
npm install -g create-react-app
```
Posteriormente, para crear e inicializar una _app_ React y poder empezar a programar se tendría que escribir:
```
create-react-app my-app
cd my-app/
npm start
```
Con la última orden se abrirá un navegador mostrando el código por defecto del archivo _src/App.js_.

2. La segunda opción es instalar manualmente [webpack, babel y React](http://www.react.express/setup).
<br><br>
Webpack
```
npm install --save-dev webpack webpack-dev-server
```
Babel
```
npm install --save-dev babel-loader babel-core babel-preset-react babel-preset-env babel-preset-stage-1 babel-plugin-transform-runtime
npm install --save babel-runtime
```
React
```
npm install --save react react-dom
```
En los tres pasos hay que hacer cambios en ciertos archivos para que la configuración sea correcta y funcione todo bien.



> Nota: como todos los ejemplos están adaptados a esta segunda opción de instalación he decidido seguir también esta vía. Los ejemplos se podrían haber adaptado a la primera configuración, más sencilla y habitual, pero he querido ser fiel al tutorial.

> Para seguir el tutorial hay que hacer todo lo que indica en el _setup_ e instalar todas las herramientas ya que sino pueden aparecer errores al intentar ejecutar el index.js de React.


<br>

### Índice de ejercicios<a name="idindex"></a>

1. [ReactDOM](#id1)
2. [DOM Components](#id2)
3. [Custom Components](#id3)
4. [Component API](#id4)
5. [Inline Styles](#id5)
6. [CSS-in-JS](#id6)


<br>


**1. ReactDOM**<a name="id1"></a>

```
import React from 'react'
import { render } from 'react-dom'

const node = document.querySelector('#app')
const element = <div>Hello World!</div>

render(element, node)
```

Lo primero que aparece es las dos primeras líneas son _imports_.
Esto se hace para importar las dos librerías básicas que nos servirán para trabajar con React:

```
import React from 'react'
```
Este _import_ se realiza para importar la librería necesaria para **crear componentes**.

```
import { render } from 'react-dom'
```
Este _import_ se realiza para importar la librería necesaria para **_renderizar_ o representar el contenido en el DOM**.



Después se declaran dos variables con const ([en ES6](http://www.react.express/block_scoped_declarations)).
La primera, _node_ se hace para seleccionar un _nodo_ del DOM en nuestra _app_ para renderizar el contenido. Esto se hace seleccionando el atributo _app_ que está añadido en el index.html

La segunda variable, _element_, se añade contenido en formato [JSX](http://www.react.express/jsx).

> [JSX](https://reactjs.org/docs/introducing-jsx.html) es un tipo de expresión de Javascript, basada en tags como XML. La lógica interna que ha llevado a utilizar este tipo de expresión en React es debido a que no se quiere separar los diferentes tipos de expresión que se pueden generar en la UI (Interficies de usuario) ya que React separa _problemas_ y no tecnologías.
JSX es un atajo  para usar la API de React.createElement() y la que se encarga de transformar o [compilar las expresiones JSX en Javascript estándar es **Babel**](https://babeljs.io/repl/#?babili=false&browsers=&build=&builtIns=false&spec=false&loose=false&code_lz=MYewdgzgLgBGIBMCmMC8MEmAVwLZLCgDoBHbJAJwE8BlJAGyWChAoAoByAYgEMAHPhwCUAKFCRYDJPkJoYAHgQBLAG4A-ABIN6IGAHVW9BAEJ5AemXqRIigWTspMqABo4iJEKA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=react&prettier=false&targets=&version=6.26.0&envVersion=).


La última línea sirve para _renderizar_ la variable _element_ en el nodo DOM.
```
render(element, node)
```

[Volver al Índice](#idindex)

<br>
**2. DOM Components** <a name="id2"></a>

```
import React from 'react'
import { render } from 'react-dom'

const node = document.querySelector('#app')
const element = (
	<div>
		<input type={'text'} defaultValue={'Type here!'} />
		<select>
			<option>A</option>
			<option>B</option>
		</select>
		<img src={'https://images.unsplash.com/photo-1539980307411-6820f89db71b?ixlib=rb-0.3.5&ixid=eyJhcHBfaWQiOjEyMDd9&s=a613de46106ddfc2cddbd4aaf6e4b437&auto=format&fit=crop&w=987&q=80'}

	</div>
)

render(element, node)
```

Este ejemplo solo cambia del anterior en el contenido de la variable _element_.
En el _div_ se añaden diversas etiquetas HTML como _input_, _select_ o _img_ para añadir contenido, bien sea para añadir texto, seleccionar varias opciones o añadir una imagen.

<br>
**3. Custom Components** <a name="id3"></a>

```
import React, { Component } from 'react'
import { render } from 'react-dom'

class Card extends Component {
  render() {
    const style = {
      margin: 20,
      padding: 20,
      color: 'white',
      backgroundColor: this.props.color,
    }

    return (
      <div style={style}>
        {this.props.children}
      </div>
    )
  }
}

const node = document.querySelector('#app')
const element = (
  <div>
    <Card color={'skyblue'}>Card 1</Card>
    <Card color={'steelblue'}>Card 2</Card>
  </div>
)

render(element, node)
```

En este ejemplo hay varios cambios con el ejemplo anterior.

El primero es en el primer _import_. Ahí se ve que se ha añadido _{component}_ a la hora de importar. Esto quiere decir que se va a importar un **Componente** de la librería React.

Acto seguido se añade el Componente en forma de clase (_class_).

Según la [documentación de React](https://reactjs.org/docs/components-and-props.html), los componentes te permiten:
>Dividir la Interficie de Usuario en piezas independientes y reutilizables y pensar en cada pieza por ella misma.
> Los componentes conceptualmente son como funciones de Javascript. Aceptan que se le pasen parámetros (que se llaman _props_) y devuelven elementos React describiendo lo que debería aparecer en pantalla.

Hay dos maneras de expresar un Componente, [mediante una función y mediante una clase](https://reactjs.org/docs/components-and-props.html).
En el ejemplo de arriba se utiliza una clase. [Estas clases utilizan sintaxis ES6](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes).


Respecto al ejercicio, decir que el componente se crea con una clase llamada _Card_.
```
class Card extends Component {}
```
Esta sintaxis quiere decir que la clase creada se extiende a [React.Component](https://reactjs.org/docs/react-component.html).

Después se añade un método llamado _render()_ donde va todo el contenido del componente. Ahí se añaden estilos CSS y también se declara el color de fondo como receptor de una _props_ que le vendrá de fuera y que añadirá dicho color.
>Hay que puntualizar que, en las clases con los valores que se reciben de fuera no se utiliza la sintaxis de _props_ sino _this.props_.

La clase devuelve los estilos y también el color de fondo que deberán recibir como _this.props_.

El valor que recibirá el color de fondo de la clase se le pasa en la variable _element_. Ahí, se llama a la clase _Card_ dos veces, cada una con un color de fondo diferente. Y esto es lo que se renderiza al final.

[Volver al Índice](#idindex)

<br>
**4. Component API** <a name="id4"></a>

```
import React, { Component } from 'react'
import { render } from 'react-dom'

class Counter extends Component {

  state = {count: 0}

  componentDidMount() {
    setInterval(() => {
      this.setState({count: this.state.count + 1})
    }, 1000)
  }

  render() {
    const {count} = this.state
    const {color, size} = this.props

    return (
      <div style={{color, fontSize: size}}>
        {count}
      </div>
    )
  }
}

class App extends Component {
  render() {
    const style = {
      flex: 1,
      justifyContent: 'center',
      alignItems: 'center',
    }

    return (
      <div style={style}>
        <Counter color={'lightblue'} size={16} />
        <Counter color={'skyblue'} size={32} />
        <Counter color={'steelblue'} size={80} />
        <Counter color={'darkblue'} size={140} />
      </div>
    )
  }
}

render(<App />, document.querySelector('#app'))
```

En este ejemplo se incluye un componente Contador (_Counter_) que mantiene el tiempo transcurrido internamente como _state.count_. El componente _App_ renderiza el componente Contador con dos _props_: el tamaño y el color.

Este ejercicio añade varios puntos que son importantes a la hora de entender React, como por ejemplo el estado (_state_). En los componentes se les puede pasar [_state_ y _props_](https://reactjs.org/docs/state-and-lifecycle.html). Los segundos serían como los argumentos de las funciones de javascript, mientras que los primeros serían como el estado interno del componente que, eventualmente, puede cambiar.

Lo primero que hace el componente _Counter_ es añadir el método _state_ con una propiedad, en este caso _count_. Esta propiedad _count_ se inicializa con un valor de 0.
>El _state_ lo define el usuario y debe ser un objeto Javascript.

```
state = {count: 0}
```

Posteriormente, se añade el método _componentDidMount()_.

```
componentDidMount() {
	setInterval(() => {
		this.setState({count: this.state.count + 1})
	}, 1000)
}
```
Los componentes tienen [un ciclo de vida](https://reactjs.org/docs/state-and-lifecycle.html) y en cada momento se puede y se debe [utilizar un método diferente](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/).

A [componentDidMount()](https://reactjs.org/docs/react-component.html#componentdidmount) se le llama cuando una instancia de un componente está siendo creado e insertado en el DOM. La inicialización que requiera nodos de DOM debe ir en este método, por lo que es el sitio idóneo para poner que se actualice el contador cada vez.

Respecto a la función _setInterval_, se establece para actualizar el valor del contador (_count_) cada segundo. Esta función se establece en formato _arrow functions_.
>Las [arrow functions](http://www.react.express/fat_arrow_functions) se usan para declarar funciones anónimas y tienen dos importantes características: el uso de _this_ es diferente del de las funciones tradicionales y no tienen argumentos.

Para actualizar el contador se pasa un objeto al método [_this.setState_](http://www.react.express/component_api). Este método es la manera que tienen los componentes de actualizar su estado. El objeto pasado es el contador _count_ actualizado con un valor de +1. Esto se hace con _this.state_ que es la manera de acceder al estado de un componente.

```
render() {
	const {count} = this.state
	const {color, size} = this.props

	return (
		<div style={{color, fontSize: size}}>
			{count}
		</div>
	)
}
```
Después se añade un método llamado _render()_ donde va el contenido del componente.
Ahí se declaran tres variables, _count_ y _color_ y _size_. La primera se iguala a _this.state_ para poder utilizarla directamente como {count} sin necesidad de escribir todas las veces _this.state.count_. Las segundas se iguala a _this.props_ ya que recibirá dos parámetros de fuera del Componente con ciertos valores que se devolverán en el apartado _return_.

```
class App extends Component {
  render() {
    const style = {
      flex: 1,
      justifyContent: 'center',
      alignItems: 'center',
    }

    return (
      <div style={style}>
        <Counter color={'lightblue'} size={16} />
        <Counter color={'skyblue'} size={32} />
        <Counter color={'steelblue'} size={80} />
        <Counter color={'darkblue'} size={140} />
      </div>
    )
  }
}
```

En el componente _App_ se añaden ciertos estilos en _render_ con el método _styel_ y después se devuelve un _div_ este método y llamando al contador del componente _Counter_ con dos argumentos, el color y el tamaño.

Al final se renderiza este componente _App_.
```
render(<App />, document.querySelector('#app'))

```

[Volver al Índice](#idindex)

<br>
**5. Inline Styles** <a name="id5"></a>

```
import React, { Component } from 'react'
import { render } from 'react-dom'

const randomColor = () => '#' + Math.random().toString(16).substr(-6)

class Card extends Component {
  render() {
    const style = {
      padding: 20,
      textAlign: 'center',
      color: 'white',
      backgroundColor: this.props.color,
    }

    return (
      <div style={style}>
        {this.props.children}
      </div>
    )
  }
}

class App extends Component {

  state = {
    color: 'skyblue'
  }

  randomizeColor = () => this.setState({color: randomColor()})

  render() {
    const {color} = this.state

    const style = {
      padding: 20,
    }

    return (
      <div style={style}>
        <Card color={color}>
          <input
            type={'button'}
            value={'Randomize Color'}
            onClick={this.randomizeColor}
          />
        </Card>
      </div>
    )
  }
}

render(<App />, document.querySelector('#app'))

```

En este ejemplo se construye un botón en el que, al pinchar en él, cambia de color de fondo de manera aleatoria. Esto se hace pasando estilos CSS a un _DOM component_.

Yendo al código, lo primero y más significativo es la variable _randomColor_, que almacena una función dónde, utilizando la función _Math.random_, se genera un número aleatorio entre 0 y 1 con decimales. Después, la función _toString_ la convierte a Hexadecimal y, por último, la función _substr_ devuelve 6 caracterés de ese string. Con eso y el # inicial se consigue un color en Hexadecimal.

```
const randomColor = () => '#' + Math.random().toString(16).substr(-6)
```
```
class Card extends Component {
  render() {
    const style = {
      padding: 20,
      textAlign: 'center',
      color: 'white',
      backgroundColor: this.props.color,
    }

    return (
      <div style={style}>
        {this.props.children}
      </div>
    )
  }
}
```
El componente _Card_ en el método _render_ añade los estilos CSS con el método _style_ y también acepta un argumento del color de fondo como una propiedad de dicho método. En la parte del _return_ retorna el método _style_ con todas las propiedad CSS antes definido y también el argumento recibido. Esto lo hace con _this.props.children_ que es un _props_ especial, típicamente definido por las tags hijas en una expresión JSX y que hace referencia al _this.props.color_ del método _style_.

```
state = {
	color: 'skyblue'
}

randomizeColor = () => this.setState({color: randomColor()})

render() {
	const {color} = this.state

```
El componente _App_ establece el color de fondo inicial, inicializándolo con el método _state_.
Después se añade una función _randomizeColor_ donde se cambia el color con _this.setState_ y aplicando a la variable _color_ la función _randomColor_ para que se cambie aleatoriamente el color de fondo.

En la parte de _render_ se iguala la variable _color_  a _this.state_ para poder utilizarla directamente como {color} sin necesidad de escribir todas las veces _this.state.color_ y también se añade un método _style_ con una propiedad CSS.

Posteriormente, en _return_ es dónde se renderiza todo.
```
return (
	<div style={style}>
		<Card color={color}>
			<input
				type={'button'}
				value={'Randomize Color'}
				onClick={this.randomizeColor}
			/>
		</Card>
	</div>
```

Aquí se puede ver que se establece el estilo inicial del componente _App_. Después se llama al componente _Card_ con la variable color como _prop_. Después se añade un _input_ con un botón y un texto y un evento _onClick_ llamando a la función _randomizeColor_ para cambiar el color cada vez que se pinche sobre el botón.

Al final se renderiza este componente _App_.

[Volver al Índice](#idindex)

<br>
**6. CSS-in-JS** <a name="id6"></a>
```
import React, { Component } from 'react'
import { render } from 'react-dom'
import styled from 'styled-components'

const randomColor = () => '#' + Math.random().toString(16).substr(-6)

const Card = styled.div`
  padding: 20px;
  text-align: center;
  color: white;
  background-color: ${props => props.color};
`

const Container = styled.div`
  padding: 20px;
`

class App extends Component {

  state = {
    color: 'skyblue'
  }

  randomizeColor = () => this.setState({color: randomColor()})

  render() {
    const {color} = this.state
    return (
      <Container>
        <Card color={color}>
          <input
            type={'button'}
            value={'Randomize Color'}
            onClick={this.randomizeColor}
          />
        </Card>
      </Container>
    )
  }
}

render(<App />, document.querySelector('#app'))

```

Este ejemplo tiene casi el mismo código que el anterior, exceptuando lo que hace referencia a los estilos que lo hace con una sintaxis diferente.
```
const Card = styled.div`
  padding: 20px;
  text-align: center;
  color: white;
  background-color: ${props => props.color};
`
```
Esta sintaxis se llama [_styled components_](https://github.com/styled-components/styled-components) y es casi igual al CSS estándar. Para utilizarla en React se debe importar la libreía con:
```
import styled from 'styled-components'
```
El resto de código es igual que el ejemplo anterior. Como significativo hay que comentar que el componente _Card_ sigue existiendo pero con la sintaxis específica de _styled components_ y también la manera en la que recibe el argumento con _props_.


---


En breve estará disponible la segunda parte de este repaso del tutorial de [react.express](http://www.react.express/).

To be continued ...
