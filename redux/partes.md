# Componentes de Redux

### Actions

**El estado de nuestra aplicación sólo cambiará si ejecutamos un action**. Un action es un simple objeto plano de javascript que describe un cambio. Así como el state es la mínima representación de los datos de nuestra app, el action es la mínima representación del cambio en los datos.

Cualquier action deberá tener siempre una propiedad type, cuyo valor sea diferente a undefined. Cada app, tendrá definidas sus propios actions para describir los cambios en el estado de sus datos.

### Reducer

El reducer es la función que tiene toda aplicación de redux, con la que se **entrega siempre un nuevo estado** de los datos, pasando como parámetros el estado previo y la acción a ejecutar.

Siempre que pasásemos como tipo de acción uno que no atañe al reducer, devolveremos el mismo estado que llegó.

```js
const myReducer = (state, action) => {
  switch(action.type) {
    case 'hazAlgo':
      // lógica que lo que tengamos que hacer
      return {
        proiedad: nuevoValor,
        ... state
      }
    default:
      return state;
  }
}
```

### Store

El store contiene los 3 principios de redux ya que será el centro de nuestra aplicación permitiendo conoceremos el estado actual de la aplicación y nos permitirá ejecutar acciones que modifiquen nuestro estado.  
Para poder crear un store, necesitamos pasarle un _reducer_ con la lógica de los cambios. El reducer será la única información que necesitaremos que Redux conozca de nuestra aplicación. El resto, lo crearemos nosotros.

#### Métodos del store.
El store tiene 3 métodos importantes.

* getState() : Devuelve el state actual del store.
* dispatch() : Dispara actions para cambiar el state.
* subscribe(): Es un callback que será llamado con cada dispacth, haciendo que tengamos disponible y actualizado el método getState.

```js
import React from 'react';
import ReactDOM from 'react-dom';
import { createStore } from 'redux';

const myReducer = (state = 0, action) => {
  switch(action.type) {
    case 'sumar':
      return state + 1;
    case 'restar':
      return state - 1;
    default:
      return state;
  }
}

// Inicializamos el store pasándole el reducer
const store = Redux.createStore(myReducer);

// Creamos nuestro componente de react
const Counter = ({value, clickSumar, clickRestar}) => (
  <div>
    <h1>{value}</h1>
    <button onClick={clickSumar}>Sumar</button>
    <button onClick={clickRestar}>Restar</button>
  </div>
);

// Renderizamos el componente.
// Cada vez que el stado cambie, el componente
// se volverá a renderizar.
const render = () => {
  ReactDOM.render(
    <Counter 
      value={store.getState()}
      clickSumar={ ()=> store.dispatch({type: 'sumar'}) }
      clickRestar={ ()=> store.dispatch({type: 'restar'}) }
    />,
    document.getElementById('root')
  );
};

store.subscribe(render);
render();
```
https://jsfiddle.net/kne7ztnu/1/
