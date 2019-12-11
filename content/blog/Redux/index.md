---
title: "State Management with Redux"
tags: ["react"]
published: true
date: "2019-12-04"
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

<br><br>


## Action

Actions are payloads of information that send data from your application to the store. They are the only source of information to the store. An action should have a **type** property with it. Let's say we have a todo app, and we have to add a todo in it, an action might look like this:
<br><br>


```js
const ADD_TODO = {
  type: "ADD TODO",
  text: "Learn Redux"
}
```
<br><br>

## Reducer

Reducer is a [pure function](https://dev.to/keevcodes/pure-functions-in-react-2o7n) that takes in the previous state, an action, and returns the next state. A reducer would generally look like this.

```js
function reducer(state, action) {
    switch(action.type) {
        case "ADD TODO":
            return newState
        deafult: 
            return state
    }
}
```
<br><br><br>

## Action Creators

As the name suggests, action creator is a function that creates an action or returns an action object.
```js
function addTodo() {
    return {
        type: "ADD TODO",
        text: "Learn Redux"
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


# React-Redux

It is the official React binding for our Redux. Using React-Redux provides our React Components access to the Redux store and thus allows them to dispatch actions to the store to update data. Let's look at some important aspects of React-Redux.

## Provider














