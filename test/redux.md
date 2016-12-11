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

## Async action creators

Cuando utilizan _redux-thunk_ u otro middleware, lo mejor es mockear completamente el store Redux para las pruebas. Puede aplicar el middleware a un store simulado usando `redux-mock-store`. También se puede usar `nock` para mockear las peticiones HTTP.

**Ejemplo**
```js
import fetch from 'isomorphic-fetch';

function fetchTodosRequest() {
  return { type: FETCH_TODOS_REQUEST }
}

function fetchTodosSuccess(body) {
  return { type: FETCH_TODOS_SUCCESS, body }
}

function fetchTodosFailure(ex) {
  return { type: FETCH_TODOS_FAILURE, ex }
}

export function fetchTodos() {
  return dispatch => {
    dispatch(fetchTodosRequest())
    return fetch('http://example.com/todos')
      .then(res => res.json())
      .then(json => dispatch(fetchTodosSuccess(json.body)))
      .catch(ex => dispatch(fetchTodosFailure(ex)))
  }
}
```

El test sería

```js
import configureMockStore from 'redux-mock-store'
import thunk from 'redux-thunk'
import * as actions from '../../actions/counter'
import * as types from '../../constants/ActionTypes'
import nock from 'nock'
import expect from 'expect'

const middlewares = [thunk]
const mockStore = configureMockStore(middlewares)

describe('async actions', () => {
  afterEach(() => {
    nock.cleanAll()
  })

  it('creates FETCH_TODOS_SUCCESS when fetching todos has been done', () => {
    nock('http://example.com/').get('/todos').reply(200, {
      body: {
        todos: ['do something']
      }
    })

    const expectedActions = [
      {
        type: types.FETCH_TODOS_REQUEST
      }, {
        type: types.FETCH_TODOS_SUCCESS,
        body: {
          todos: ['do something']
        }
      }
    ]
    const store = mockStore({todos: []})

    return store.dispatch(actions.fetchTodos())
      .then(() => { 
        // return of async actions
        expect(store.getActions()).toEqual(expectedActions)
      })
    })
})

```