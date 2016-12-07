# Renderizando los nodos en el DOM

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