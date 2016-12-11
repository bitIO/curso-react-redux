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

## Reducers

Estos deberían devolver un nuevo estado después de aplicar una acción

```js
import { ADD_TODO } from '../constants/ActionTypes'

const initialState = [
  {
    text: 'Use Redux',
    completed: false,
    id: 0
  }
]

export default function todos(state = initialState, action) {
  switch (action.type) {
    case ADD_TODO:
      return [
        {
          id: state.reduce((maxId, todo) => Math.max(todo.id, maxId), -1) + 1,
          completed: false,
          text: action.text
        },
        ...state
      ]

    default:
      return state
  }
}
```

Su test sería

```js
import reducer from '../../reducers/todos'
import * as types from '../../constants/ActionTypes'

describe('todos reducer', () => {
  it('should return the initial state', () => {
    expect(reducer(undefined, {})).toEqual([
      {
        text: 'Use Redux',
        completed: false,
        id: 0
      }
    ])
  })

  it('should handle ADD_TODO', () => {
    expect(reducer([], {
      type: types.ADD_TODO,
      text: 'Run the tests'
    })).toEqual([
      {
        text: 'Run the tests',
        completed: false,
        id: 0
      }
    ])

    expect(reducer([
      {
        text: 'Use Redux',
        completed: false,
        id: 0
      }
    ], {
      type: types.ADD_TODO,
      text: 'Run the tests'
    })).toEqual([
      {
        text: 'Run the tests',
        completed: false,
        id: 1
      }, {
        text: 'Use Redux',
        completed: false,
        id: 0
      }
    ])
  })
})
```

## Probar un componente

Consideremos el siguente componente

```js
import React, { PropTypes, Component } from 'react'
import TodoTextInput from './TodoTextInput'

class Header extends Component {
  handleSave(text) {
    if (text.length !== 0) {
      this.props.addTodo(text)
    }
  }

  render() {
    return (
      <header className='header'>
          <h1>todos</h1>
          <TodoTextInput 
            newTodo={true}
            onSave={this.handleSave.bind(this)}
            placeholder='What needs to be done?'
          />
      </header>
    )
  }
}

Header.propTypes = {
  addTodo: PropTypes.func.isRequired
}

export default Header
```

Para probar los componentes hacemos un ayudante de setup () que pasa las devoluciones de llamada rechazadas como apoyos y renderiza el componente con renderizado superficial. Esto permite que las pruebas individuales afirmen si las llamadas se llamaron cuando se esperaba.