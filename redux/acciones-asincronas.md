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