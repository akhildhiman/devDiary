---
title: 'Breaking down JSX'
tags: ["React"]
published: true
date: '2019-12-21'
---
![](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2017/10/1506909912jsx.jpg)

JSX might seem like some fancy term but it's nothing more than syntactic sugar for the **React.createElement()** API.

React.createElement takes three parameters, i.e., type, props and children.


```js
React.createElement(type, props, children)
```

```js
<button color="red">Click!</button>
```
So, this is our JSX- a button with a color property. The HTML element/Component is our type here, color is our prop, and the text "Click" is children of our component. 


The compiled code would look like this.
```js
React.createElement("button", { color: "red" }, "Click!")
```
<br><br>

## What actually happens under the hood ?

[Babel](https://babeljs.io), the transpiler is mainly responsible for allowing us to use JSX because we know that in order for the browser to understand, the code has to be converted into ES5, and that's what Babel is doing. Any ES6 thing like arrow function, class, let etc. will be handled by Babel. 


```js
const App = (props) => {
    return (
        <div>
            `Hello, my name is {props.name}`
        </div>
    )
}

ReactDOM.render(<App name="Sam" />, document.getElementById("root")) // suppose we have index.html and there's an element with an id of root in which we are rendering our application
```
<br>


Babel compiles the above code in the "form" that only browser can understand.


```js
var App = function App(props) {
    return React.createElement("div", {name: "Sam"}, "hello")
}

ReactDOM.render(React.createElement(App,
 {name: "Sam"}
 ), document.getElementById("root"))
```





