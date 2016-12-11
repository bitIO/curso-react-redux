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

Ahora vamos a añadir una ruta básica. Para ello, necesitamos declarar el contenedor de rutas y la ruta:

```js
<Router history={hashHistory}>
  <Route path='/' component={Home} />
  <Route path='/address' component={Address} />
</Router>
```


[https://github.com/reactjs/react-router-tutorial](https://github.com/reactjs/react-router-tutorial)

