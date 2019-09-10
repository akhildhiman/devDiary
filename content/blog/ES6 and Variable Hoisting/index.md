---
title: 'ES6 and Variable Hoisting'
tags: ["javascript"]
published: true
date: '2019-09-10'
---

![](https://miro.medium.com/max/800/1*_UFRRMoz4KCSW1lsoNylew.jpeg)

Before ES6 came into play, **var**  was the preferred choice of variable declaration in JavaScript, but, var had its own issues which later got resolved with the introduction of **let** and **const** in ECMAScript 2015. Before moving any further, let's first discuss about the var keyword.

<br>

## var

In general, var is **globally scoped** when it is declared outside a function and is **locally scoped** when decalred inside a function. Scope in programming terms is nothing but the visibility of variables. Let's take an example for a better understanding.

```javascript
var firstName = "Rick";
function random() {
	var lastName = "Sanchez";
}
```
<br>

This will give an error, because the variable **lastName** is locally scoped, and is not available outside the function.

```javascript
var firstName = "Rick";
function random() {
	var lastName = "Sanchez";
}
console.log(lastName); // lastName is not defined
```
<br><br>

## Hoisting of var

Hoisting in JavaScript is defined as - _Variable and function **declarations** are moved to the top of their respective scope before code execution._

So, here we are logging into the console and then assigning the value to the **name** variable. What JavaScript does is that, it looks for the variable declarations and hoist them to the top, initializing them with a value of **undefined.**

```js
console.log(name);
var name = "Jerry";
```

How JavaScript understands the above code in the compiling phase:

```js
var name;
console.log(name); 
name = "Jerry";
```

<br>

**It is important to note that the variables declared with var can be REASSIGNED.**

<br> 

## let 

let is block scoped. It means that the varible declared with the let keyword is only available inside the block.

```js
let firstName = "Rick";
if (true) {
	let lastName = "Sanchez";
	console.log(lastName);
}
console.log(lastName); // lastName is not defined
```
<br>

Here, we're getting an error that **_lastName_** is not defined, because, let is block scoped and is not available outside its scope. If we had used **var** in place of **let** in this example, the output would've been **Sanchez** because of the re-assignment feature of var. 

This is one of the things that could cause problems in your code. Let's compare them both to see the difference.


```js
    var greeting = "hey hi";
    if (true) {
        var greeting = "say Hello instead"; 
    }
    console.log(greeting) //"say Hello instead"
```
<br>

```js
    let greeting = "say Hi";
    if (true) {
        let greeting = "say Hello instead";
    }
    console.log(greeting);//"say Hi"
```

<br><br>

##const

Like the name suggests, const can never be redeclared or redefined. It remains constant. Like let declarations, const is also **block scoped.**

```js
const Name = "Rick";
const Name = "Jerry"; //Identifier 'Name' has already been declared
```

<br><br>

## Hoisting of let and const

Well, all variable declarations are hoisted in JavaScript whether it's var, let or const. The difference lies in the initialization part. 

**var gets initialized with a value of undefined while let and const are not initialized.**


<br><br><br><br>

_Thank you for reading this article._











