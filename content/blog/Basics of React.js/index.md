---
title: 'Beginner''s guide to React.js'
tags: ["react"]
published: true
date: '2019-09-27'
---

![](https://reactjs.org/logo-og.png)

In this article, we'll discuss about some of the basic topics in React:
* #### Virtual DOM
* #### JSX
* #### Components
* #### Props
* #### State
<br>

**React** is a JavaScript library used for building user interfaces. In other words, you could say it's responsible for the view layer of our web application. 
<br><br>
### Declarative
React follows the declarative style of programming. It simply means, **"what we want"**, unlike the imperative style that follows the **"how do we do this ?"** approach. Let me explain a bit further.  Assume, your objective is to go to the hospital. Now, there are two ways of going about this. 

***Decalarative***: You just want to reach the hospital.

***Imperative***: You look for the route, board a taxi, and finally reach the destination.
<br><br>

### Component-Based
This is one of the best features in React. At this time, just remember that components in React are just basically JavaScript functions, and there are two ways to define them. We'll cover more of the components as we move further.
<br><br>


# Virtual DOM
You might know about DOM, right ? Well, the virtual DOM is quite similar to the actual DOM. You might recall in JS, whenever anything changed, the DOM would update the nodes in the DOM tree, and subsequently our page would change.

The virtual DOM remains in-sync with the actual DOM. What React does it that whenver we update (change the state) our application, it compares the previous state to the updated state (using the diff algorithm), in turn creates an updated Virtual DOM, and finally apply the changes to the actual DOM. Well, the question is, "Why can't React update the actual DOM directly ?" If we change the state every now and then, we would have to re-render our elements every time, and that makes for a really expensive task. Virtual DOM **only** updates the latest changes that have been made, and thus makes it more efficient.

You could read more about this in the [official docs](https://reactjs.org/docs/reconciliation.html).

![](https://www.cronj.com/blog/wp-content/uploads/DOM2.png)
*image courtesy of cronJ.com*

<br>


#JSX
JSX looks similar to HTML, but it's not. It's the syntax notation used to "create" React elements and thus the UI. 

```js
function App() {
  return(
    <h1 className="header">Hello World</h1> // JSX
  )
}
```

Under the hood, if you try and open [babel](https://babeljs.io/), React sees the above code like this:
<br><br>

```js
React.createElement("h1", {className: "header"}, "Hello World");
```
<br>


You could style React components in a number of ways. There's a good article on styling [**here**](https://codeburst.io/4-four-ways-to-style-react-components-ac6f323da822).

<br>

#Components
Component is a JavaScript function or a class that returns a React element and describes how the UI should look like. Let's understand this with an example.

#### Functional Component
```js
function App() {
    return(
        <h1>Hello World<h1>
    )
}

Output: Hello World

```
<br>

You cannot use methods like setState() in functional components, that's why they're called **stateless components** because functional components return the UI from the input provided by us. On the other hand, if you look at a class component, you could see it has a constructor, a state, and after we could even change the state. So, it's somewhat of a **stateful component.**

<br>

#### Class-Based Component
```js
class App extends React.Component {
    render() {
        <h1>Hello World<h1>
    }
}

Output: Hello World

```
**render()** method is required when it comes to class based components.

<br>

# Props
Props are used for passing data from one component to another component. You could say that props are like data-attributes. Let's see why!
```js
function Name(props) {
    return(
        <h1>Hello, {props.name}</h1>
    )
}

function App() {
    return(
        <Name name="Rick"/>
    )
}

ReactDOM.render(<App />, document.getElementById("root"))


Output: Hello, Rick
```
<br>

So, we have two componets here, i.e., **Name** and **App** component. React calls the Name component with {name: "Rick"} as the props which we've defined in the App component. Remember, props is in the object form. In simple words, Name will receive the "name" key of the object props, and thus renders the element onto the screen.

The Documentation says- **"All React components must act like pure functions with respect to their props."** Props are read-only. Props are immutable. Props are always passed from outside the component. The function returns what we want it to return. The solution was to define something that would come in handy while handling dynamic data. Yes, you're thinking right, it's called **State**.
<br><br>

# State
State is one of the most important concepts in React. Almost everything depends on it. With every state change, our view changes. 
This is a simple example of a counter. Whenver the button is being clicked, the count value should increment by one.
```js

class App extends React.Component {
  constructor() {
    super()
    this.state = {
      count: 0
    }
    this.handleClick = this.handleClick.bind(this)
  }

  handleClick() {
    this.setState((prevState) => {
      return {
        count: prevState.count + 1
      }
    })

  }

  render() {
    return(
      <div>
        <h1>{this.state.count}</h1>
        <button onClick={this.handleClick}>Increment</button>
      </div>
    )
  }
}

```
<br>

What we want to do here is to update the counter, i.e., update the previous state by one. We cannot go here and say:

```js
this.state.count + 1
```
<br>

Instead, in order to set the state, React uses **setState()** method. It's important to note that setState is asynchronous. In order to avoid that, callback comes in handy. In the above code snippet, while setting our state, we've used a callback receiving prevState as a parameter. So, we return the previous state and increment it by one, evert time the button is clicked. 


<iframe height="400px" width="100%" src="https://repl.it/repls/TepidSharpLifecycle?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

<br><br><br>
*Thank You for reading this article.*








