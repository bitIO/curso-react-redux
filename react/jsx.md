# JSX, comprobación de tipos y contexto

Vamos a ir al revés.

## Contexto (que no se debería usar 😰)

Con React, es fácil ver el flujo de datos a través de sus componentes. Viendo un componente, se puede ver qué propiedades se le están pasando y, si este tiene componentes hijos, qué propiedades se le pasan a este.

En algunos casos, querremos pasar datos a través del árbol de componentes sin tener que pasar las propiedades hacia abajo *manualmente* en cada nivel. Esto se puede hacerlo directamente en React con la API de `context`. **Es una API experimental y es probable que se rompa en futuras versiones de React.**

Para usarlo tendremos que añadir las propiedades `childContextTypes` y `getChildContext` al proveedor de contexto y React pasará la información hacia abajo automáticamente a cualquier componente en el subárbol. Para que los componentes de menor nivel puedan acceder al contexto deben definir la propiedad `contextTypes`ya que si no está definida, el contexto será un objeto vacío.

Si en la declaración del contexto hacemos referencia al estado o las propiedades, cuando se produzca una actualización, dichos cambios se debrían pasar hacia abajo. Y digo deberían porque Facebook dice que la API para esto está, escencialmente, rota.

**<span style="color: red">Por lo tanto, aunque podemos usarlo, no debemos.</span>**

```js
class Button extends React.Component {
  render() {
    return (
      <button style={{background: this.context.color}}>
        {this.props.children}
      </button>
    );
  }
}

Button.contextTypes = {
  color: React.PropTypes.string
};

class Message extends React.Component {
  render() {
    return (
      <div>
        {this.props.text} <Button>Delete</Button>
      </div>
    );
  }
}

class MessageList extends React.Component {
  getChildContext() {
    return {color: "purple"};
  }

  render() {
    const children = this.props.messages.map((message) =>
      <Message text={message.text} />
    );
    return <div>{children}</div>;
  }
}

MessageList.childContextTypes = {
  color: React.PropTypes.string
};
```

```js
class MediaQuery extends React.Component {
  constructor(props) {
    super(props);
    this.state = {type:'desktop'};
  }

  getChildContext() {
    return {type: this.state.type};
  }

  componentDidMount() {
    const checkMediaQuery = () => {
      const type = window.matchMedia("(min-width: 1025px)").matches ? 'desktop' : 'mobile';
      if (type !== this.state.type) {
        this.setState({type});
      }
    };

    window.addEventListener('resize', checkMediaQuery);
    checkMediaQuery();
  }

  render() {
    return this.props.children;
  }
}

MediaQuery.childContextTypes = {
  type: React.PropTypes.string
};
```
## Comprobación de tipos

Ya hemos visto todo lo que podemos hacer con JSX. Ahora veremos como comprobar que los propiedades que nos llegan son del tipo que esperamos sin usar Flow ni TypeScript. Para ello usaremos `PropTypes`.

```js
MyComponent.propTypes = {
  // You can declare that a prop is a specific JS primitive. By default, these
  // are all optional.
  optionalArray: React.PropTypes.array,
  optionalBool: React.PropTypes.bool,
  optionalFunc: React.PropTypes.func,
  optionalNumber: React.PropTypes.number,
  optionalObject: React.PropTypes.object,
  optionalString: React.PropTypes.string,
  optionalSymbol: React.PropTypes.symbol,

  // Anything that can be rendered: numbers, strings, elements or an array
  // (or fragment) containing these types.
  optionalNode: React.PropTypes.node,

  // A React element.
  optionalElement: React.PropTypes.element,

  // You can also declare that a prop is an instance of a class. This uses
  // JS's instanceof operator.
  optionalMessage: React.PropTypes.instanceOf(Message),

  // You can ensure that your prop is limited to specific values by treating
  // it as an enum.
  optionalEnum: React.PropTypes.oneOf(['News', 'Photos']),

  // An object that could be one of many types
  optionalUnion: React.PropTypes.oneOfType([
    React.PropTypes.string,
    React.PropTypes.number,
    React.PropTypes.instanceOf(Message)
  ]),

  // An array of a certain type
  optionalArrayOf: React.PropTypes.arrayOf(React.PropTypes.number),

  // An object with property values of a certain type
  optionalObjectOf: React.PropTypes.objectOf(React.PropTypes.number),

  // An object taking on a particular shape
  optionalObjectWithShape: React.PropTypes.shape({
    color: React.PropTypes.string,
    fontSize: React.PropTypes.number
  }),

  // You can chain any of the above with `isRequired` to make sure a warning
  // is shown if the prop isn't provided.
  requiredFunc: React.PropTypes.func.isRequired,

  // A value of any data type
  requiredAny: React.PropTypes.any.isRequired,

  // You can also specify a custom validator. It should return an Error
  // object if the validation fails. Don't `console.warn` or throw, as this
  // won't work inside `oneOfType`.
  customProp: function(props, propName, componentName) {
    if (!/matchme/.test(props[propName])) {
      return new Error(
        'Invalid prop `' + propName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  },

  // You can also supply a custom validator to `arrayOf` and `objectOf`.
  // It should return an Error object if the validation fails. The validator
  // will be called for each key in the array or object. The first two
  // arguments of the validator are the array or object itself, and the
  // current item's key.
  customArrayProp: React.PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
    if (!/matchme/.test(propValue[key])) {
      return new Error(
        'Invalid prop `' + propFullName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  })
};
```

Con la comprobación de tipos también podemos requerir que nuestro componente sólo tenga un hijo.

```js
class MyComponent extends React.Component {
  render() {
    // This must be exactly one element or it will warn.
    const children = this.props.children;
    return (
      <div>
        {children}
      </div>
    );
  }
}

MyComponent.propTypes = {
  children: React.PropTypes.element.isRequired
};
```


Así mismo, podemos definir los valores predefinidos para las propiedades en caso de que estas no vengan. Es decir, se *resuelve* primero el `defaultProps` que `propTypes`. 

```js
class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

// Specifies the default values for props:
Greeting.defaultProps = {
  name: 'Stranger'
};

// Renders "Hello, Stranger":
ReactDOM.render(
  <Greeting />,
  document.getElementById('example')
);
```
## JSX 'avanzado'

**Usar nombres con puntuación**

Útil cuando tenemos un función que exporta más de un componente (aunque se puede hacer, **no lo hagas**)

```js
import React from 'react';

const MyComponents = {
  DatePicker: function DatePicker(props) {
    return <div>Imagine a {props.color} datepicker here.</div>;
  }
}

function BlueDatePicker() {
  return <MyComponents.DatePicker color="blue" />;
}
````

**Los componentes custom deben empezar por mayusculas**

De no hacerlo así, se pueden tomar por elementos estándar HTML

**Tipo de componente dinámico en tiempo de ejecución**

Es posible que, por algún extraño motivo queramos implementar la lógica de nuestro componente de esta manera. No es recomendable.

```js
import React from 'react';
import { PhotoStory, VideoStory } from './stories';

const components = {
  photo: PhotoStory,
  video: VideoStory
};

function Story(props) {
  // Correct! JSX type can be a capitalized variable.
  const SpecificStory = components[props.storyType];
  return <SpecificStory story={props.story} />;
}
```

**Pasando propiedades**

A un componente se le pueden pasar propiedades que sean cadenas o expresiones - `if` y `for`no son expresiones - siempre que estas estén contenidas dentro de llaves `{ }`.

```html
<MyComponent foo={1 + 2 + 3 + 4} />
<MyComponent message="hello world" />
<MyComponent message={'hello world'} />
<MyComponent message="&lt;3" />
<MyComponent message={'<3'} />
```

**`True` por defecto**

Si no le pasamos un valor a una propiedad, esta se convierte en `true`

```html
<MyTextBox autocomplete />
<MyTextBox autocomplete={true} />
```

**Spread**

```js
const props = {firstName: 'Ben', lastName: 'Hector'};
return <Greeting {...props} />;
```

**Hijos**

Los hijos de un componente pueden ser:

* Cadenas: Recuerda que JSX elimina los espacios al principio y al final de la linea. También lineas en blanco. Y retornos de carro se convierten en un espacio.
* JSX: Otros componentes
* Expresiones javascript
* Funciones

```js
function ListOfTenThings() {
  return (
    <Repeat numTimes={10}>
      {(index) => <div key={index}>This is item {index} in the list</div>}
    </Repeat>
  );
}

// Calls the children callback numTimes to produce a repeated component
function Repeat(props) {
  let items = [];
  for (let i = 0; i < props.numTimes; i++) {
    items.push(props.children(i));
  }
  return <div>{items}</div>;
}
```

Boleanos, null y undefined son ignorados.

**Renderizado condicional**

```html
<div>
  {showHeader && <Header />}
  <Content />
</div>
```

```html
<div>
  {props.messages.length > 0 &&
    <MessageList messages={props.messages} />
  }
</div>
```