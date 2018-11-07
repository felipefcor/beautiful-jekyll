---
layout: post
title: Aprendiendo React III. Siguiendo el tutorial de React.express
tags: [React, node.js, npm, webpack, babel, javascript, html, css]
---
### [Aprende Javascript con MentoringJS - Step 10 ](http://MentoringJS.com)
Esta es la tercera parte de mi revisión del tutorial de [react-express](http://www.react.express/).

[La primera parte se puede ver en mi blog](https://felipefcor.github.io/2018-10-26-Tutorial-React-express(I)).

[La segunda parte se puede ver en mi blog](https://felipefcor.github.io/2018-11-01-Tutorial-React-express(II))


### Índice de ejercicios<a name="idindex"></a>

[Ejercicios del 1 al 6](https://felipefcor.github.io/2018-10-26-Tutorial-React-express(I))

[Ejercicios del 7 al 14](https://felipefcor.github.io/2018-11-01-Tutorial-React-express(II))


<br>
**15. Component State (ToDo-List)**

Almacenar información en el _state_ de los componentes está bien para aplicaciones pequeñas y también para porciones de aplicaciones independientes del resto de la aplicación. La mejora manera de crear componentes es agruparlos en dos categorías: _containers_ y _componentes_.

Los _containers_ son conscientes de la información y de la lógica de la aplicación. También pueden pasar información y _callbacks_ como _props_ a los componentes de presentación y también controlar la actualización de información cuando el usuario interactúa con la aplicación.

_Componentes simples/presentacionales_.
La mayoría de componentes no contienen nada de lógica. Estos componentes de presentación se puede utilizar fácilmente en otra aplicación diferente ya que son completamente genéricos y sus únicas entradas son sus propios _props_. Los componentes de presentación normalmente se llaman _componentes_.

Para aclarar estos conceptos se va a mostrar una aplicación (una ToDo-List) con varios de los componentes mencionados.

Esta aplicación tiene 1 _container_ y 3 _componentes_. Normalmente, estos _containers_ y _componentes_ suelen ir cada uno en su propio fichero y se debe exportar cada uno posteriormente.

**Ficheros:**<a name="idficheros"></a>

+ [index.js](#idindexjs)
El índex es la entrada del proyecto. Es la entrada para el _bundle_ de Javascript y será el que renderizará el componente _root_ en el DOM.

+ [App.js](#idappjs)
App es el componente _container_ "inteligente", el que contendrá la información y lógica de la ToDo-List para añadir y eliminar ítems. App renderizará los otros componentes, _List_, _Input_ y _Title_, pasando la información y _callbacks_ de la ToDo-List para modificar la lista.

+ [List.js](#idlistjs)
Este componente renderiza una lista de _strings_. Se dispara con el _callback_ _onClickItem_ cuando un ítem es apretado.

+ [Input.js](#idinputjs)
Este componente renderiza un campo de entrada. Mantiene el _input_ actual en su estado y entonces lanza un _callback_, _onSubmitEditing_ cuando el usuario aprieta Enter.

+ [Title.js](#idtitlejs)
Un componente de título sencillo. _Puro componente de presentación_

---
**index.js**<a name="idindexjs"></a>

```
import { render } from "react-dom";
import React from "react";

import App from "./App";

render(<App />, document.querySelector("#app"));
```

Aquí se hacen varios imports de las librerías de React y también el componente _App_ del fichero _App.js_ del mismo directorio de trabajo.

Aquí vemos una novedad, el _import_ del componente _App_ se hace sin {}. Esto es debido a que el componente (clase) _App_ del fichero App.js se ha exportado de ese fichero como _default_. En la explicación del archivo App.js se verá más en profundidad.

Al final renderiza este componente _App_ al DOM.


[Volver a los ficheros](#idficheros)



**App.js**<a name="idappjs"></a>

```
import React, { Component } from 'react'

import List from './List'
import Input from './Input'
import Title from './Title'

export default class App extends Component {

  state = {
    todos: ['Click to remove', 'Learn React', 'Write Code', 'Ship App'],
  }

  onAddTodo = (text) => {
    const {todos} = this.state

    this.setState({
      todos: [text, ...todos],
    })
  }

  onRemoveTodo = (index) => {
    const {todos} = this.state

    this.setState({
      todos: todos.filter((todo, i) => i !== index),
    })
  }

  render() {
    const {todos} = this.state

    return (
      <div style={styles.container}>
        <Title>
          To-Do List
        </Title>
        <Input
          placeholder={'Type a todo, then hit enter!'}
          onSubmitEditing={this.onAddTodo}
        />
        <List
          list={todos}
          onClickItem={this.onRemoveTodo}
        />
      </div>
    )
  }
}

const styles = {
  container: {
    display: 'flex',
    flexDirection: 'column',
  }
}
```

La primera y más llamativa diferencia con el resto de ejemplos que se han ido viendo es la siguiente:
```
import React, { Component } from 'react'

import List from './List'
import Input from './Input'
import Title from './Title'

export default class App extends Component {
```
Aquí no solo salen algunos _imports_ diferentes (como en el index.js) sino también el componente (clase) _App_ es algo diferente.

En primer lugar, el módulo _App.js_, como todos los demás excepto _index.js_, no tienen al principio del archivo el import de react-dom. Esto es debido a que no les hace falta importar de React el componente react-dom ya que no van a renderizar nada al DOM. El único fichero que renderiza es el _index.js_ que hace una importación del fichero _App.js_ que será el único que haga una renderización al DOM. Este fichero _App.js_ sí que tiene el import de react-dom tal y como hemos visto.

En segundo lugar, nos encontramos con _export default_.

Cuando se trabajo con _módulos_, es decir, ficheros separados donde en cada uno hay un componente, las clases creadas en cada módulo son privadas. Para hacerlas accesibles desde otros módulos (ficheros) se debe hacer pública. Esto es lo que se hace con _export_. Además, como se ve por ejemplo en List.js, el componente _List_ se hace de la siguiente manera:

```
export default class List extends Component {}
```
Aquí vemos que se exporta el componente como _export default_. Este default se añade para indicar a React que este componente será el por defecto a exportar. Cuando se hace así, desde el módulo o fichero que se llame a este componente se puede llamar sin necesidad de añadir {}.

Esto es lo que se conoce como [_Export name/default_ clases](https://reactjs.org/docs/code-splitting.html#named-exports).

Respecto al código:

En primer lugar, se establece un _state_ con un array:
```
state = {
  todos: ['Click to remove', 'Learn React', 'Write Code', 'Ship App'],
}
```

Posteriormente, se añaden dos funciones en formato _arrow_functions_:
```
onAddTodo = (text) => {
  const {todos} = this.state

  this.setState({
    todos: [text, ...todos],
  })
}
```
Esta función sirve para añadir un nuevo elemento al array _todos_. La función tiene un parámetro _text_ que será lo que se añada al array. Esto se hace con _this.setState_ y modificando el array _todos_ añadiendo _text_. Esto vemos que se hace con el [_spread operator_](http://www.react.express/array_spread), característica de ES2015.

La siguiente función, _onRemoveTodo_ es la encargada de borrar un elemento de la lista del ToDo.
```
onRemoveTodo = (index) => {
  const {todos} = this.state

  this.setState({
    todos: todos.filter((todo, i) => i !== index),
  })
}
```

Aquí vemos que se le pasa un parámetro _index_ a dicha función. Dentro de _this.setState_ se añade una función nueva y se le asigna el resultado a _todos_.
En esta función se usa el método _filter_ que sirve para que revise (itere) por todos los elementos del array _todos_ y le pase el predicado de la función a cada elemento del array. La función en _arrow_functions_ tiene dos parámetros, _todo_ e _i_, mientras que el contenido es que _i_ sea diferente de _index_, que es el parámetro de la función principal, _onRemoveTodo_.

Básicamente, esta función recibe un elemento (index) a borrar (que se llamará más tarde con un evento) y lo que hace es revisar todo el array para devolver el mismo array pero sin ese elemento que se ha seleccionado.

Después llegamos a la parte del método _render_ donde se renderiza el contenido final del componente _App_.
```
render() {
  const {todos} = this.state

  return (
    <div style={styles.container}>
      <Title>
        To-Do List
      </Title>
      <Input
        placeholder={'Type a todo, then hit enter!'}
        onSubmitEditing={this.onAddTodo}
      />
      <List
        list={todos}
        onClickItem={this.onRemoveTodo}
      />
    </div>
  )
}
}
```

En esta parte del código se devuelve un _div_ con unos estilos determinados posteriormente y con diverso contenido JSX y llamando a los otros componentes exportados de la aplicación, como _Title_, _Input_ y _List_.

En cada parte de este componente se añade contenido específico de ese módulo.

En _Title_ se añade simplemente el título de la ToDo-List.

En _Input_ se añade un _placeholder_ y un evento _onSubmitEditing_ llamando a la función _onAddTodo_ que sirve para añadir un nuevo elemento.

En _List_ se añade contenido dinámico con el contenido del array _todos_ y también un evento _onClickItem_ llamando a la función de eliminar elementos _onRemoveTodo_.


Al final de todo se añade el contenido CSS que se utiliza para darle estilos a la lista To-Do.
```
const styles = {
  container: {
    display: 'flex',
    flexDirection: 'column',
  }
}
```


[Volver a los ficheros](#idficheros)



**List.js**<a name="idlistjs"></a>

```
import React, { Component } from "react";

export default class List extends Component {
  renderItem = (text, i) => {
    const { onClickItem } = this.props;

    return (
      <div style={styles.item} onClick={() => onClickItem(i)}>
        {text}
      </div>
    );
  };

  render() {
    const { list } = this.props;

    return (
      <div style={styles.container}>
        {list.map(this.renderItem)}
      </div>
    );
  }
}

const styles = {
  container: {
    display: "flex",
    flexDirection: "column"
  },
  item: {
    backgroundColor: "whitesmoke",
    marginBottom: 5,
    padding: 15
  }
};
```

En este módulo _List.js_ se exporta por defecto el componente _List_.
En este componente  devuelve una lista de arrays tal y como se ve en el _return_ del método _render_.
```
return (
      <div style={styles.container}>
        {list.map(this.renderItem)}
      </div>
    );
```
Aquí se ve que se le pasa el método _map_ a la constante _list_ que antes se ha definido. La función que se llama para crear este nuevo array es _renderItem_.
```
renderItem = (text, i) => {
  const { onClickItem } = this.props;

  return (
    <div style={styles.item} onClick={() => onClickItem(i)}>
      {text}
    </div>
  );
};
```
Esta función tiene dos parámetros, _text_ e _i_. Se declara dentro una constante _onClickItem_ con contenido dinámico que será aceptada como _prop_. Esta función devuelve código JSX con unos estilos CSS definidos posteriormente y un evento _onClick_ llamando al ítem concreto que se ha apretado y que se representa con _onClickItem_.

Al final se declara un objeto de estilos CSS con varias propiedades:
```
const styles = {
  container: {
    display: "flex",
    flexDirection: "column"
  },
  item: {
    backgroundColor: "whitesmoke",
    marginBottom: 5,
    padding: 15
  }
};
```

[Volver a los ficheros](#idficheros)


**Input.js**<a name="idinputjs"></a>

```
import React, { Component } from "react";

export default class Input extends Component {
  state = {
    value: ""
  };

  handleChange = e => {
    this.setState({ value: e.target.value });
  };

  handleKeyPress = e => {
    if (e.key !== "Enter") return;

    const { onSubmitEditing } = this.props;
    const { value } = this.state;

    if (!value) return; // Don't submit if empty

    onSubmitEditing(value);
    this.setState({ value: "" });
  };

  render() {
    const { placeholder } = this.props;
    const { value } = this.state;

    return (
      <input
        style={styles.input}
        type={"text"}
        value={value}
        placeholder={placeholder}
        onChange={this.handleChange}
        onKeyPress={this.handleKeyPress}
      />
    );
  }
}

const styles = {
  input: {
    fontSize: "100%",
    padding: 15,
    borderWidth: 0
  }
};
```

Este módulo es casi equivalente al que vimos en la parte de _Input Handling_ del [propio tutorial de react.express](http://www.react.express/data_component_state).
También lo revisé en [mi segundo artículo sobre React](https://felipefcor.github.io/2018-11-01-Tutorial-React-express(II)).

Lo único que cambia es que se añade una nueva función(_handleKeyPress_) para comprobar si el usuario ha apretado la tecla Enter.
```
handleKeyPress = e => {
  if (e.key !== "Enter") return;

  const { onSubmitEditing } = this.props;
  const { value } = this.state;

  if (!value) return; // Don't submit if empty

  onSubmitEditing(value);
  this.setState({ value: "" });
};
```

En esta función se añade un condicional en el que se indica que si no se aprieta Enter no se devuelva nada. Si además el valor de _value_ está vacío tampoco se devuelve nada. Tan solo se devuelve en los casos contrarios. Cuando se cambia el valor de _value_ y se aprieta Enter se llama a la función _onSubmitEditing_.

Posteriormente se devuelve en _return_ contenido JSX con un campo _input_ con ciertos estilos CSS y algunas propiedades de dicho campo. Las más llamativas son _onChange_ y _onKeyPress_ que llaman a las dos funciones declaradas previamente, _handleChange_ y _handleKeyPress_.

Finalmente se añade un objeto _styles_ con ciertos estilos CSS.


[Volver a los ficheros](#idficheros)


**Title.js**<a name="idtitlejs"></a>

```
import React, { Component } from "react";

export default class Title extends Component {
  render() {
    const { children } = this.props;

    return (
      <div style={styles.header}>
        <div style={styles.title}>{children}</div>
      </div>
    );
  }
}

const styles = {
  header: {
    backgroundColor: "skyblue",
    padding: 15
  },
  title: {
    textAlign: "center",
    color: "white"
  }
};
```

Este módulo tiene un componente que exporta por defecto, _Title_. En este componente se le pasa como _prop_ [_children_](https://reactjs.org/docs/glossary.html#propschildren) que será lo que se devuelva en el _return_. Ahí se devuelve código JSX con estilos CSS y con el contenido dinámico pasado como _prop_.

Este contenido dinámico que se pasa como _children_ es el que se pasa cuando se llama al componente _Title_ desde el módulo _app_
```
return (
      <div style={styles.container}>
        <Title>
          To-Do List
        </Title>
```

Finalmente se añade un objeto _styles_ con ciertos estilos CSS.


---


Con este artículo finalizo la revisión del tutoriol de React.express.
En siguientes posts seguiré con React a tope. Stay tuned!
