---
title: 'Lifting State Up'
tags: ["React"]
published: true
date: '2019-12-22'
---

![](https://reactjs.org/logo-og.png)

You might've heard about _Lifting Your State_ hundreds of times before but have really been unable to comprehend what does it actually mean, neither do I.

So, I looked up the [official docs](https://reactjs.org/docs/lifting-state-up.html) and it says- _**Often, several components need to reflect the same changing data. We recommend lifting the shared state up to their closest common ancestor.**_ 

Let's understand this with a simple example.


```js
import React from "react";
import ReactDOM from "react-dom";

import "./styles.css";

class Counter extends React.Component {
  state = {
    counter: 0
  };

  decrement = () => {
    this.setState({ counter: this.state.counter - 1 });
  };

  increment = () => {
    this.setState({ counter: this.state.counter + 1 });
  };

  render() {
    return (
      <>
        <button type="button" onClick={this.decrement}>
          -
        </button>
        {this.state.counter}
        <button type="button" onClick={this.increment}>
          +
        </button>
      </>
    );
  }
}

class App extends React.Component {
  render() {
    return (
      <div className="App">
        <Counter />
        <p>You have clicked 0 times!</p>
      </div>
    );
  }
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

First off, we have a **Counter** component. This component has the state initialized with a value of counter set to zero. Also, we have increment and decrement methods. The output would something look like this.

<iframe
     src="https://codesandbox.io/embed/agitated-tereshkova-hjcby?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="agitated-tereshkova-hjcby"
     allow="geolocation; microphone; camera; midi; vr; accelerometer; gyroscope; payment; ambient-light-sensor; encrypted-media; usb"
     sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"
 ></iframe>

<br>

 Now, we want **"You clicked 0 times"** to increment on clicking the button. In order to do that, could we do add **```clicks: 0```** field in **Counter** like this and pass this data in state to the **App** component ? I don't see any other solution.

 ```js
class Counter extends React.Component {
  state = {
    counter: 0,
    clicks: 0
  };

  ////
  ////

  class App extends React.Component {
  render() {
    return (
      <div className="App">
        <Counter />
        <p>You have clicked 0 times!</p> // how do I update this ?
      </div>
    );
  }
}
  ```

Well, I don't think that's quite possible.

So, the solution would be **lift the state up**. Instead of keeping the state in **Counter**, we move the state to our **App** component. See here.

```js
import React from "react";
import ReactDOM from "react-dom";

import "./styles.css";

class Counter extends React.Component {
  render() {
    const { increment, counter, decrement } = this.props;

    return (
      <>
        <button type="button" onClick={decrement}>
          -
        </button>
        {counter}
        <button type="button" onClick={increment}>
          +
        </button>
      </>
    );
  }
}

class App extends React.Component {
  state = {
    clicks: 0,
    counter: 0
  };

  decrement = () => {
    this.setState({
      counter: this.state.counter - 1,
      clicks: this.state.clicks + 1
    });
  };

  increment = () => {
    this.setState({
      counter: this.state.counter + 1,
      clicks: this.state.clicks + 1
    });
  };

  render() {
    const { clicks, counter } = this.state;

    return (
      <div className="App">
        <Counter
          increment={this.increment}
          decrement={this.decrement}
          counter={counter}
        />
        <p>You have clicked {clicks} times!</p>
      </div>
    );
  }
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```
<iframe
     src="https://codesandbox.io/embed/strange-thunder-h1i7m?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="strange-thunder-h1i7m"
     allow="geolocation; microphone; camera; midi; vr; accelerometer; gyroscope; payment; ambient-light-sensor; encrypted-media; usb"
     sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"
></iframe>

<br>

Now, the funny part is, you might've done this unknowingly before or you'd consider this approach to be the general way of doing it. I totally agree with you, but yeah, that's the whole lifting state up fuzz is. Even the docs definition would make some sense now.

Try the **[CodeSandBox](https://codesandbox.io/s/strange-thunder-h1i7m)** here.

<br><br>

Thank You for reading this article. Follow me on [Twitter](https://twitter.com/_himalayan_) for more updates.






  





