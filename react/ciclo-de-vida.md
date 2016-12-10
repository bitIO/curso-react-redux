# Ciclo de vida

Cada componente tiene varios "métodos de ciclo de vida" que se puede sobreescribir para realizar acciones en momentos concretos del proceso. Los métodos prefijados con `will` se llaman justo antes de que algo suceda, y los métodos prefijados con `did` se llaman justo después de que algo sucede.

## Montaje
Estos métodos se llaman cuando se crea una instancia de un componente y se inserta en el DOM:

### componentWillMount()
Se invoca inmediatamente antes de que se produzca el montaje. Se llama antes del `render()`, por lo tanto, es un momento ideal para setear el `state` ya que no desencadena una nueva renderización. 

Este es el único *hook* del ciclo de vida que se utiliza cuando se hace renderización el servidor. Por lo general, Facebook recomienda utilizar el constructor en lugar de este método.
### componentDidMount()
Se invoca inmediatamente después de montar un componente. La inicialización que requiere nodos DOM debe ir aquí. Si necesitamos cargar datos desde un *endpoint*, este es un buen lugar para instanciar la solicitud de red. Si cambiamos el estado en este método seproducirá una nueva renderización.

## Actualización
Una actualización puede ser causada por cambios en las propiedades o estado. Estos métodos se llaman cuando un componente se vuelve a renderizar:

### componentWillReceiveProps(nextProps)
Se invoca antes de que un componente montado reciba nuevas propiedades. Si necesitmos actualizar el estado en respuesta a cambios de las props (por ejemplo, para restablecerlo), puede comparar `this.props` y `nextProps` y realizar transiciones de estado utilizando `this.setState()` en este método. 

React puede llamar a este método incluso si las propiedades no han cambiado, así que aseguraos de comparar los valores actuales y siguientes para mejorar el rendimiento. `componentWillReceiveProps()` no se invoca si simplemente llama `this.setState`

### shouldComponentUpdate(nextProps, nextState)

Se usará para que React sepa si la salida de un componente no se ve afectada por el cambio actual en el estado o las propiedades. El comportamiento predeterminado es volver a renderizar en cada cambio de estado y, en la gran mayoría de los casos, esto será lo que tenemos que hacer. `shouldComponentUpdate()` se invoca antes de renderizar cuando se reciben un nuevo estado o propiedades. El valor predeterminado es `true`- Este método no se llama para el render inicial o cuando se utiliza `forceUpdate()`.

Devolver false no impide que los componentes secundarios vuelvan a renderizar cuando su estado cambie. Si devuelve `false`, entonces `componentWillUpdate`, `render` y `componentDidUpdate` no serán invocados. **Tened en cuenta que en el futuro React puede tratar `shouldComponentUpdate` como una sugerencia en lugar de una directiva estricta, y devolver false puede resultar en una nueva renderización del componente.**

Si despues de hacer un *profiling* determinamos que un componente es lento , podemos cambiarlo para que hereda de `React.PureComponent` que implementa `shouldComponentUpdate` con una comparación de propiedades y estado superficial.

### componentWillUpdate(nextProps, nextState)

Se invoca inmediatamente antes de la renderización cuando se reciben nuevas propiedades o estado. Usaremos este método como una oportunidad para realizar la preparación antes de que se produzca una actualización. Este método no se llama para el render inicial.

No se puede llamar a `this.setState()` aquí. Si necesitamos actualizar el estado en respuesta a un cambio de props, utilizaremos `componentWillReceiveProps`.

### componentDidUpdate(prevProps, prevState)

Se invoca inmediatamente después del render. Este método no se llama para el render inicial.

Se usa, por ejemplo, para operar en el DOM cuando el componente se ha actualizado. Esto también es un buen lugar para hacer peticiones de red (por ejemplo, una petición de red no será necesaria si las propiedades no han cambiado).

## Desmontaje
Este método se llama cuando se quita un componente del DOM:

### componentWillUnmount()

Se invoca inmediatamente antes de que un componente esté desmontado y destruido. Realice cualquier limpieza necesaria en este método, como invalidar temporizadores, cancelar solicitudes de red o limpiar cualquier elemento DOM que se creó en `componentDidMount`.