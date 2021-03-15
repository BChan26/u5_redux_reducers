# Redux Reducers

<img src="https://cdn-images-1.medium.com/max/1600/1*EdiFUfbTNmk_IxFDNqokqg.png" alt="redux" height="400" />



## Requirements

- Have `intro to redux` working.

## Objectives

- Learn how redux reducers work
- Implement reducers

## What Are Reducers?

Reducers are functions that return some type of state. You can think of them as little modules that we can break up our state into instead of having one large object containing every piece of state for our application. Here's an example:

```js
const initialState = {
  myName: ''
}

const SomeReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'SOME_TYPE':
      return { ...state, myName: action.payload }
    default:
      return { ...state }
  }
}

export default SomeReducer
```

As you can see theres quite a bit going on here so let's break it down:

- We set up some type of initial state, you can either pass this as an argument to our reducer or store it in it's own variable.

- We also recieve an action as the second argument for our reducer, this is coming from redux when we wire this reducer to our store.

- The action variable contains two things, a `Type` and an `Action`.

- We use a switch statement to check what the action type is, dependant on that type we'll perform some kind of state update utilizing the payload from the action. ( We'll set these up later on )

- Our reducer should always return a new object with our state by default.

- We export this reducer for use later on.

## Adding A Reducer For Our Todo List

Open your Todo List application from earlier. We'll be adding a reducer to handle some state and linking it to our store.

Create a new folder in the `store` directory called `reducers`

In the reducers folder create a new file called `TodoReducer.js`.

Let's go ahead and build this reducer, add the following to your `TodoReducer`:

```js
const initialState = {
  todos: [],
  newTodo: ''
}

const TodoReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'ADD_TODO':
      return { ...state, todos: [...state.todos, action.payload] }
    case 'NEW_TODO':
      return { ...state, newTodo: action.payload }
    default:
      return { ...state }
  }
}

export default TodoReducer
```

Let's install a dependency to allow our redux devtools to read our state:

```sh
npm install redux-devtools-extension
```

Let's wire this reducer to our store. In the `store/index.js` add the following:

```js
import { createStore } from 'redux'
import { composeWithDevTools } from 'redux-devtools-extension'
import TodoReducer from './reducers/TodoReducer'

const store = createStore(TodoReducer, composeWithDevTools())

export default store
```

Open your redux devtools and click on the `state` tab, you should see our todos array and the newTodo string.

## Multiple Reducers

We've successfully set up a single reducer to handle our todos, but what if we wanted to use multiple reducers? Let's start by creating a new reducer in our `reducers` folder called `AppReducer.js`. Add the following:

```js
const initialState = {
  appLoading: false
}

const AppReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'TOGGLE_APP_LOADING':
      return { ...state, appLoading: action.payload }
    default:
      return { ...state }
  }
}

export default AppReducer
```

Let's utilize this reducer with our store. In the `store/index.js` file, pull `combineReducers` from the redux import. Let's make our store file look like the following:

```js
import { createStore, combineReducers } from 'redux'
import { composeWithDevTools } from 'redux-devtools-extension'
import TodoReducer from './reducers/TodoReducer'
import AppReducer from './reducers/AppReducer'

const store = createStore(
  combineReducers({ todoState: TodoReducer, appState: AppReducer }),
  composeWithDevTools()
)

export default store
```

Let's break this down:

- We import the `combineReducers` function from redux.
- We put the `combineReducers` function inside of our `createStore` function and pass in an object with how we want our state to be formatted.
- We tell redux that we want a `todoState` set to our `TodoReducer` and an `appState` set to our `AppReducer`.

This is what's known as middleware in Redux.

In your redux devtools you should now see two new things:

- a `todoState` object
- an `appState` object

With our state from each reducer inside of them.

## Resources

[Understanding Reducers](https://css-tricks.com/understanding-how-reducers-are-used-in-redux/)

[What Is A Reducer](https://daveceddia.com/what-is-a-reducer/)
