# Redux Reducers

<img src="https://cdn-images-1.medium.com/max/1600/1*EdiFUfbTNmk_IxFDNqokqg.png" alt="redux" height="400" />

## Overview
In this lesson, we'll be learning about Redux Reducers, functions that allow us to break up our application state into manageable sections associated with certain parts of the overall state.

## Objectives
- Learn how redux reducers work
- Implement reducers

## Getting Started
- Have `intro to redux` working.
- We'll be building off from where we finished last.

___
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

![](https://forum.attainu.com/uploads/default/optimized/1X/764a6fca95c6f0e0783b4efc53877cc09541360f_2_690x209.png)

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

We've now successfully set up our store! We'll be moving on to actions and types in the next lesson.


![](https://res.cloudinary.com/ahonore42/image/upload/v1615871989/ga/Screen_Shot_2021-03-16_at_12.18.56_AM_k2upar.png)

## Recap
In this lesson we learned about setting the initial state of our store within our `reducers`. Applications often have multiple reducers to split up different parts of the overall state into manageable sections associate with a particular part of the app. In this case, we set up an `AppReducer` to handle whether or not our main app was loading, and a `TodoReducer` to manage the state of our `todos`.

## Resources

- [Understanding Reducers](https://css-tricks.com/understanding-how-reducers-are-used-in-redux/)
- [What Is A Reducer](https://daveceddia.com/what-is-a-reducer/)
