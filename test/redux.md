# Redux

Para realizar las pruebas de Redux, se recomienda usar Jest

```
npm install --save-dev jest babel-jest
```


## Action creators

En Redux, los _action creators_ que retornan objetos planos. Cuando los probamos, queremos comprobar que devuelven el objeto esperado y que se ha llamado a la acción correcta.

```js
export function addTodo(text) {
  return {
    type: 'ADD_TODO',
    text
  }
}
```

can be tested like:

```js
import * as actions from '../../actions/TodoActions'
import * as types from '../../constants/ActionTypes'

describe('actions', () => {
  it('should create an action to add a todo', () => {
    const text = 'Finish docs'
    const expectedAction = {
      type: types.ADD_TODO,
      text
    }
    expect(actions.addTodo(text)).toEqual(expectedAction)
  })
})
```