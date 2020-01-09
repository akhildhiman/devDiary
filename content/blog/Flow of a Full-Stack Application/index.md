---
title: 'Flow of a Full-Stack Application'
tags: ["React"]
published: true
date: '2020-01-05'
---


![](https://miro.medium.com/max/600/1*o3K3iFBBtyM8aRAUrfrZyw.png)

In this article, I'll explain the whole flow of a full-stack application built on top of the MERN stack. 

* Node.js
* MongoDB
* Express.js
* React
* Redux
* Webpack
* Babel

<br><br>

## Configuring Webpack and Babel
```js
//webpack.config.js
var webpack = require('webpack');
var path = require('path');
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  mode: 'development',
  devtool: 'inline-source-map',
  entry: [
    './client/index.js',
  ],
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: { loader: 'babel-loader' },
      },
      {
        test: /\.(scss|css)$/,
        use: [
          { loader: MiniCssExtractPlugin.loader },
          {
            loader: 'css-loader',
          },
          { loader: 'sass-loader' }
        ]
      },
      {
        test: /\.(png|jpg|gif)$/,
        use: [
          {
            loader: 'file-loader',
            options: {}
          }
        ]
      }
    ]
  },
  resolve: {
    extensions: ['.js', '.jsx']
  },
  output: {
    filename: 'bundle.js',
    path: __dirname + '/dist/bundle/',
    publicPath: '/static/'
  },
  plugins: [
    new webpack.HotModuleReplacementPlugin(),
    new webpack.DefinePlugin({
      'process.env': {
        'NODE_ENV': JSON.stringify('development')
      }
    }),
    new MiniCssExtractPlugin({
      filename: "bundle.css",
    })
  ]
}

//.babelrc
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react"
  ],
  "plugins": [
    "@babel/plugin-proposal-class-properties",
    "@babel/plugin-transform-react-jsx",
    "@babel/plugin-transform-runtime"
  ]
}


```
Before getting into the main parts, first we have to configure Webpack. For those of you who are not familiar with Webpack, it's basically a module bundler which bundles all of our code and generates a bundle and give to the browser that it can understand. I've written an entire article on [Setting up a React project using Webpack and Babel](http://localhost:8000/Everything%20you%20need%20to%20know%20about%20React%20Router/), you can read about it for a better understanding.

Babel helps in transform our code, e.g., **babel-loader** looks for the ```js/jsx``` files and transform into something that the browser can understand. Similarly, **file-loade**r would load all the static files with the extensions of ```png,jpg,gif``` and transform them. **css-loader** and **sass-loader** convert the styles that we have used. 

Further in .babelrc, we've defined some babel presets and plugins. **@babel/preset-env** transpiles ES6 syntax to ES5 or whatever the browser can understand whereas **@babel/preset-react** handles the conversion of JSX modules into the simpler form. **@babel/plugin-proposal-class-properties** transpiles the class modules and so on. 

We set the entry point of our application to ```'./client/index.js'```. The final bundle that webpack will generate will be in the **dist** folder with the filename of ```bundle.js```.

 ```publicPath: '/static/'``` tells the **webpack-dev-server** to serve our bundle from this path, i.e. ```/static/```.

<hr>

```js
//app.js
var createError = require('http-errors');
var express = require('express');
var path = require('path');
var cookieParser = require('cookie-parser');
var logger = require('morgan');
var mongoose = require('mongoose');

var indexRouter = require('./routes/index');
var usersRouter = require('./routes/users');


var app = express();

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');

app.use(logger('dev'));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));

if (process.env.NODE_ENV === "development") {
  const webpack = require("webpack");
  const webpackConfig = require("./webpack.config");
  const compiler = webpack(webpackConfig);

  app.use(
    require("webpack-dev-middleware")(compiler, {
      noInfo: true,
      publicPath: webpackConfig.output.publicPath
    })
  );

  app.use(require("webpack-hot-middleware")(compiler));
}

mongoose.connect('mongodb://localhost:3003/myApp', async function (err) {
  console.log('connected ?', err ? false : true);
});


app.use('/', indexRouter);
app.use('/users', usersRouter);



// catch 404 and forward to error handler
app.use(function(req, res, next) {
  next(createError(404));
});

// error handler
app.use(function(err, req, res, next) {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};

  // render the error page
  res.status(err.status || 500);
  res.render('error');
});

module.exports = app;
```
<br><br>


<!-- This is our **app.js** file. You can see that we have done a lot of stuff in it like database connection, error handler, view engine, etc. ejs is the chosen view engine here. -->



```index.js``` in **client** folder would look like this. 

```js
import React from 'react'
import ReactDOM from 'react-dom'

const App = () => {
    return (
        <div>My App</div>
    )
}


ReactDOM.render(<App />, document.getElementById('root'))
```
<br><br>


Now, let's go into the **views** directory. Our React App would mount here. Also, we have to include our bundled files here, i.e., bundle.js and bundle.css.

```js
<!DOCTYPE html>
<html>
  <head>
    <title><%= title %></title>
    <link rel='stylesheet' href='/static/bundle.css' />
  </head>
  <body>
    <div id="root"></div>
    <script src="/static/bundle.js"></script>
  </body>
</html>
```
<br><br>

The request first comes to the ```indexRouter```. So, we go to ```index.js``` in our **routes** folder. It basically renders the **index.ejs** file which we have shown above. 

```js
const express = require('express');
const router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index', { title: 'My App' });
});

module.exports = router;
```

Now, if you ```npm start``` you'll see **My App** on your screen. That's our basic frontend setup.

<br><br>


## Authentication (BackEnd)


* Authentication is the process of confirming a user’s identity. In backend, the server verifies the identity of the user and sends an affirmative response back to the client-side(if true).


* User has its credentials saved in the database when he tries to register, in the pre-save hook/function. After that password is hashed using npm’s bcrypt package.

```js
userSchema.pre('save', function(next) {
  if (this.password) {
    this.password = bcrypt.hashSync(this.password, 10); // this refers to the user object
    next();
  }
}

userSchema.methods.confirmPassword = function(password) {
  return bcrypt.compareSync(password, this.password);
};
```

* In client side, we have a login form; user enters his credentials such as email and password. When the user submits the button, a request goes to the server and pass through the middlewares and then to the routers that will handle the request.


* Then, we find the user using email or username, and then the validatePassword function gets called which compares the entered password to the already hashed password in the database. If it returns true, a JSON web token is generated using jwt.sign and given to the user. In case of a session, a particular cookie for the user is generated with the session id to later identify the user.


* We can store token in the localStorage or in the redux store itself.


* Now, if the user tries to access some protected routes, say, admin route, the authtoken is send via headers, and after that a middleware calls a function in the server-side called verifyToken which checks if there’s a token in the headers. It’s important to know, a token comes with endcodes payload, signature and a secret. verifytoken decodes the userId from the token and thus verifies the user. Finally, a response is send back.

```js
var jwt = require('jsonwebtoken');

function generateToken(payload) {
  return jwt.sign(payload, 'abcdef');
}

function verifyToken(req, res, next) {
  var token = req.headers.authorization || '';
  if (token) {
    jwt.verify(token, 'secret', (err, decoded) => {
      if (err) res.json({ token: 'Token Not Matched' });
      next();
    });
  } else {
    return res.json({ token: 'Token Not Found' });
  }
}
```

#[Redux](http://localhost:8000/State%20Management%20with%20Redux/)





























