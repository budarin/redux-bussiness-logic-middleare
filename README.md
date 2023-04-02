# redux-business-logic-middleare

Middleware for processing business login in redux application.

## Instllation

Using npm

```shell
npm install --save-dev @budarin/redux-business-logic-middleare
```

Using yarn

```shell
yarn add -D @budarin/redux-business-logic-middleare
```

## Using:

Let's implement a simple business rule for Todo: “when creating a todo, send the todo to the server and to the redux store”.

Let's describe the essence of Todo — its constants, actions, business rules and a redeser in accordance with the concept of [ducks](https://github.com/erikras/ducks-modular-redux).

`./ducks/todo.js`

```js
import { addTodo } from 'src/client/services/api'
import { onAction } from'@budarin/redux-business-logic-middleare';

export const ADD_TODO = 'TODO/ADD_TODO';

export const addTodo = ( todo ) => ({
    type: ADD_TODO,
    payload: { todo }
});

// let's add our business rule
onAction(ADD_TODO, (store, next, action) => {
    // call the API method to send todo to the server
    void addTodo(action.payload).catch((error) => console.error(error) );

    // otherwise, we pass the action to the next middleware
    return next(action);
})

export default const reducer = (state = initialState, action) => {
    switch (action.type) {
        case 'TODO/ADD_TODO': {
            ...
        }
        default:
            return state;
    }
};

...
```

Add midleware to stores middlewares

```js
import todoReducer from '../ducks/todo.js'
import { createStore, applyMiddleware, combineReducers } from 'redux'
import { bussinesLogicMiddleware } from '@budarin/redux-business-logic-middleare';

const reducers = combineReducers(
    ...
    todoReducer,
    ...
);

const store = createStore(
    reducers, 
    initialState, 
    applyMiddleware(bussinesLogicMiddleware)
);
```

To remove bussines-rule from processing

```js
import { ADD_TODO } from '../ducks/todo.js'
import { offAction } from '@budarin/redux-business-logic-middleare';

offAction(ADD_TODO);
```
To remove all bussines-rules from processing

```js
import { removeAllBusinessRules } from '@budarin/redux-business-logic-middleare';

removeAllBusinessRules();
```