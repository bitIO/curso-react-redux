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

    // Entonces obtenemos los datos del servidor 
    // y esperamos hasta  que termine antes de continuar.
    var data = yield call(fetch, 'https://someurl.com/someendpoint');

    // Cuando ha terminado hacemos 
    // el dispatch de la accion dataLoaded.
    put(dataLoaded(data));
}
```

El código de arriba casi se lee como una novela, evita el _callback hell_ y, además de eso, es fácil de probar. 

Y ahor preguntas **¿por qué es fácil de probar?** La respuesta tiene que ver con nuestra capacidad de probar los "efectos" que redux-saga exporta sin necesidad de completarlos.

Estos efectos - que importamos de redux-saga/effects - son _handlers_ que nos permiten interactuar fácilmente con nuestro código redux:

* put() - hace disptach una acción de nuestra saga.
* take() - detiene nuestra saga hasta que una acción sucede en nuestra aplicación.
* select() - obtiene una parte del estado redux (como mapStateToProps).
* call() - llama a la función pasada como el primer argumento con los argumentos restantes.

Veamos como probarlo

```js
/* sagas.test.js */

var sagaGenerator = fetchData();

describe('fetchData saga', function() {
  // Test that our saga starts when an action is dispatched,
  // without having to simulate that the dispatch actually happened!
  it('should wait for the FETCH_DATA action', function() {
    expect(sagaGenerator.next()).to.equal(take(FETCH_DATA));
  });

  // Test that our saga calls fetch with a specific URL,
  // without having to mock fetch or use the API or 
  // be connected to a network!
  it('should fetch the data from the server', function() {
    expect(sagaGenerator.next()).to.equal(
      call(fetch, 'https://someurl.com/someendpoint')
    );
  });

  // Test that our saga dispatches an action,
  // without having to have the main application running!
  it('should dispatch the dataLoaded action when the data has loaded', function() {
    expect(sagaGenerator.next()).to.equal(put(dataLoaded()));
  });
});
```

Recuerda que los generadores de Esnext no pasan la palabra clave yield hasta que se llama a `generator.next()`, momento en el que ejecutan la función, hasta que encuentran la siguiente palabra clave yield. Mediante el uso de los efectos redux-saga, podemos probar fácilmente cosas asíncronas sin necesidad de mockear nada y sin necesidad de red para nuestras pruebas.

[Veamos el ejemplo de la documentación](https://github.com/yelouafi/redux-saga/)

[Tutorial completo de yelouafi](https://yelouafi.github.io/redux-saga/docs/introduction/BeginnerTutorial.html)