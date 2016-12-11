# Sagas

Partamos de un ejemplo usando redux-thunk.

```js
function fetchData() {
  return function thunk(dispatch) {
    fetch('https://someurl.com/somendpoint', function callback(data) {
      dispatch(dataLoaded(data));
    });
  }
}
```

Esto tiene dos puntos difíciles: probar esas funciones es muy duro, y, conceptualmente, tener datos de búsqueda en las acciones no parece estar del todo muy bien.
Una gran ventaja de Redux es que los _action creators_ son fácilmente comprobables. Al devolver un thunk de una acción, de repente tenemos que llamar a la acción, hacer un mock de la función de envío, etc.

Redux-saga utiliza las funciones del generador Esnext para hacer que **el código asíncrono parezca sincrónico**, y hace que los flujos asíncronos sean muy fáciles de probar. Las sagas son como un hilo separado en la aplicación que maneja todas las cosas asíncronas, sin molestar al resto de la aplicación.

```js
/* sagas.js */

import {call, take, put} from 'redux-saga/effects';

// El * antes de function nos indica que es un generador
function * fetchData() {
    // yield indica que esperaremos. En este caso esperaremos
    // hasta que ocurra FETCH_DATA.
    yield take(FETCH_DATA);

    // Entonces obtenemos los datos del servidor y esperamos hasta 
    // que termine antes de continuar.
    var data = yield call(fetch, 'https://someurl.com/someendpoint');

    // Cuando ha terminado hacemos el dispatch de la accion dataLoaded.
    put(dataLoaded(data));
}
```

