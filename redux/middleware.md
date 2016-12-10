# Middleware

El middleware Redux no es más que una función que se invoca después de que se envía una acción, pero antes de que un reductor se encargue de la acción. La gente utiliza el middleware para logging, informes de fallos, conversación con una API asíncrona, enrutamiento y más. Es algo similar a la programación de aspecto.

## Cómo funciona

No creo que haya una necesidad de reinventar la rueda así que vamos a usar el ejemplo que se ofrece en la documentación de Redux:

```js
const logger = store => next => action => {
  console.log('dispatching', action);
  let result = next(action);
  console.log('next state', store.getState());
  return result;
}
 
export default logger;
```

Esto es realmente una cascada de funciones de flecha y podría ser reescrito así en ES5:

```js
function logger(store) {
  return function wrapDispatch(next) {
    return function dispatch(action) {
      ...
    }
  }
}
```

Este es un concepto bien conocido de la programación funcional y se llama ["currying"](https://www.youtube.com/watch?v=iZLP4qOwY8I).

Por lo tanto, la función `logger` es en realidad nuestro middleware. Recibe el _store_ que es inyectado por la función `applyMiddleware (que se explicará esto más adelante).
El `logger` devuelve otra función que recibe la siguiente función como parámetro. Esta función nos permite enviar la siguiente acción.
Y la última, la más interna, recibe el objeto de la acción como parámetro.

## Veamos un ejemplo 



