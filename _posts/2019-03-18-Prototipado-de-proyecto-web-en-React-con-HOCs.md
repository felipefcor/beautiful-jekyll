---
layout: post
title: Prototipado de proyecto web en React con HOCs
tags: [javascript, ES6, React, components, HOCs, refactoring]
---
### [Aprende Javascript con MentoringJS - Step 13 ](http://MentoringJS.com)
En este artículo voy a explicar como he intentado refactorizar el proyecto web hecho en React de una web de fisioterapia basándome en el concepto de HOCs y el de cómo separar componentes.

Antes de nada, los artículos sobre mi proyecto anterior en los que se basa este artículo se pueden ver en mi blog en las siguientes direcciones:

<a href="https://felipefcor.github.io/2018-12-14-Prototipado-de-mi-proyecto-en-React-I">Prototipado-de-mi-proyecto-en-React-I</a>
<br>
<a href="https://felipefcor.github.io/2018-12-14-Prototipado-de-mi-proyecto-en-React-II">Prototipado-de-mi-proyecto-en-React-II</a>

Para realizar esta revisión (o refactorización) me he basado en algunos artículos dónde se explica y detalla que significa HOCs y también cómo separar componentes:

- [Atributos de una correcta arquitectura](https://dmitripavlutin.com/7-architectural-attributes-of-a-reliable-react-component/?utm_source=reactnl&utm_medium=email)
- [Como se debería separar Componentes](https://reactarmory.com/answers/how-should-i-separate-components)
- [High Order Components: Un patrón de diseño de aplicaciones React](https://www.sitepoint.com/react-higher-order-components/)
- [Estructurando nuestra aplicaciones en React gracias a los HOCs](http://jamesknelson.com/structuring-react-applications-higher-order-components/)
- [React HOCs en profundidad](https://medium.com/@franleplant/react-higher-order-components-in-depth-cf9032ee6c3e)

En primer lugar, habría que aclarar que son los HOCs (o Higher-Order Components). Según James K. Nelson, este tipo de componentes serían [funciones JavaScript que añaden funcionalidad a las clases de componentes existentes](http://jamesknelson.com/structuring-react-applications-higher-order-components/). Es decir, serían componentes de componentes.

Respecto a la refactorización del código, a continuación voy a ir explicando en detalle qué he pensado y cómo he ido modificando mis componentes.

- Lo primero que he hecho ha sido reestructurar las carpetas del proyecto tal y como [Dave Ceddia](https://daveceddia.com/react-project-structure/) indicaba en un artículo y [yo recogí en otro](https://felipefcor.github.io/2019-01-17-React-desde-dentro/).

  Por ello, de la [estructura anterior](https://github.com/felipefcor/Prototipado-proyecto-React) he adaptado el proyecto a una nueva. En esta nueva estructura he añadido la carpeta _containers_ y he movido ahí todos los componentes que manejan datos o que, en un futuro es posible que gestiones datos. En la carpeta _components_ he dejado los componentes más representacionales: la galería, los de estilos y el de la barra de navegación.

  <img width="100" height="300" src="https://raw.githubusercontent.com/felipefcor/Prototipado-proyecto-React-HOCs/master/Selecci%C3%B3n_049.png">


  - Lo segundo que he hecho ha sido individualizar todas las funciones dentro de componentes en componentes propios. Esto lo he hecho sobre todo en el componente Home, ya que de ahí he sacado tres componentes más: [_Search.js_, _Settings.js_ y _Language.js_](https://github.com/felipefcor/Prototipado-proyecto-React-HOCs/tree/master/src/containers).


  - Lo tercero que he hecho ha sido separar componentes para que cada uno tenga [una sola responsabilidad (single responsibility)](https://dmitripavlutin.com/7-architectural-attributes-of-a-reliable-react-component/?utm_source=reactnl&utm_medium=email#1singleresponsibility). En mi versión anterior he detectado que había algunos componentes que tenían varias responsabilidades, como por ejemplo _AboutUs.js_ y _Appointment.js_.

  En el caso de _Home.js_ al haber separado los componentes que había en este, ha quedado solo una responsabilidad, la del _routing_ del índex. En este caso, no hay que hacer nada más según mi punto de vista.

  En el caso del sobre nosotros (_AboutUs_) habría que separar el mapeado de los trabajadores de las otras partes del componentes, como son la parte de la localización y de las redes sociales. Estos dos nuevos componentes he decidido ponerlos en el directorio  _components_ ya que son componente simples que muestran una imagen o mapa y datos (que podrían contener imágenes) que enlazan a las redes sociales. Se crean dos nuevos componentes: _AboutUsLocation.js_ y _AboutUsSocial.js_.

  Otro beneficio de separar en componentes pequeños es que [se pueden reutilizar](https://dmitripavlutin.com/7-architectural-attributes-of-a-reliable-react-component/?utm_source=reactnl&utm_medium=email#4reusable). Esta característica es bastante apreciable en el tema del componente de redes sociales ya que se podría reutilizar por toda la aplicación, incluso se podría poner un _footer_ estático con esa información.

  En el caso del componente _Appointment_ habría que separar el componente principal (Appointment) del _template_, es decir _AppointmentTemplate_. El primero tan solo renderiza lo que coge del segundo. El segundo, además, tendría que ser una plantilla con condicionales que sirviese cierto contenido al primero. Siguiendo esta lógica, creo un nuevo componente con la plantilla de las citas llamado _AppointmentTemplate.js_.


> Otros cambios que no he hecho pero que más adelante se deberían hacer son los que tienen que ver con los componentes de buscar y de ajustes. Estos dos componentes se gestionan a través de los [iconos de la parte superior (izquierda y derecha) de la página principal](http://felipefcor.surge.sh/). En ambos se gestionará estado, tanto para buscar como para realizar ajustes. Además, seguramente se tendrá que crear funciones que gestionen los eventos (de click y otros). Llegado el momento se deberá también separar estos componentes en otros más simples y también separarlos racionalmente para que el estado se gestione en el componente principal en ambos casos.

Revisando la mayoría de componentes de mi aplicación compruebo que la mayoría gestiona datos, bien para mostrar información sobre citas, trabajadores, servicios, etc. Entiendo que los datos que se utilizan en esos componentes y que solo los renderizan, se podrían poner en algún otro apartado dónde solo haya datos. Así quedaría todo más compacto y si se cambiasen los datos no se tendría que cambiar el componente en sí, ya que este cogería los datos de cualquier sitio y los mostraría en pantalla.

Tanto los cambios anteriores como el de separar los datos ayudarían a cumplir el [principio de encapsulación del que se habla en el artículo antes citado](https://dmitripavlutin.com/7-architectural-attributes-of-a-reliable-react-component/?utm_source=reactnl&utm_medium=email#2encapsulated).

>[Todos estos cambios se pueden ver en mi github](https://github.com/felipefcor/Prototipado-proyecto-React-HOCs/tree/master/src), dónde he ido subiendo los cambios que he ido haciendo en esta _refactorización_.


- **Uso de HOCs**

Respecto al uso de HOCs para refactorizar la aplicación, después de leer [el artículo de Jack Franklin](https://www.sitepoint.com/react-higher-order-components/), creo que se podría añadir el ejemplo que indica en el _routing_. En mi código no hay una llamada a alguna clase de CSS en dicho _routing_ pero es posible que en un futuro tenga que ponerla. En ese momento aprovecharía para hacer un _Componente de Orden Mayor (HOC)_ para no repetir la misma clase en todos los links del _routing_.

Se pasaría de esto:
```
<Link to="/" activeClassName="active-link">Home</Link>
<Link to="/about" activeClassName="active-link">About</Link>
<Link to="/contact" activeClassName="active-link">Contact</Link>
```
a esto:
```
<Link to={this.props.to} activeClassName="active-link">
        {this.props.children}
</Link>;

<AppLink to="/">Home</AppLink>
<AppLink to="/about">About</AppLink>
<AppLink to="/contact">Contact</AppLink>

```
Otra manera de construir HOCs sería a través de una función que recibe como parámetro una _prop_ y que devuelve un componente. En el ejemplo de Jack Franklin se utiliza para reutilizar un mismo compenente en el que solo cambie un parámetro, como por ejemplo a la hora de hacer login. Si hay un usuario se puede acceder al apartado correspondiente. Si no hay usuario, no se podría acceder.

Esto también lo menciona [franleplant en su artículo. Él lo llama _Props Proxy_](https://medium.com/@franleplant/react-higher-order-components-in-depth-cf9032ee6c3e).

Para _franleplant_ hay otro tipo de HOCs en React, que son los _Inheritance Inversion_. En este caso, se crearía una clase dentro de un componente que heredaría (o se extendería) las características del componente _padre_.

En ambos casos, no he encontrado la manera de relacionarlo o incluirlo en mi código. Es cierto que quizá hace falta muchas más vueltas para poder ver dicha relación y aplicarlo. De momento, lo voy a dejar aquí, en próximas revisiones es posible que vaya añadiéndolos poco a poco y la aplicación vaya quedando más compacta y mejor estructurada.

---
