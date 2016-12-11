# Errores comunes

#### Uso de un elemento que no está disponible
`React.createElement: type should not be null, undefined, boolean, or number`

Básicamente nos está diciendo que estamos intentando usar un componente al que no se tiene acceso. Esto puede ser porque no se ha importado, no se ha exportado o la ruta no es correcta.

#### Importar de manera incorrecta

Cuando hacemos un import en un _reducer_, por ejemplo y hemos definido las constantes de los tipos de acciones y las importamos, **ojo con olvidar las llaves { }**

```js
import FETCH_IMAGES form '../action/types';

export default function(state = [], action) {
  switch (action.type) {
    case FETCH_IMAGES:
     return action.payload;
    default:
     return state;
  }
}
```

#### Render de JS objects en lugar de strings

`Uncaught Error: Invariant Violation: Objects are not valid as a React child`

En antiguas versiones esto se permitia, ahora tenemos que pasar un string

```js
<p>{{ color:blue, highlight: true }}</p>
```