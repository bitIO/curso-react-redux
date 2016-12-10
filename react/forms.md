# Componetes con y sin controls

En HTML, elementos de formulario como `<input>`, `<textarea>` y `<select>` normalmente mantienen su propio estado y lo actualizan en función de la entrada del usuario. En React, el estado mutable normalmente se mantiene en la propiedad `state` de los componentes y sólo se actualiza con `setState()`.

Podemos combinar los dos haciendo que el estado de React sea la "[fuente única de la verdad](https://en.wikipedia.org/wiki/Single_source_of_truth)". El componente React que procesa un formulario también controla lo que sucede en ese formulario en la entrada de usuario posterior. Un elemento de formulario de entrada cuyo valor es controlado por React de esta manera se denomina *"componente controlado"*.

```js
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

Dado que el atributo valor se establece en nuestro elemento de formulario, el valor mostrado siempre será `this.state.value`, haciendo que el estado React sea la fuente de la verdad. `handleChange` **se ejecuta en cada pulsación de tecla para actualizar el estado React** por lo que el valor mostrado se actualizará.

Con un componente controlado, cada mutación de estado tendrá una función de controlador asociada. Esto hace que sea sencillo modificar o validar la entrada del usuario.

**Ejercicios:** 

* Convertirlo en un componente sin control.
* Cambiar el input por un textarea
* Añadir un componente controlado de tipo select

## Componentes no controlados

Cuando estamos adaptando una aplicación existente, puede ser muy tedioso convertir todos los controles existentes a componentes controlados.

En estos casos, haremos uso de la propiedad `ref` para gestionar dichos componentes. React admite este atributo especial que puede adjuntar a cualquier componente. El atributo `ref` recibe un *callback* que se ejecutará inmediatamente después de montar o desmontar el componente. 

Cuando se utiliza el atributo ref en un elemento HTML, el argumento del callback es el elemento DOM subyacente. Cuando se desmonta el componente, el callback será llamado con `null`.

```js
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);
    this.focus = this.focus.bind(this);
  }

  focus() {
    // Explicitly focus the text input using the raw DOM API
    this.textInput.focus();
  }

  render() {
    // Use the `ref` callback to store a reference to the text input DOM
    // element in this.textInput.
    return (
      <div>
        <input
          type="text"
          ref={(input) => { this.textInput = input; }} />
        <input
          type="button"
          value="Focus the text input"
          onClick={this.focus}
        />
      </div>
    );
  }
}
```

Cuando se utiliza el atributo ref en un componente personalizado, el argumento es la instancia montada del componente. No puede utilizar el atributo ref en componentes funcionales porque no tienen instancias. Sin embargo, puede utilizar el atributo ref dentro de la función render de un componente funcional.

```js
class AutoFocusTextInput extends React.Component {
  componentDidMount() {
    this.textInput.focus();
  }

  render() {
    return (
      <CustomTextInput
        ref={(input) => { this.textInput = input; }} />
    );
  }
}
```

```js
function CustomTextInput(props) {
  // textInput must be declared here so the 
  // ref callback can refer to it
  let textInput = null;

  function handleClick() {
    textInput.focus();
  }

  return (
    <div>
      <input
        type="text"
        ref={(input) => { textInput = input; }} />
      <input
        type="button"
        value="Focus the text input"
        onClick={handleClick}
      />
    </div>
  );  
}
```

Finalmente, el componente no controlado quedaria así

```js
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.input.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" ref={(input) => this.input = input} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

**Ejercicio:** Propon una solución para que este proceso de adaptación de un código existente sean menos tedioso.