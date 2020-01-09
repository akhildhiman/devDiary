---
title: 'Scheduling Cron Jobs in Node.js'
tags: ["Node"]
published: true
date: '2020-01-08'
---

![](https://i.imgur.com/gT85SPn.jpg)


Cron jobs in node.js come in handy whenever there's a need to run scripts on the server over and over again at certain intervals. It could be any task, such as sending email to the user at any particular time or day. In this article, we're going to test that with the help of [nodemailer](https://nodemailer.com/about/).

<br>

To begin with, we will create a node application with the following commands:

```
mkdir cron-jobs
npm init -y
```
<br><br>

Now, we need to install ```node-cron``` and ```nodemailer``` packages from npm. Create a file called ```index.js``` as the entry point of the application or simply, our server file.


```js
npm install node-cron 
npm install nodemailer
touch index.js
```
<br>

```js
//index.js
const express = require("express")
const cron = require("node-cron")
const nodemailer = require("nodemailer")
const app = express()

app.listen(8000)
```
<br><br>

Before setting up the cron job, let's first configure nodemailer. 

```js
let transporter = nodemailer.createTransport({
    service: "gmail",
    auth: {
        user: "email@gmail.com",
        pass: "password"
    }
})

let mailOptions = {
    from: "email@gmail.com",
    to: "receiveremail@gmail.com",
    subject: "Nodemailer",
    text: "Testing Nodemailer",
    html: "<h1>Testing Nodemailer</h1>"
}

transporter.sendMail(mailOptions, (err, info) => {
    if(err) {
        console.log("error occurred", err)
    } else {
        console.log("email sent", info)
    }
})
```
<br>

* **_transporter_** is an object that holds the email service we are using, an auth object having the sender's email and password. 
* **_mailOptions_** contains standard email info. We can also use templates such as ejs or hbs.
* **_sendMail_** method takes in mailOptions and a callback. 

<br>

> It's important to note that we're using gmail as the service. In order to use it, we will have to turn on the less secure app feature.

<br>


Cron's schedule method takes in:

* Time interval at which the cron job will run.
* Callback function which runs after the message is sent.

<br>


The asterisks in ```cron.schedule``` refer to the time interval at which the code will get executed. We can set the time up as described in the format below:


```
┌──────────────── second (optional) 
| ┌────────────── minute 
| | ┌──────────── hour 
| | | ┌────────── day of month 
| | | | ┌──────── month 
| | | | | ┌────── day of week
| | | | | | 
| | | | | |
* * * * * *
```
<br>



```js
//For a cron job to run every second
cron.schedule("* * * * * *", () => {
    //code to be executed
})

//This will run every 10 seconds
cron.schedule("*/10 * * * * *", () => {
    //code to be executed
})

//This will run at the start of every minute
cron.schedule("0 * * * * *", () => {
    //code to be executed
})

//This will run at the start of every hour
cron.schedule("0 * * * *", () => {
    //code to be executed
})

// This will run on 20th of every month at 02:00 hours
cron.schedule("* 02 20 * *", () => {
    //code to be executed
})
``` 
<br><br>

## Setting up the cron job with nodemailer

Final code would look like this:



```js

const express = require("express")
const cron = require("node-cron")
const nodemailer = require("nodemailer")
const app = express()

let transporter = nodemailer.createTransport({
    service: "gmail",
    auth: {
        user: "email@gmail.com",
        pass: "password"
    }
})

// Here, we're scheduling a cron job and it will send an email at the start of every minute.
// Info contains the mail content.
// In case of sending mail to multiple users, we can add multiple recipients.
cron.schedule("* * * * *", () => {
    console.log("sending email")
    let mailOptions = {
        from: "email@gmail.com",
        to: "receiveremail@gmail.com",
        subject: "Nodemailer",
        text: "Testing Nodemailer",
        html: "<h1>Testing Nodemailer</h1>"
}

transporter.sendMail(mailOptions, (err, info) => {
    if (err) {
        console.log("error occurred", err)
    } else {
        console.log("email sent", info)
    }
  })
})

app.listen(8000)

```
<br><br>
Finally, head over to your terminal and start the server. 

```js
node index.js
```


<br><br>

Thank You for reading this article. Follow me on [Twitter](https://twitter.com/_himalayan_) for more updates.
















































