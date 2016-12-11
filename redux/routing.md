# Routing con react-router-redux

Ya hemos visto en el capítulo de React cómo usar el router. Ahora lo combinaremos con Redux.

Esta biblioteca ayuda a mantener ese estado de sincronización con el store. Mantiene una copia de la ubicación actual oculta en estado. Al _rebobinar_ el estado de aplicación - con una herramienta como Redux DevTools - ese cambio de estado se propaga a react-router para que pueda ajustar el árbol de componentes en consecuencia. Puede saltar por el estado, rebobinar, reproducir y restablecer tanto como desee, y esta biblioteca garantizará que los dos permanezcan sincronizados en todo momento.

```js
import React from 'react'
import ReactDOM from 'react-dom'
import { createStore, combineReducers } from 'redux'
import { Provider } from 'react-redux'
import { Router, Route, browserHistory } from 'react-router'
import { syncHistoryWithStore, routerReducer } from 'react-router-redux'

import reducers from '<project-path>/reducers'

// Add the reducer to your store on the `routing` key
const store = createStore(
  combineReducers({
    ...reducers,
    routing: routerReducer
  })
)

// Create an enhanced history that syncs navigation events with the store
const history = syncHistoryWithStore(browserHistory, store)

ReactDOM.render(
  <Provider store={store}>
    { /* Tell the Router to use our enhanced history */ }
    <Router history={history}>
      <Route path="/" component={App}>
        <Route path="foo" component={Foo}/>
        <Route path="bar" component={Bar}/>
      </Route>
    </Router>
  </Provider>,
  document.getElementById('mount')
)
```

##### ¿Cómo puedo observar los eventos de navegación - para analíticas , por ejemplo?

```js
const history = syncHistoryWithStore(browserHistory, store)
history.listen(location => analyticsService.track(location.pathname))
```