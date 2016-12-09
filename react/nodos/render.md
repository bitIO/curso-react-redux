# Renderizando los nodos en el DOM

React proporciona la función `ReactDOM.render()` de `react-dom.js` que se utiliza para procesar los nodos React en el DOM.

Vamos a crear dos elementos, uno que representa un tag HTMl existente y uno no existente, y los renderizamos en el HTML existente.

```js
var HTMLLi = React.createElement('li', {className:'bar'}, 'foo');
var HTMLCustom = React.createElement('foo-bar', {className:'bar'}, 'foo');
ReactDOM.render(HTMLLi, document.getElementById('existente'));
ReactDOM.render(HTMLCustom, document.getElementById('custom'));
```
```html
<body>
    <div id="existente"></div>
    <div id="custom"></div>
</body>
```
El resultado sería

```html
<body>
    <div id="existente">
      <li class="bar" data-reactid=".0">foo</li>
    </div>
    <div id="custom">
      <foo-bar classname="bar" children="foo" data-reactid=".1">
        foo
      </foo-bar>
    </div>
</body>
```

Para renderizarlos usando JSX, el proceso sería bastante similar. La diferencia es que Babel se encargará de hacer la tranpilación del JSX a `React.createElement()`.

```js
var HtmlLi = <li className="bar">foo</li>;
var HtmlCustom = <foo-bar className="bar" >foo</foo-bar>;

ReactDOM.render(HtmlLi, document.getElementById('lista'));
ReactDOM.render(HTMLCustom, document.getElementById('contenedor'))
```

```html
<body>
  <ul id="lista"></ul>
  <div id="contenedor"></div>
</body>
```

## Actualización del elemento
Los elementos de React son inmutables, es decir, que una vez que creemos un elemento, no se podrán cambiar ni sus hijos ni sus atributos. Un elemento es como un solo fotograma en una película: representa la interfaz de usuario en un determinado momento.

Con nuestro conocimiento hasta ahora, la única manera de actualizar la interfaz de usuario es crear un nuevo elemento y pasarlo a `ReactDOM.render()`

```js
función tick () {
   const element = (
     <div>
       <h1> Hola, mundo! </ h1>
       <h2> Es {new Date().toLocaleTimeString()}.</h2>
     </ div>
   );
   ReactDOM.render (
     element,
     document.getElementById('root')
   );
}

setInterval (tick, 1000);
```

Si abrimos el inspector de código veremos que React sólo realiza las actualizaciones de lo que es necesario. React DOM compara el elemento y sus hijos con el anterior, y sólo aplica las actualizaciones DOM necesarias para llevar el DOM al estado deseado.