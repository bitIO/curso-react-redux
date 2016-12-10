# Introducción

En su site oficial, se define como _a predictable state container for JavaScript apps_. Lo cual se podría traducir como contenedor de estados predecibles para aplicaciones javascript.

## Los 3 principios de Redux:

Redux basa su funcionamiento en 3 principios clave:

1. **Una sola fuente de datos \(SSOT\)**: El estado de la aplicación se almacena en un árbol de objetos dentro de una única _STORE_. Esto hace que sea más fácil depurar una aplicación y sea más rápido su desarrollo.
2. **El estado es sólo de lectura**: La única manera que tenemos de cambiar el estado será mediante _ACTIONS_. Así nos aseguramos que la vista nunca modifica el estado, sino que expresa su intención de mutar.
3. **Las mutaciones se escriben como funciones puras**: Para especificar como cambiará el estado por las acciones, se utilizan los _REDUCERS_, que son funciones puras a las que pasamos el estado anterior y la acción a realizar devolviendo un nuevo estado de la aplicación, en lugar de modificar el estado anterior.

Otro dibujito para comparar con lo anterior

![](/assets/redux-paper.png)

### Actions

**El estado de nuestra aplicación sólo cambiará si ejecutamos un action**. Un action es un simple objeto plano de javascript que describe un cambio. Así como el state es la mínima representación de los datos de nuestra app, el action es la mínima representación del cambio en los datos.
  
Cualquier action deberá tener siempre una propiedad type, cuyo valor sea diferente a undefined. Cada app, tendrá definidas sus propios actions para describir los cambios en el estado de sus datos.


### Reducer

El reducer es la función que tiene toda aplicación de redux, con la que se **entrega siempre un nuevo estado** de los datos, pasando como parámetros el estado previo y la accion a ejecutar.

Siempre que pasásemos como tipo de acción uno que no atañe al reducer, devolveremos el mismo estado que llegó.


```js
const myReducer = (state, action) => {
  switch(action.type) {
    case 'sumar':
      return state + 1;
    case 'restar':
      return state - 1;
    default:
      return state;
  }
}
```


