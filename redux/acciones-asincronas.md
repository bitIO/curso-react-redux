# Acciones asíncronas
`
Ya hemos visto como usar middlewares para realizar acciones síncronas. Ahora toca el otro pilar de los middlewares, las acciones asíncronas.

## Crear una aplicacion nueva

Creamos una aplicación nueva con `create-react-app` y eliminamos todo el código de App.js, App.css, index.css, logo.svg y eliminamos sus referencias.

Importamos los middleware `redux-logger`, `redux-thunk` y la libreria para peticiones asíncronas `axios`.

Así debería quedar nuestro App.js nuevo inicial.

```js
import React from 'react';
import { applyMiddleware, createStore } from 'redux';

import axios from 'axios';
import logger from 'redux-logger';
import thunk from 'redux-thunk';

const reducer = (state = {}, action) => {
  return state;
};

const middleware = applyMiddleware(thunk, logger());
const store = createStore(reducer, middleware);

store.subscribe(() => {
  console.log('Store has changed', store.getState());
});

store.dispatch({ type: 'FOO' });

class App extends React.Component {
  render() {
    return (
      <div className="App">
        Boilerplaite app
      </div>
    );
  }
}

export default App;
```

### Modificar dispatch usando _thunk_

El middleware _redux-thunk_ nos permite escribir creadores de acciones que devuelven una función en lugar de una acción. El _thunk_ se puede utilizar para retrasar el envío de una acción, o para enviar sólo si se cumple una cierta condición. La función interna recibe los métodos de almacén dispatch y getState como parámetros.

```js
store.dispatch((dispatch) => {
  dispatch({ type: 'POSTS_FETCH_START' });
  axios
    .get('https://jsonplaceholder.typicode.com/posts')
    .then((response) => {
      dispatch({ type: 'POSTS_FETCH_FINISHED', payload: response.data });
    })
    .catch((error) => {
      dispatch({ type: 'POSTS_FETCH_FAILED', payload: error });
    });
});
```

### Modificar el reducer

Puesto que ahora tenemos estados dinámicos, necesitamos inicilizar el estado, ya no puede ser un objeto vacio.

```js
const initialState = {
  fetching: false,
  fetched: false,
  posts: [],
  error: null,
};
```

Y modificaremos el reducer para que atienda las nuevas acciones

```js
const reducer = (state = initialState, action) => {
  switch (action.type) {
    case 'POSTS_FETCH_START':
      return {
        ...state,
        fetching: true,
      };
    case 'POSTS_FETCH_FINISHED':
      return {
        ...state,
        fetching: false,
        fetched: true,
        posts: action.payload,
      };
    case 'POSTS_FETCH_FAILED':
      return {
        ...state,
        fetching: false,
        error: action.payload,
      };
    default:
      return state;
  }
};
```
