# Routing

Lo que dicen en la documentación

> React Router keeps your UI in sync with the URL. It has a simple API with powerful features like lazy code loading, dynamic route matching, and location transition handling built right in. Make the URL your first thought, not an after-thought.

Creamos una apliación nueva y, cuando acabe de crearse ⌛️, instalamos el router.

```npm i -S react-router```

e importamos en nuestro App.js

```js
import { 
  Router, Route, Link, IndexRoute, hashHistory, browserHistory 
} from 'react-router';
```

Ahora vamos a añadir una ruta básica. Para ello, necesitamos declarar el contenedor de rutas y la ruta junto con los componentes asociados:

```js
const Oficina = () => <h1>Esta es una oficina</h1>
const Direccion = () => <h1>Esta es la dirección</h1>

class App extends Component {
  render() {
    return (
      <Router history={hashHistory}>
        <Route path='/' component={Oficina} />
        <Route path='/direccion' component={Direccion} />
      </Router>
    )
  }
}
```

Estamos usando `hashHistory` para el historial del navegación. Maneja el historial con la parte hash de la url. Añade ese extra para simular algunos comportamientos que el navegador tiene de forma nativa al usar urls reales. 
Podemos cambiarlo por `browserHistory`. Cuando se utiliza el browserHistory debemos tener un servidor que siempre devolverá la aplicación en cualquier ruta; por ejemplo si usamos nodejs, una configuración como la siguiente funcionaría:

```js
const express = require('express')
const path = require('path')
const port = process.env.PORT || 8080
const app = express()

// serve static assets normally
app.use(express.static(__dirname + '/public'))

// handle every other route with index.html, which will contain
// a script tag to your application's JavaScript file(s).
app.get('*', function (request, response){
  response.sendFile(path.resolve(__dirname, 'public', 'index.html'))
})

app.listen(port)
```


[https://github.com/reactjs/react-router-tutorial](https://github.com/reactjs/react-router-tutorial)

