# Introducción a React

## ¿Qué es React?
React es una herramienta de JavaScript que facilita la razonamiento, la construcción y el mantenimiento de interfaces de usuario sin estado y con estado. Proporciona los medios para definir declarativamente y dividir una interfaz de usuario en componentes de interfaz de usuario (a.k.a., componentes React) utilizando nodos similares a HTML denominados nodos React. Los nodos React eventualmente se transforman en un formato para renderizado de interfaz de usuario (por ejemplo, HTML / DOM, canvas, svg, etc ...).

**¿Queeeeeeeeeeeee?**

Que es una librería que nos permite crear interfaces de usuarios basada en componentes.

## Principios de React

React se basa en un *statement* que, inicialmente, causa controversia

> Re-renderizar todo con cada actualización

Esto, a pesar de sonar *caro* en términos de computación no lo es ya que la frase, tampoco es del todo *cierta*. 

React usa una representación virtual del arbol DOM, llamada virtual DOM, que es un descripción ligera de todos los componentes del UI. Con cada actualización, React actualiza todo el árbol aplicando sólo los cambios necesarios con un algoritmo de comparación muy eficiente que, finalmente, resulta en cambios mínimos en el DOM real con lo que se producen menos *re-flows*.

## ¿Qué es un componente React?

Los componentes son pequeños elementos de interfaz de usuario que muestran los datos a medida que cambia con el tiempo. Entonces estos componentes se componen juntos, anidados dentro de otros para crear interfaces de usuario enteras. Los sitios web o web apps pueden utilizar React para todas sus interfaces de usuario o sólo fragmentos de ella.

```js
class HelloMessage extends React.Component {
    render(){
        return <div>Hello {this.props.name}</div>;
    }
};

ReactDOM.render(<HelloMessage name="John" />, document.getElementById('app'));

```

## Componente React vs Web component

React y Web Components están diseñados para resolver diferentes problemas. Los web components proporciona una encapsulación fuerte para los componentes reutilizables, mientras que React proporciona una biblioteca declarativa que mantiene el DOM sincronizado con sus datos. Los dos objetivos son complementarios. Como desarrollador, puede utilizar React en sus componentes web o utilizar componentes web en React, o ambos.

La mayoría de las personas que utilizan React no utilizan web components, pero es posible que lo desee, especialmente si está utilizando componentes de interfaz de usuario de terceros escritos utilizando web components.

Ejemplo de uso de web components (Polymer). Primero creamos un componente.

```js
<polymer-element name="shop-home"  attributes="">
  <template>
    <style>
      @host { :scope {display: block;} }
    </style>
    <span>I'm <b>shop-home</b>. This is my Shadow DOM.</span>
  </template>
  <script>
    Polymer('shop-home', {
      //applyAuthorStyles: true,
      //resetStyleInheritance: true,
      created: function() { },
      enteredView: function() { },
      leftView: function() { },
      attributeChanged: function(attrName, oldVal, newVal) { }
    });
  </script>
</polymer-element>
```

añadiéndolo al documento actual, su uso sería

```js
<iron-pages role="main" selected="[[page]]" attr-for-selected="name" selected-attribute="visible">
  <!-- home view -->
  <shop-home name="home" categories="[[categories]]"></shop-home>
  <!-- list view of items in a category -->
  <shop-list name="list" route="[[subroute]]" offline="[[offline]]"></shop-list>
  <!-- detail view of one item -->
  <shop-detail name="detail" route="[[subroute]]" offline="[[offline]]"></shop-detail>
  <!-- cart view -->
  <shop-cart name="cart" cart="[[cart]]" total="[[total]]"></shop-cart>
  <!-- checkout view -->
  <shop-checkout name="checkout" cart="[[cart]]" total="[[total]]" route="{{subroute}}"></shop-checkout>
</iron-pages>
```

Este componente, creado con React sería

```js
import React, {PropTypes} from 'react';

export default class ShopHome extends React.Component {
  constructor(props) {
    super(props);
  }

  render() {
    return (<div>Shop Home Name: {this.props.name}</div>);
  }
}

ShopHome.propTypes = {
  name: PropTypes.string,
};
```

y para usarlo lo tendríamos que importar

```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './index.css';

import ShopHome from './shop-home.jsx';

ReactDOM.render(
  <App >
    <ShopHome name="Nombre de la tienda" />
  </App>,
  document.getElementById('root')
);
```