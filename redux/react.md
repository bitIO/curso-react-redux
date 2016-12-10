# Redux con React

**Redux no tiene relación con React**. Puedes escribir aplicaciones Redux con React, Angular, Ember, jQuery o JavaScript puro.

Dicho esto, Redux funciona especialmente bien con bibliotecas como React y Deku porque te permiten describir la interfaz de usuario como una función de estado, y Redux emite actualizaciones de estado en respuesta a las acciones.

Para poder usar Redux con React necesitamos instalar el paquete npm `npm install --save react-redux.`

## Cambio de paradigma

A partir de ahora, tendremos dos tipos de componentes, los componetes de representación y los componentes contenedores - más detalles en [este artículo en medium de Dan Abramov](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0).

<table>
<thead>
  <tr>
    <th></th>
    <th>Componentes de presentación</th>
    <th>Componentes contenedor</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Propósito</td>
    <td>Cómo se ven las cosas (marcado, estilos)</td>
    <td>Cómo funcionan las cosas (búsqueda de datos, actualizaciones de estado)</td>
  </tr>
  <tr>
   <td>Consciente de Redux</td>
   <td>No</td>
   <td>Sí</td>
  </tr>
  <tr>
    <td>Lectura de datos</td>
    <td>Lee de props</td>
    <td>Se suscribe al estado Redux</td>
  </tr>
  <tr>
    <td>Modificar datos</td>
    <td>Callbacks en props</td>
    <td>Dispatch acciones Redux</td>
  </tr>
<tbody>
</table>

### Componentes de presentación

Describen la apariencia pero no saben de dónde vienen los datos, ni cómo cambiarlos. Si migra de Redux a otra cosa, podrá mantener todos estos componentes exactamente iguales. No tienen dependencia de Redux.

```js
import React, { PropTypes } from 'react'

const Todo = ({ onClick, completed, text }) => (
  <li
    onClick={onClick}
    style={{
      textDecoration: completed ? 'line-through' : 'none'
    }}
  >
    {text}
  </li>
)

Todo.propTypes = {
  onClick: PropTypes.func.isRequired,
  completed: PropTypes.bool.isRequired,
  text: PropTypes.string.isRequired
}

export default Todo
```