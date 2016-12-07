## Nodos usando JSX

Al lio directamente 😎 JS vs JSX
```js
const reactNodeLi = React.createElement('li', {id:'li1'}, 'one');
const HTMLLi = <li className="bar">foo</li>;
```

JSX es una sintaxis similar a XML/HTML utilizada por React que extiende ECMAScript para que el HTML pueda coexistir con el código de JavaScript. **La sintaxis está destinada a ser utilizada por preprocesadores (por ejemplo, transpiladores como Babel) para transformar el texto similar al HTML que se encuentra en los archivos JavaScript en objetos JavaScript estándar que un motor de JavaScript analizará**.


> Básicamente, usando JSX puede escribir estructuras HTML en el mismo archivo que escribe código JavaScript, Babel transformará estas expresiones en código JavaScript real. A diferencia del pasado, en lugar de poner JavaScript en HTML, JSX nos permite poner HTML en JavaScript.

Mezclar HTML y JavaScript en el mismo archivo suele ser un tema bastante polémico.

JSX es más fácil de leer y escribir  que grandes bloquesde  JavaScript. Además, el equipo de React claramente cree que JSX es más adecuado para definir soluciones de interfaz de usuario que una solución tradicional de plantillas (por ejemplo, Handlebars):

```js
// código en JSX
const nav = (
  <ul id="nav">
    <li><a href="#">Inicio</a></li>
    <li><a href="#">Clientes</a></li>
    <li><a href="#">Cuentas</a></li>
    <li><a href="#">Ayuda</a></li>
  </ul>
);

// transpilado por Babel 😳
const nav = React.createElement(
   "ul", { id: "nav" },
   React.createElement("li", null, React.createElement("a", { href: "#" }, "Inicio")),
   React.createElement("li", null, React.createElement("a", { href: "#" }, "Clientes")),
   React.createElement("li", null, React.createElement("a", { href: "#" }, "Cuentas")),
   React.createElement("li", null, React.createElement("a", { href: "#" }, "Ayuda"))
);
```

No pienses en JSX como una plantilla, sino como una sintaxis JS especial que tiene que ser compilada. Para ello, usamos Babel - que es una [selección subjetiva del equipo React](https://facebook.github.io/react/blog/2015/09/10/react-v0.14-rc1.html#compiler-optimizations) para transformar el código ES\* y la sintaxis JSX al código ES5.

Por qué Facebook cree que JSX merece la pena:
* Las personas menos técnicas todavía pueden entender y modificar las partes requeridas. Los desarrolladores y diseñadores de CSS encontrarán JSX más familiar que JavaScript puro. 
* Puede aprovechar toda la potencia de JavaScript en HTML y evitar el aprendizaje o el uso de un lenguaje de plantillas. JSX no es una solución de plantillas. Es una sintaxis declarativa utilizada para expresar una estructura de árbol de nodos y componentes de interfaz de usuario.
* Al agregar un paso de transformación de JSX, encontrarás errores en tu HTML que de otra forma te perderías.
* JSX promueve la idea de estilos en línea. Que puede ser una buena cosa (o no).
* JSX es algo separado de React que no intenta cumplir con ninguna especificación XML o HTML. JSX está diseñado como una característica ECMAScript y la similitud con XML/HTML es sólo superficial (es decir, se ve como XML/HTML para que pueda escribir algo familiar). 
* Actualmente [se está escribiendo una especificación de JSX](https://facebook.github.io/jsx/) para ser utilizada por cualquiera como una extensión de sintaxis similar a XML a ECMAScript sin ninguna semántica definida.

Recuerda que en JSX, `<div />` es válido mientras que `<div>` solo no lo es. **Tienes que cerrar todas las etiquetas, siempre**.

Más sobre Babel en [http://babeljs.io/](http://babeljs.io/) o en el [manual de Babel](https://github.com/thejameskyle/babel-handbook/blob/master/translations/en/user-handbook.md).

**Recordatorio**

* Las etiquetas JSX admiten la sintaxis de cierre automático de XML, por lo que, opcionalmente, se puede dejar la etiqueta de cierre desactivada.
* Si pasamos atributos a elementos HTML nativos que no existen en la especificación HTML, React no los renderizará al DOM actual. Sin embargo, si utiliza un elemento personalizado (es decir, no un elemento HTML de soporte), los atributos personalizados se agregarán a elementos personalizados (<drago-component custom-attribute = "foo" />).
* El atributo de `class` debe escribirse `className`
* El atributo `for` debe escribirse `htmlFor`
* El atributo de `style` toma un objeto de propiedades - en camel case.
* Todos los atributos son camel case (por ejemplo, accept-charset se escribe como acceptCharset)
* Para representar elementos HTML, asegúrese de que la etiqueta HTML este en _lowercase_

### Atributos

En comparación con cómo lo hacíasmos en JS

```js
const reactNodeLi = React.createElement('li',
{
    foo:'bar',
    id:'li1',
    // no se puede usar class por ser una palabra reservada por
    // lo que se usa className para el atributo class del nodo
    className:'blue',
    'data-test':'test',
    'aria-test':'test',
    // las propiedades de estilo se pasarán dentro de un objeto
    // y en formato como camel case
    style:{backgroundColor:'red'}
},
'texto'
);
```
que producía este html
 
 ```html
 <li id="li1" data-test="test" class="blue" aria-test="test" style="background-color:red;" data-reactid=".0">texto</li>
 ```
 
 En JSX lo definiríamos de la siguiente manera
 
 ```js
const reactNodeLi = 
  <li ariaTest="test" className="blue" dataTest="test" style={{ backgroundColor: 'red' }}>
    texto
  </li>
 ```
 
 y, una vez transpilado, se convertiría al JS que ya conocemos
 
 ```js
 var reactNodeLi = React.createElement(
    'li',
    { 
      'data-test': 'test',
      className: 'blue',
      'aria-test': 'test',
      style: {
          backgroundColor: 'red'
      }
    },
    text
);
```

### Eventos en JSX

Pocas diferencias con como lo hacíamos en JS

```js
const mouseOverHandler = () => {
  console.log('estás encima de mi!');
};
const clickhandler = () => {
  console.log('me pinchas y no sangro');
};
var reactNode = (
  <div onClick={clickHandler} onMouseOver={mouseOverHandler} >
    click y mouseover
  </div>
);

ReactDOM.render(reactNode, document.getElementById('app'));
```

Cabe destacar que usamos `{ }` para conectar las funciones JS a un evento.

**Pro tips:**

* Los eventos en React se activan en la fase de _bubbling_. Para activar un evento en la fase de captura, hay que añadir la palabra _Capture_ al nombre del evento (por ejemplo, `onClick` se convertirá en `onClickCapture`).
* No todos los eventos DOM son proporcionados por React. Pero se puede hacer uso de ellos utilizando los métodos de ciclo de vida de React.
* 