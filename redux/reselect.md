# reselect

Reselect es una biblioteca sencilla para crear funciones selectoras memorizadas y componibles. Los selectores Reselect se pueden utilizar para calcular eficientemente los datos derivados del store de Redux.

## ¿Por qué?

En el ejemplo anterior, `mapStateToProps` llama a `getVisibleTodos` para _calcular_ los `todos`. Esto funciona muy bien, pero hay un inconveniente: `todos` se _calcula_ cada vez que se actualiza el componente. Si el árbol de estado es grande o el cálculo es costoso, repetir el cálculo en cada actualización puede causar problemas de rendimiento. Reselect puede ayudar a evitar estos recálculos innecesarios.

## Creación de un selector memorizados

Nos gustaría reemplazar `getVisibleTodos` con un selector memorizados que recalcula `todos` cuando el valor de `state.todos` o `state.visibilityFilter` cambia, pero no cuando ocurren cambios en otras partes (no relacionadas) del árbol de estado.

Reselect proporciona una función `createSelector` para crear selectores memorizados. `createSelector` toma una matriz de selectores de entrada y una función de transformación como sus argumentos. Si el árbol de estado Redux está mutado de una manera que hace que el valor de un selector de entrada cambie, el selector llamará a su función de transformación con los valores de los selectores de entrada como argumentos y devolverá el resultado. Si los valores de los selectores de entrada son iguales a los de la llamada anterior al selector, devolverá el valor previamente calculado en lugar de llamar a la función de transformación.

Vamos a definir un selector memorizado llamado `getVisibleTodos` para reemplazar la versión no-memorizada anterior:

```js
import { createSelector } from 'reselect'

const getVisibilityFilter = (state) => state.visibilityFilter
const getTodos = (state) => state.todos

export const getVisibleTodos = createSelector(
  [ getVisibilityFilter, getTodos ],
  (visibilityFilter, todos) => {
    switch (visibilityFilter) {
      case 'SHOW_ALL':
        return todos
      case 'SHOW_COMPLETED':
        return todos.filter(t => t.completed)
      case 'SHOW_ACTIVE':
        return todos.filter(t => !t.completed)
    }
  }
)
```

### Componer selectores

Un selector memorizado puede ser un selector de entrada a otro selector memorizado. Vamos a usar `getVisibleTodos` como un selector de entrada a un selector que filtra, adicionalmente, por palabra clave:

```js
const getKeyword = (state) => state.keyword

const getVisibleTodosFilteredByKeyword = createSelector(
  [ getVisibleTodos, getKeyword ],
  (visibleTodos, keyword) => visibleTodos.filter(
    todo => todo.text.indexOf(keyword) > -1
  )
)
```

### Conectar el selector a React

```js
import { connect } from 'react-redux'
import { toggleTodo } from '../actions'
import TodoList from '../components/TodoList'
import { getVisibleTodos } from '../selectors'

const mapStateToProps = (state) => {
  return {
    todos: getVisibleTodos(state)
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
