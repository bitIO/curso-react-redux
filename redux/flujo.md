# Flujo de datos

La arquitectura Redux gira en torno a un estricta política de flujo de datos unidireccional.

Esto significa que todos los datos de una aplicación siguen el mismo patrón de ciclo de vida, lo que hace que la lógica de la aplicación sea más predecible y más fácil de entender. También fomenta la normalización de los datos, de modo que no terminan con múltiples copias independientes de los mismos datos que no se conocen entre sí.

El ciclo de vida de datos en cualquier aplicación Redux sigue estos 4 pasos:

**1º Se invoca `store.dispatch(acction)`**

Una acción es un objeto plano que describe lo que sucedió. Por ejemplo "A Francisco le gustó el artículo con id 12342" o " Se ha añadido <text> a la lista de todos"

Puede llamar a `store.dispatch()`desde cualquier lugar de su aplicación - componentes y callbacks XHR, o eventos _cronificados_.

**2º - El _store_ llama a la función del reducer.**

Tened en cuenta que un _reducer_ es una función pura. Sólo calcula el siguiente estado. Debe ser completamente previsible (llamarla con las mismas entradas muchas veces debe producir las mismas salidas) no realizando ningún efecto secundario, como las llamadas API o las transiciones del enrutador. Esto debe suceder antes de que se envíe una acción.

**3º - El _reducer_ raíz puede combinar la salida de múltiples _reducers_ en un único árbol de estado.**

Cómo estructurar el reductor raíz depende de nosotros. Redux provee una función - `combineReducers()` - para dividir el reductor raíz en funciones separadas que cada una maneja una rama del árbol de estado.

Aquí es cómo funciona `combineReducers()`. Digamos que tiene dos reductores, uno para una lista de todos y otro para la configuración de filtro actualmente seleccionada:

```js
function todos(state = [], action) {
  // Somehow calculate it...
  return nextState
}

function visibleTodoFilter(state = 'SHOW_ALL', action) {
  // Somehow calculate it...
  return nextState
}

let todoApp = combineReducers({todos, visibleTodoFilter})
```
Cuando se emite una acción, el todoApp devuelto por combineReducers llamará a ambos reductores:

```js
let nextTodos = todos(state.todos, action)
let nextVisibleTodoFilter = visibleTodoFilter(state.visibleTodoFilter, action)
```
A continuación, combinará ambos conjuntos de resultados en un único árbol de estado:

```js
return {
  todos: nextTodos,
  visibleTodoFilter: nextVisibleTodoFilter
}

```

`combineReducers()` es una utilidad, no es obligatorio usarla; Puedes escribir tu propio reductor de raíz! ( o buscar otro por ahí)

**4º - El store guarda el árbol de estado completo devuelto por el reductor raíz.**

Ahora se invocará a cada _listener_ registrado con `store.subscribe()` y los _listeners_ pueden llamar a `store.getState()` para obtener el estado actual.

Por fin, la interfaz de usuario puede actualizarse para reflejar el nuevo estado.