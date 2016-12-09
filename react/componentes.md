# Componentes
Los componentes permiten dividir la interfaz de usuario en piezas independientes y reutilizables y pensar en cada pieza de forma aislada.

Conceptualmente, los componentes son como las funciones de JavaScript. Aceptan entradas arbitrarias (_props_) y devuelven elementos de React que describen lo que debería aparecer en la pantalla.

## Componentes Funcionales y Componentes Clase

Hay dos formas básicas de crear componentes.

### Componentes funcionales

```js
const Welcome = (props) => {
  return <h1>Hello, {props.name}</h1>;
}
```

### Componentes clase

```js
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

## Composición

Los componentes pueden referirse a otros componentes. Esto nos permite usar la misma abstracción de componentes para cualquier nivel de detalle. Un botón, un formulario, un cuadro de diálogo, una pantalla.

Por ejemplo, podemos crear un componente App que escriba los nombres de los asistentes al curso:


```js
function Alumno(props) {
  return <div><strong>Nombre:</strong> {props.name}</div>;
}

function App() {
  return (
    <div>
      <Alumno name="Francisco" />
      <Alumno name="Sandra" />
      <Alumno name="Héctor" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('app')
);
```

Por lo general, las aplicaciones React tienen un componente de aplicación único. Sin embargo, si integramos React en una aplicación existente, deberemos comenzar de abajo hacia arriba con un pequeño componente como `Button` y gradualmente avanzar hasta la parte superior de la jerarquía del arbol sea nuestro componente `App`.

**Nota:**
Los componentes **deben devolver un solo elemento** raíz. Esta es la razón por la que hay un `<div>` para contener todos los elementos `<Alumno />`

### Ejercicio

Convertir en componentes el siguiente código:
```js
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

Solución:

```js
const formatDate = (date) => {
  return date.toLocaleDateString();
}
const Avatar = (props) => {
  return (
    <img className="Avatar"
         src={props.user.avatarUrl}
         alt={props.user.name} />
  );
}
const UserInfo = (props) => {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}

const Comment = (props) => {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}

const comment = {
  date: new Date(),
  text: 'Los gatitos siempre molan en el front!',
  author: {
    name: 'Hello Kitty',
    avatarUrl: 'http://placekitten.com/g/64/64'
  }
};

ReactDOM.render(
  <Comment
    date={comment.date}
    text={comment.text}
    author={comment.author} />,
  document.getElementById('root')
);
```