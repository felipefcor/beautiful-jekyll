---
layout: post
title: Aprendiendo React II. Siguiendo el tutorial de React.express
tags: [React, node.js, npm, webpack, babel, javascript, html, css]
---
### [Aprende Javascript con MentoringJS - Step 10 ](http://MentoringJS.com)
Esta es la segunda parte de mi revisión del tutorial de [react-express](http://www.react.express/).

[La primera parte se puede ver en mi blog](https://felipefcor.github.io/2018-10-26-Tutorial-React-express(I)).


### Índice de ejercicios<a name="idindex"></a>

[Ejercicios del 1 al 6](https://felipefcor.github.io/2018-10-26-Tutorial-React-express(I))

7.[Event Handling I](#id7)<br>
8.[Event Handling II](#id8)<br>
9.[Custom Components and Events](#id9)<br>
10.[Input Handling](#id10)<br>
11.[Conditional Rendering I y II](#id11)<br>
12.[Conditional Rendering III](#id12)<br>
13.[Lists and keys](#id13)<br>
14.[Refs and the DOM](#id14)<br>

<br>
**7. Event Handling I**<a name="id7"></a>
```
import React, { Component } from 'react'
import { render } from 'react-dom'

class CounterButton extends Component {

  state = {count: 0}

  render() {
    const {count} = this.state

    return (
      <button type='button' onClick={() => this.setState({count: count + 1})}>
        Click HERE to increment: {count}
      </button>
    )
  }
}

render(<CounterButton />, document.querySelector('#app'))
```
Para empezar, están los dos _imports_ habituales, el _component_ de _react_ y el _render_ de _react-dom_.

Después se crea un componente con forma de clase: _CounterButton_.

En este componente se añade el método _state_ con una propiedad, en este caso _count_. Esta propiedad _count_ se inicializa con un valor de 0.
En el método _render_ se declara la variable _count_ y se iguala a _this.state_ para poder utilizarla directamente como _{count}_ sin necesidad de escribir todas las veces _this.state.count_.

En la parte del _return_ se añade un botón en JSX con el evento _onClick_ para que cada vez que se pinche se cambie el estado de la variable _count_ sumándole una unidad. El cambio se hace con _this.setState_ que es la manera de cambiar el estado establecido en _setState_ en React.

Por último,[indicar que para añadir un _callback_ a un evento](http://www.react.express/event_handling), se debe pasar una función como un atributo de un elemento React. Esto es lo que pasa en el botón con _onClick_.

Al final se renderiza el componente _CounterButton_.

[Volver al Índice](#idindex)

<br>
**8. Event Handling II**<a name="id8"></a>
```
import React, { Component } from 'react'
import { render } from 'react-dom'

class CounterButton extends Component {

  state = {count: 0}

  handleClick = () => {
    const {count} = this.state

    this.setState({count: count + 1})
  }

  render() {
    const {count} = this.state

    return (
      <button type='button' onClick={this.handleClick}>
        Click HERE to increment: {count}
      </button>
    )
  }
}

render(<CounterButton />, document.querySelector('#app'))
```
Esta es una versión mejorada, como [se dice en el tutorial](http://www.react.express/event_handling).

En el ejemplo anterior se declara una función dentro de los _props_ del elemento React. Esto no es lo mejor ya que cada vez que se llama a _render_ se crea una función nueva ya que el componente compara los _props_ y ve que el _prop_ de _onClick_ ha cambiado. Esto puede causar innecesarios re-renders y un mal rendimiento.

Para evitar dicha situación se puede crear una función _handleClick_ en el cuerpo del componente. Ahí se debe poner la variable _const_ y también el _this.setState_ para ir cambiando el valor de _const_.

Por último, en el _return_, el evento _onClick_ llama a la función _handleClick_.

[Volver al Índice](#idindex)

<br>
**9. Custom Components and Events**<a name="id9"></a>

```
import React, { Component } from 'react'
import { render } from 'react-dom'

class CounterButton extends Component {
  render() {
    const {onPress, children} = this.props

    return (
      <button type='button' onClick={onPress}>
        {children}
      </button>
    )
  }
}

class App extends Component {

  state = {count: 0}

  handlePress = () => {
    const {count} = this.state

    this.setState({count: count + 1})
  }

  render() {
    const {count} = this.state

    return (
      <CounterButton
        count={count}
        onPress={this.handlePress}
      >
        Click HERE to increment: {count}
      </CounterButton>
    )
  }
}

render(<App />, document.querySelector('#app'))
```

Este ejemplo tiene el mismo resultado que los dos anteriores. La única diferencia es que se hace un componente personalizado con el evento _onClick_.

Como solo los componentes DOM pueden controlar eventos como _onClick_, el componente personalizado (en este caso _CounterButton_) debe renderizar un componente DOM y pasarle el _prop_ _onClick_. El componente _CounterButton_ simplemente es un puente por el que pasa el evento _onClick_.

```
class CounterButton extends Component {
  render() {
    const {onPress, children} = this.props

    return (
      <button type='button' onClick={onPress}>
        {children}
      </button>
    )
  }
}
```
Este el componente _CounterButton_. Aquí se añaden dos constantes, _onPress_ y _children_ que se igualan a _this.state_ para poder utilizarlas directamente por sus nombres sin necesidad de escribir todas las veces _this.state.count_.

En la parte del _return_ se devuelve un botón JSX con el evento _onClick_ que llama a la constante _onPress_, que más abajo se definirá.

```
class App extends Component {

  state = {count: 0}

  handlePress = () => {
    const {count} = this.state

    this.setState({count: count + 1})
  }

  render() {
    const {count} = this.state

    return (
      <CounterButton
        count={count}
        onPress={this.handlePress}
      >
        Click HERE to increment: {count}
      </CounterButton>
    )
  }
}
```

En el componente _App_ se añade el método _state_ con la constante _count_ inicializándola a 0. Después se crea la función _handlePress_ que es dónde se cambia el valor de _count_ añadiéndole una unidad.

En el método _render_ se devuelve contenido JSX llamando a la constante _count_ y a la constante _onPress_. Esta última llama a la función _handlePress_.

Con todo ello se consigue que cada vez que se pinche en el botón creado en el componente de _CounterButton_ este llame a la constante _onPress_ que a su vez llama la función _handlePress_ que es dónde se añade una unidad a la constante _count_ que también se añade al DOM.

[Volver al Índice](#idindex)
<br>

**10. Input Handling**<a name="id10"></a>

Tradicionalmente, en el desarrollo web los _input_ de los usuarios se guardan en el DOM y la información que escribe el usuario es extraída del DOM para ser usada en la aplicación. React simplifica esto tratando los elementos _input_ como sin estado.  Un elemento _input_ tiene dos _props_, una _value_ y otra _onChange_ y estas dos _props_ te dan el control total de los _inputs_ sin tener que tocar el DOM.

El siguiente ejemplo muestra que cada vez que se renderiza el _input_, se pasa el actual valor de _value_ del estado del componente. Cada vez que el usuario escribe en el campo del _input_, se actualiza el estado para incluir el nuevo estado, lo que lanza una re-renderización. Con esto se puede controlar el valor del campo _input_ sin preocuparse de las operaciones del DOM, que hace React por debajo.

```
import React, { Component } from 'react'
import { render } from 'react-dom'

class Input extends Component {

  state = {value: ''}

  handleChange = (e) => {
    this.setState({value: e.target.value})
  }

  render() {
    const {value} = this.state

    return (
      <div>
        <label htmlFor={'id'}>
          Enter value
        </label>
        <input
          id={'id'}
          type={'text'}
          value={value}
          placeholder={'Placeholder'}
          onChange={this.handleChange}
        />
        <br />
        <br />
        My value: {value}
      </div>
    )
  }
}

render(<Input />, document.querySelector('#app'))

```
Respecto al código en sí, en la clase _Input_ se añade un _state_ vacío y una función _handleChange_ donde se actualiza el valor de _value_ que pasará el usuario más adelante (en el _return_ en el apartado JSX).

En el _return_ se añade código JSX con un _label_ y un _input_ que es dónde irá casi toda la información, incluyendo la _prop_ _onChange_ que recibe el contenido del usuario y lo actualiza llamando a la función _handleChange_. Por último, se renderiza el valor de _value_.

[Volver al Índice](#idindex)

<br>
**11. Conditional Rendering I y II**<a name="id11"></a>

En este ejemplo se muestra cómo trabajar con condicionales en React.

```
import React, { Component } from 'react'
import { render } from 'react-dom'

class Card extends Component {
  render() {
    const {title, subtitle} = this.props

    return (
      <div style={styles.card}>
        <h1 style={styles.title}>{title}</h1>
        {subtitle && (
          <h2 style={styles.subtitle}>{subtitle}</h2>
        )}
      </div>
    )
  }
}

class App extends Component {
  render() {
    return (
      <div>
        <Card title={'Title'} />
        <Card title={'Title'} subtitle={'Subtitle'} />
      </div>
    )
  }
}

const styles = {
  card: {
    padding: 20,
    margin: 20,
    textAlign: 'center',
    color: 'white',
    backgroundColor: 'skyblue',
    border: '1px solid rgba(0,0,0,0.15)',
  },
  title: {
    fontSize: 18,
    lineHeight: '24px',
  },
  subtitle: {
    fontSize: 14,
    lineHeight: '18px',
  },
}

render(<App />, document.querySelector('#app'))
```

El ejemplo concreto renderiza contenido con el condicional **&&** en función de si existe un _prop_ o no. Si existe el _prop_ subtitulo lo muestra y, si no existe, no lo muestra.

```
class Card extends Component {
  render() {
    const {title, subtitle} = this.props

    return (
      <div style={styles.card}>
        <h1 style={styles.title}>{title}</h1>
        {subtitle && (
          <h2 style={styles.subtitle}>{subtitle}</h2>
        )}
      </div>
    )
  }
}
```
En el componente _Card_ se añaden dos _props_: _title_ y _subtitle_. Se devuelve código JSX con estilos CSS (el genérico de card y el de _title_ y _subtitle_ para cada uno) y el condicional && para mostrar contenido en función de si se recibe el _prop_ _subtitle_.

```
class App extends Component {
  render() {
    return (
      <div>
        <Card title={'Title'} />
        <Card title={'Title'} subtitle={'Subtitle'} />
      </div>
    )
  }
}
```
El componente _App_, que es el que se renderiza al final, devuelve código JSX con dos llamadas al componente _Card_. La primera solo con una _prop_ y la segunda con dos. Este resultado muestra dos recuadros, el primero solo muestra el título en h1 mientras que el segundo muestra el título en h1 y el subtítulo en h2.

El condicional && funciona porque como no se le pasa un subtítulo se interpreta la parte izquierda del condicional como _undefined_ por lo que React no renderiza nada más, es decir, lo que hay a la derecha de &&. Cuando se le pasa algo más la parte izquierda es _truthy_ con lo que sí que se revisa lo de la parte derecha de la condición y se renderiza el subtítulo.

Al final se declara un método de estilos CSS con varias propiedades.

---
También se puede utilizar otro operador para el condicional, como por ejemplo el [operador ternario](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Operadores/Conditional_Operator)
```
class Card extends Component {
  render() {
    const {title, subtitle} = this.props

    return (
      <div style={styles.card}>
        <h1 style={styles.title}>{title}</h1>
        {subtitle ? (
          <h2 style={styles.subtitle}>{subtitle}</h2>
        ) : (
          <h3 style={styles.empty}>No subtitle</h3>
        )}
      </div>
    )
  }
}
```
En este caso, se renderiza el subtítulo si existe y si no existe se le pasa _No subtitle_ con unos estilos CSS específcos.

[Volver al Índice](#idindex)

<br>
**12. Conditional Rendering III**<a name="id12"></a>

El último caso de condicionales es con un _if/else_.
```
import React, { Component } from 'react'
import { render } from 'react-dom'

class Card extends Component {
  renderContent() {
    const {title, subtitle} = this.props

    return (
      <div>
        <h1 style={styles.title}>{title}</h1>
        {subtitle ? (
          <h2 style={styles.subtitle}>{subtitle}</h2>
        ) : (
          <h3 style={styles.empty}>No subtitle</h3>
        )}
      </div>
    )
  }

  render() {
    const {loading, error} = this.props

    let content

    if (error) {
      content = 'Error'
    } else if (loading) {
      content = (
        <h3 style={styles.empty}>Loading...</h3>
      )
    } else {
      content = this.renderContent()
    }

    return (
      <div style={styles.card}>
        {content}
      </div>
    )
  }
}

class App extends Component {
  render() {
    return (
      <div>
        <Card loading={true} />
        <Card error={true} />
        <Card title={'Title'} subtitle={'Subtitle'} />
      </div>
    )
  }
}

const styles = {
  card: {
    padding: 20,
    margin: 20,
    textAlign: 'center',
    color: 'white',
    backgroundColor: 'skyblue',
    border: '1px solid rgba(0,0,0,0.15)',
  },
  title: {
    fontSize: 18,
    lineHeight: '24px',
  },
  subtitle: {
    fontSize: 14,
    lineHeight: '18px',
  },
  empty: {
    fontSize: 12,
    lineHeight: '15px',
    opacity: 0.5,
  }
}

render(<App />, document.querySelector('#app'))
```

Este ejemplo tiene la primera parte de código igual al anterior ejemplo. Cambia en que declara un método _renderContent_ donde declara las dos constantes _title_ y _subtitle_:

```
renderContent() {
    const {title, subtitle} = this.props
```
Después, crea otro método _render()_ y añade dos constantes más y un condicional con if, else if y else.

Los casos son, si la constante _error_ es verdadera, la variable _content_ declarada antes tendrá el contenido Error. Si la constante _loading_ es verdadera, el contenido a mostrar es un código JSX con Loading y estilos CSS (empty). Si alguno no es verdadero se llama al contenido de _this._renderContent()_ que en este caso devolverá "No subtitle" ya que no tienen subtítulo.
```
render() {
    const {loading, error} = this.props

    let content

    if (error) {
      content = 'Error'
    } else if (loading) {
      content = (
        <h3 style={styles.empty}>Loading...</h3>
      )
    } else {
      content = this.renderContent()
    }

    return (
      <div style={styles.card}>
        {content}
      </div>
    )
  }
}
```
El contenido a renderizar y dónde se pueden modificar lo que se muestre está en la clase _App_.
```
class App extends Component {
  render() {
    return (
      <div>
        <Card loading={true} />
        <Card error={true} />
        <Card title={'Title'} subtitle={'Subtitle'} />
      </div>
    )
  }
}
```

[Volver al Índice](#idindex)

<br>
**13. Lists and Keys**<a name="id13"></a>

A cada componente se le puede pasar una _prop_ especial llamada _key_. React utiliza dicha _key_ para determinar la identidad del elemento renderizado.

En componentes individuales, a diferencia de las listas de componentes, React automáticamente asigna una _key_ a los elementos basado en el orden de renderizado. Un ejemplo sería:
```
(
  <div>
    <h1>Title</h1>
    <h2>Subtitle</h2>
  </div>
)
```
La asignación por React de las _keys_ sería la siguiente
```
div: 0
  h1: 0.0
  h2: 0.1
```
En las listas funciona diferente ya que se gestiona mapeando un array con el [método _map()_](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Objetos_globales/Array/map):

```
import React, { Component } from 'react'
import { render } from 'react-dom'

const data = [
  {id: 'a', name: 'Devin'},
  {id: 'b', name: 'Gabe' },
  {id: 'c', name: 'Kim'},
]

class List extends Component {
  render() {
    return (
      <div>
        {data.map(item => <div key={item.id}>{item.name}</div>)}
      </div>
    )
  }
}

render(<List />, document.querySelector('#app'))
```

En este ejemplo se declara un array llamado "data" con un _id_ y con un _name_.
Para extraer esos datos se utiliza el [método _map()_](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Objetos_globales/Array/map) en formato _arrow_functions_ y con un parámetro, _item_. Ahí se le pasa contenido JSX con el contenido dinámico de _id_ y _name_ del array. El resultado interno es que se muestra los tres nombres  pero además con el id asignado correctamente en cada ítem.
```
(
  <div>
    {[
      <div key={'a'}>{'Devin'}</div>
      <div key={'b'}>{'Gabe'}</div>
      <div key={'c'}>{'Kim'}</div>
    ]}
  </div>
)
```

Esto se puede mejorar utilizando el índex del propio _map_ como _key_:
```
(
  <div>
    {data.map((item, index) => <div key={index}>{item.name}</div>)}
  </div>
)
```

En este caso, se añade el parámetro _index_ a la función que tiene por objetivo utilizar el propio índex de _map_. El ejemplo sería igual exceptuando que se debería quitar la propiedad _id_ del array. Quedaría así el array:
```
const data = [
  {name: 'Devin'},
  {name: 'Gabe' },
  {name: 'Kim'},
]
```
El resultado sería:
```
(
  <div>
    {[
      <div key={0}>{'Devin'}</div>
      <div key={1}>{'Gabe'}</div>
      <div key={2}>{'Kim'}</div>
    ]}
  </div>
)
```

[Volver al Índice](#idindex)

<br>
**14. Refs and the DOM**<a name="id14"></a>

A veces cuando trabajas con React es necesario acceder directamente a los nodos del DOM subyacentes que renderizas, ya que es posible que se quiera medir un nodo o tener la posición de scroll. O incluso es posible que se necesite interactuar con una librería diferente que modifica directamente el DOM. React proporciona una solución para esto con la _prop_ llamada _ref_.

Trabajando con _ref_:

Se puede pasar una función _callback_ como _ref_ que será llamada por la instancia del componente antes de la renderización inicial. Se puede guardar la referencia para usarla dentro del ciclo de vida de React. La instancia o bien será un _Custom Component_ o un nodo DOM. En cada caso se puede llamar a métodos de esta instacia.


```
import React, { Component } from 'react'
import { render } from 'react-dom'

class Card extends Component {
  state = {
    width: null,
    height: null,
  }

  saveRef = (ref) => this.containerNode = ref

  measure() {
    const {clientWidth, clientHeight} = this.containerNode

    this.setState({
      width: clientWidth,
      height: clientHeight,
    })
  }

  componentDidMount() {
    this.measure()
  }

  componentDidUpdate() {
    this.measure()
  }

  shouldComponentUpdate(nextProps, nextState) {
    return (
      this.state.width !== nextState.width ||
      this.state.height !== nextState.height
    )
  }

  render() {
    const {width, height} = this.state

    return (
      <div
        style={styles.card}
        ref={this.saveRef}
      >
        <h2 style={styles.subtitle}>My dimensions are:</h2>
        {width && height && (
          <h1 style={styles.title}>{width} x {height}</h1>
        )}
      </div>
    )
  }
}

const styles = {
  card: {
    padding: 20,
    margin: 20,
    textAlign: 'center',
    color: 'white',
    backgroundColor: 'skyblue',
    border: '1px solid rgba(0,0,0,0.15)',
  },
  title: {
    fontSize: 18,
    lineHeight: '24px',
  },
  subtitle: {
    fontSize: 14,
    lineHeight: '18px',
  },
}

render(<Card />, document.querySelector('#app'))
```
El funcionamiento general de este ejemplo es que, después del primer renderizado, se guarda el inicial _width_ y _height_ con _setState_. Esto hace disparar una segunda renderización que muestra estas dos características lo que cambiará el valor de ambas de nuevo. Ese cambio lanza una tercera renderización que mostrará el último valor de _width_ y _height_, y con fortuna, no se cambiarán las dimensiones de nuevo. Si así pasase, se podría entrar en un bucle infinito. Para evitar el bucle se añade al ciclo de vida _shouldComponentUpdate_.

[Volver al Índice](#idindex)

----
Próximo y último capítulo en breve...
