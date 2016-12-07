#¿Qué son los nodos de React?

El tipo o valor principal que se crea al utilizar React es lo que se conoce como **nodo React**. Un nodo React se define como:

> Una representación virtual ligera, sin estado, inmutable, de un nodo DOM.

Por lo tanto, los nodos React no son nodos DOM reales (por ejemplo, nodos de texto o de elementos), sino una representación de **un nodo DOM potencial**. El el llamado DOM virtual. En pocas palabras, React se utiliza para definir un DOM virtual utilizando nodos React, que alimentan los componentes de React, que se pueden utilizar para crear un DOM real estructurado o de otro tipo (por ejemplo, React Native). **Los nodos React pueden crearse usando JSX o JavaScript.** La mayor parte de los desarrolladores usan JSX para la creación de nodos.

## Renderizando los nodos en el DOM

React proporciona la función `ReactDOM.render()` de `react-dom.js` que se utiliza para procesar los nodos React en el DOM.

Vamos a crear dos elementos, uno que representa un tag HTMl existente y uno no existente, y los renderizamos en el HTML existente.

```js
var HTMLLi = React.createElement('li', {className:'bar'}, 'foo');
var HTMLCustom = React.createElement('foo-bar', {className:'bar'}, 'foo');
ReactDOM.render(HTMLLi, document.getElementById('existente'));
ReactDOM.render(HTMLCustom, document.getElementById('custom'));
```

El resultado sería

```html
<body>
    <div id="existente"><li class="bar" data-reactid=".0">foo</li></div>
    <div id="custom"><foo-bar classname="bar" children="foo" data-reactid=".1">foo</foo-bar></div>
</body>
```