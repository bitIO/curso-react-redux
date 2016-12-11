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

#### Fetch data en el router no el `componentWillMount`

Habitualmente hacemos el _fecth_ de los datos cuando sabemos que el compoente se va a inicializar. ¿Qué pasa si se instancia ese componente 5 veces?

Para evitar este tipo de situaciones, podemos usar el hook [onEnter de react-router](https://github.com/ReactTraining/react-router/blob/master/docs/guides/RouteConfiguration.md#enter-and-leave-hooks) para hacer el dispatch aquí. De esta manera, el componente se acerca más a un componente de presentación puro (stateless).

#### Flujos de datos erróneos con Redux

Es posible que lleguemos a usar alguna api que nos provea de datos y que caigamos en el error de _meter dichos datos_ directamente en nuestro estado en react o queramos meterlos al store. Pero debemos recordar que Redux debe ser nuestro SSOT.

Por ejemplo, si usamos Firebase como fuente de datos, podemos usar `react-fire`. Pero este paquete va en contra de los flujos de datos en redux.

![](/assets/Bad data flow.png)

