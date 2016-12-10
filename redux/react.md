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
import Todo from './Todo'

const TodoList = ({ todos, onTodoClick }) => (
  <ul>
    {todos.map(todo =>
      <Todo
        key={todo.id}
        {...todo}
        onClick={() => onTodoClick(todo.id)}
      />
    )}
  </ul>
)

TodoList.propTypes = {
  todos: PropTypes.arrayOf(PropTypes.shape({
    id: PropTypes.number.isRequired,
    completed: PropTypes.bool.isRequired,
    text: PropTypes.string.isRequired
  }).isRequired).isRequired,
  onTodoClick: PropTypes.func.isRequired
}

export default TodoList
```

### Componentes contenedor

Técnicamente, un componente de contenedor es sólo un componente React que utiliza `store.subscribe()` para leer una parte del árbol de estado Redux y suministrar _props_ a un componente de presentación que lo procesa. Se puede escribir un componente de contenedor manualmente, pero Redux sugiere generar componentes de contenedor con la función `connect()` de la biblioteca `react-redux` que proporciona muchas optimizaciones útiles para evitar re-renders innecesarios.

Para usar `connect()`, debe definir una función especial llamada `mapStateToProps` que indique cómo transformar el estado de _store_ Redux actual en parte de las _props_ que desea transferir a un componente de presentación.

Además de leer el estado, los componentes contenedor pueden enviar acciones. De manera similar a `mapStateToProps`, se puede definir una función llamada `mapDispatchToProps()` que recibe el método `dispatch()` y devuelve los props con callbacks que queremos inyectar en el componente de presentación.


```js
import { connect } from 'react-redux'

const getVisibleTodos = (todos, filter) => {
  switch (filter) {
    case 'SHOW_ALL':
      return todos
    case 'SHOW_COMPLETED':
      return todos.filter(t => t.completed)
    case 'SHOW_ACTIVE':
      return todos.filter(t => !t.completed)
  }
}

const mapStateToProps = (state) => {
  return {
    todos: getVisibleTodos(state.todos, state.visibilityFilter)
  }
}

const mapDispatchToProps = (dispatch) => {
  return {
    onTodoClick: (id) => {
      dispatch(toggleTodo(id))
    }
  }
}

const VisibleTodoList = connect(
  mapStateToProps,
  mapDispatchToProps
)(TodoList)

export default VisibleTodoList
```

