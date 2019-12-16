---
title: "State Management with Redux"
tags: ["react"]
published: true
date: "2019-12-11"
---

![](https://i.pinimg.com/originals/71/f0/a4/71f0a4c41735f951f9823725ee42cf8a.png)

This article is specifically aimed towards people who have some experience with React or people learning React.

According to the official [Redux](https://redux.js.org/) documentation, **Redux is a predictable state container for JavaScript apps**. It's important to see the [motivation](https://redux.js.org/introduction/motivation) behind redux intially and why we needed a separate "tool" for managing the state of our application. As our apps become more complex, it's quite hard to keep track of what's going on. The state cannot change by itself, it can only change as a consequence of a specific action, and that's what makes Redux a **predictable container**. In short, Redux will know what every action will do and how the state should change.

Redux is based upon Facebook's Flux architecture that follows the concept of Unidirectional data flow, however, there are still some differences in between those two. You can read about it [here](https://medium.com/edge-coders/the-difference-between-flux-and-redux-71d31b118c1).

You might've heard of the term **state** a numerous times before while working in React. Well, State holds the data of your application and the UI depends on it. You might even call it a combination of both UI and data. It's managed by the store in Redux.

Before going any further, let's first discuss about some important terms in Redux.

<br><br>

## Store

It contains the entire state of our application. The only way to change the state, is, via an action. We write pure reducer functions that return the new state of our application based on the type of action. We use createStore method imported from "redux" library to create our redux store. It takes in our reducer function, and middleware, when we have to make calls to API endpoints.

It's important to note that there should be a single store for our entire application, unlike flux's architecture, which deals with a thing for having multiple stores. Creating multiple stores in redux would be considered an anti-pattern.

```js
const store = createStore(reducer)
const store = createStore(reducer, applyMiddleware) // applyMiddleware or any middleware is used in case of asynchronous calls
```

Store also provides us some predefined methods:

**getState()**- returns the current state tree of your application.

**dispatch()**- dispatches an action.

**subscribe()**- listener; called everytime whenver the state changes.

<br>

## Action

Actions are payloads of information that send data from your application to the store. They are the only source of information to the store. An action should have a **type** property with it. Let's say we have a todo app, and we have to add a todo in it, an action might look like this:
<br><br>

```js
const ADD_TODO = {
  type: "ADD TODO",
  text: "Learn Redux",
}
```

<br><br>

## Reducer

Reducer is a [pure function](https://dev.to/keevcodes/pure-functions-in-react-2o7n) that takes in the previous state, an action, and returns the next state. A reducer would generally look like this.

```js
function reducer(state, action) {
  switch (action.type) {
    case "ADD TODO":
      return {...state, todo: action.payload}
    deafult:
     return state
  }
}
```

<br><br>

## Action Creators

As the name suggests, action creator is a function that "creates" an action or returns an action object.

```js
function addTodo() {
  return {
    type: "ADD TODO",
    text: "Learn Redux",
  }
}
```

<br><br><br>

# The Redux Flow

The most important principle in Redux is to never mutate the state object. Instead, return a new object, easily achievable by ES6's spread operator, like this:

```js
{...state}
```

<br>

So far, we've come to know that an action is responsible for a state change. Now, let's look at how data flows throughout this process.

Interestingly, an action object can be dispatched from anywhere in our application, even from components. So, after an action has been dispatched, it goes off to the reducer. Store calls the reducer function and pass two arguments into it, i.e., the current state and the dispatched action. Reducer checks what **type** of action has arrived and matches conditionally, and based on that, returns the new resulting state. So, basically reducer's only job is to return a predictable state based on a predictable input, therefore- a **pure function.** Store saves the new state, and you can easily use **store.getState()** to access the current state of the application.

<br><br><br>

# React-Redux

It is the official React binding for our Redux. Using React-Redux provides our React Components access to the Redux store and thus allows them to dispatch actions to the store to update data. Let's look at some important aspects of React-Redux.
<br><br><br>

## connect

connect() is a higher order function which connects a React component to the Redux store.

```js
connect(mapStateToProps)(mapDispatchToProps)(Component)
```
<br>

It takes in **mapStateToProps** and **mapDispatchToProps** as two parameters, and the "connected" Component. Thus, our Component has access to the state via mapStateToProps and the functions it can use to dispatch actions to the store, via mapDispatchToProps. One cool thing is that, if we don't provide mapDispatchToProps to the connect() function, our Component receives dispatch in its props by default.
<br>
<br>
<br>
## Provider

Provider let the components have access to the store that have been wrapped in the connect() function. Generally, we provide store at the top level, in index.js itself. We provide the store to the App.js, and thus, all the nested connected components receive the entire store via props.
<br><br><br>

## Redux-Thunk

Thunk is the middleware used to handle asynchronous actions in Redux such as making API calls to the backend. It returns a function and let that function dispatch an action object to the store. So, thunk is basically a function that returns an another function, and it's merely a 14 line code. How cool is that!!!

Let's understand all about Redux that we've talked so far by creating a simple application that just shows the user's github username.
<br><br><br><br>
<hr>

First off, let's create our **store**.

```js
store.js

import { createStore, applyMiddleware } from "redux"
import { userReducer } from "./userReducer"
import thunk from "redux-thunk"

const store = createStore(userReducer, applyMiddleware(thunk))

export default store
```
<br>




**store** takes in reducer, which, in our case, is called **userReducer** and **applyMiddleware(thunk)**. Here, we're just giving our store a reducer and a thunk middleware, for handling async actions.
<br>


```js
index.js

import React from "react"
import ReactDOM from "react-dom"
import App from "./App"
import { Provider } from "react-redux"
import store from "./store"

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("root")
)
```

<br>


```js
userReducer.js

const initialState = {
  user: null,
}

const userReducer = (state = initialState, action) => {
  switch (action.type) {
    case "FETCH_USER_DETAILS":
      return {
        ...state,
        user: action.data,
      }
    default:
      return state
  }
}

export { userReducer }
```
<br>

**userReducer** takes in state, action and returns the new state object based on the action type. Initially, state containes an empty user. After some time, we expect the state to change with the information coming from the API.




```js
ListUser.js

import React, {Component} from 'react'
import {connect} from 'react-redux'
import {listUser} from './actions'

class ListUser extends Component {
  handleClick = () => {
    this.props.dispatch(listUser('getify'))
  }

  render() {
    return (
      <div>
        <h1>{this.props.user}</h1>
        <button onClick={this.handleClick}>Submit</button>
      </div>
    )
  }
}

const mpaStateToProps = state => {
  return state
}

export default connect(mpaStateToProps)(ListUser)

```
<br>


This is our **ListUser** Component. It has a button, when clicked, shows the username. We're also connecting ListUser to the store. This means that it is receiving state and dispatch in its props. That's why we're able to make a call to the **listUser** action with username as a parameter.

```js
    this.props.dispatch(listUser('getify'))
```
<br>


```js
actions.js

const listUser = username => {
  return dispatch => {
    fetch(`https://api.github.com/users/${username}`)
      .then(res => res.json())
      .then(data => {
        dispatch({ type: "FETCH_USER_DETAILS", data: data.login })
      })
      .catch(err => console.log(err))
  }
}

export { listUser }
```
<br>

So, generally, action creators are functions that return a plain JavaScript object, but, in this case, our listUser action creator becomes a **"thunk"** that returns a function instead of an object. This function provides the dispatch method for dispatching an action. Here, the data is being fetched from the github API, and then the action is being dispatched with **type** and the incoming data. It then goes to the reducer and updates the store. Thunk comes in handy in case of async tasks whenver there's a delay of information. 


To summarize, when the button is being clicked, we call listUser action creator, which is basically a thunk, returns a function, fetches the info, dispatches an action, action goes to the userReducer, userReducer returns the new state, store gets updated, the ListUser Component receives the updated state via props. Thus, when we call **this.props.user**, we're able to get the username. Phew, that's the whole redux flow, as simple as that!!

Here's the [codesandbox](https://codesandbox.io/s/nervous-architecture-pqycx), if you wanna try it out.

<br><br>

_Thanks for reading this article._ 




