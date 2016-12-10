# Estado

La mayoría de los componentes simplemente deben renderizar sus propiedades. Pero los componentes también ofrecen estado, y se utiliza para almacenar información sobre el componente que puede cambiar con el tiempo. Normalmente, el cambio se produce como resultado de eventos de usuario o eventos del sistema (es decir, como una respuesta a la entrada del usuario, una solicitud del servidor o el paso del tiempo).

* Si un componente tiene un estado, un estado predeterminado debe proporcionar utilizando `getInitialState()` - deprecated - o en el constructor inicializarlo.
* Los cambios de estado suelen ser la forma en que se inicia la re-renderización de un componente y todos los subcomponentes.
* Para cambiar el estado usamos `this.setState()`. React usará sus algoritmos de _diff_ para modificarlo y hacer *merge*.
* Un cambio de estado combina nuevos datos con datos antiguos que ya están contenidos en el estado (es decir, `this.state`)
* Un cambio de estado se ocupa internamente de los re-renders de llamada. Nunca deberías llamar directamente a `this.render()`
* El objeto de estado sólo debe contener la cantidad mínima de datos necesarios para la interfaz de usuario. No coloque datos calculados, otros componentes React o _props_ en el objeto de estado.

```js
class Alumno extends React.Component {
  constructor(props) {
    super(props);
    this.state = {};
    // o si queremos emplear los valores de los props
    // this.state = { ...props };
  }
}
```


## State vs Props

El estado de los componentes y las props comparten un cierto punto en común: 

* * **Ambos son objetos planos JS**
* Ambos pueden tener valores por defecto
* Ambos deben ser accedidos / leídos a través de `this.props` o `this.state`, pero ninguno debe ser dado valores de esta manera. Es decir, ambos son de sólo lectura cuando se usa esto.

Sin embargo, se utilizan por diferentes razones y de diferentes maneras.
**Props:**

* * Se pasan a un componente desde arriba, ya sea un componente padre o desde el ámbito de inicio donde React se procesa originalmente.
* Están diseñados como valores de configuración pasados al componente. Piensad en ellos como argumentos pasados en una función (si no usa JSX que es exactamente lo que son).
* Son inmutables para el componente que los recibe. Es decir, no cambian desde dentro del componente


**Estado:**

* Es una representación serializable de datos (un objeto JS) en un punto en el tiempo que normalmente está vinculado a la interfaz de usuario
* Siempre debe comenzar con un valor predeterminado y, a continuación, el estado es mutado internamente por el componente mediante `setState()`
* Sólo puede ser mutado por el componente que contiene el estado. Es privado en este sentido.
* **No debes mutar el estado de los componentes del niño**. Un componente nunca debería haber compartido estado mutable.
* Debe contener sólo la cantidad mínima de datos necesarios para representar el estado de su UI.
* Debe ser evitado siempre que sea posible 😳. **Es decir, los componentes sin estado son ideales, los componentes con estado añaden complejidad**. La documentación de React sugiere:


> Un patrón común es crear varios componentes *stateless* que sólo representan datos y tener un componente con estado por encima de ellos en la jerarquía que pasa su estado a sus hijos a través de las *props*. El componente con estado encapsula toda la lógica de interacción, mientras que los componentes *stateless* se encarga de renderizar los datos de forma declarativa

## Usar el estado de manera correcta

Debe haber una sola "fuente de verdad" para cualquier dato que cambie en una aplicación React. Normalmente, el estado se agrega primero al componente que lo necesita para renderizar. Entonces, si otros componentes también lo necesitan, podemos *elevarlo* al ancestro común más cercano. En lugar de intentar sincronizar el estado entre los diferentes componentes, debe recoradr que en React, los datos fluyen de arriba hacia abajo.

Llevar el estado a un ancestro común implica la escritura de más código *boilerplate*  pero como beneficio, necesitaremos menos trabajo para encontrar y aislar errores. Puesto que cualquier estado "vive" en algún componente y sólo ese componente  puede cambiarlo, el aread de errores se reduce considerablemente.