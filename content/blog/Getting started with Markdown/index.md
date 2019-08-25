---
title: 'Getting Started with Markdown'
tags: ["javascript"]
published: true
date: '2019-08-17'
---

![](https://cdn.guidingtech.com/media/assets/WordPress-Import/2014/01/markdown-logo2-300x201.png)

## What is Markdown ?

Markdown is a lightweight markup language that is used to format plaintext files and convert them into proper HTML or XHTML documents. It was created by **John Gruber** and the legendary [**Aaron Swartz**](https://en.wikipedia.org/wiki/Aaron_Swartz), with the goal of enabling people "to write using an easy-to-read and easy-to-write plain text format".

You might know about Microsoft Word, which itself is a text formatting tool used to create documents. Unlike MS Word, Markdown doesn't offer any fancy things like the **B** button or *I* , instead, what we have here is the quirky ***Markdown syntax***.

## How does it work ?

You don't need to know a lot of nerdy stuff to understand the language, however, it's better to know, how things are happening behind the scenes. 

> What's happening is that our .md file is being converted into an HTML file, right ? But how ? Well, there's a Markdown processor that processes or parse that .md file and finally outputs a properly formatted HTML document.

Markdown files have a file extension of .md. You might've come across  **readme** file in a Github repository; Well, Yes! That's the markdown file. Not just Github, markdown is also being used in:

#### `* Static site generators.`

#### `* Websites like Bitbucket, Slack, Reddit, Discord, and many more.`

#### `* Writing Blog posts.`

#### `* Providing Documentation.`

<br>

## Markdown Cheat Sheet

Let's take a closer look at the Markdown syntax and see for yourself, how easy and useful it is.


<br>

### Italics

To italicize a piece of text, either use * or _ 

`*This text* is italic` => *This text* is italic.

`_This text_ is italic` => *This text* is italic.

<br>

### Strong

Use either ** or __ to create bold text.

`This text is **strong**` => This text is **strong**.

`This text is __strong__` => This text is **strong**.

<br>

### Strikethrough

Use ~~ to create strikethrough text.

`~~This text~~ is strikethrough` => ~~This text~~ is strikethrough.

<br>

### Blockquote

Use > to make your own blockquote.

> This is a quote

<br>

### Horizontal Rule

You could either use --- or ___ to create an hr.

---
___

<br>

### Links

`[Google](http://www.google.com)` => [Google](http://www.google.com)


`[Google](http://www.google.com "Google Homepage")` => [Google](http://www.google.com "Google Homepage")

<br>

### Images

Markdown even lets you embed images. Use an exclamation mark followed by the address of the image.

Here's the fallout boy for you.


`![Fallout Boy Image](https://qph.fs.quoracdn.net/main-qimg-43080ade882169211fc1ea739644ea74)`
 ![Fallout Boy Image](https://qph.fs.quoracdn.net/main-qimg-43080ade882169211fc1ea739644ea74)


### Code Blocks

It's one of the cool features of markdown. Many popular chatrooms or discussion forums like Slack, Discord and Reddit use this feature extensively.

```javascript
function add(a, b) {
    return a + b;
}
```
<br>

### Tables

Creating a table is a tedious task, even in HTML because you have to go through numerous td and tr kind of stuff. Don't worry, Markdown has got your back!

```
| Name         | Email               |
| -------------| ------------------- |
|  Cartman     | e_cartman@gmail.com |
|  Kenny       | kenny12@gmail.com   |
```

You'll get something like this:

| Name       | Email               |
| -----------| ------------------- |
|  Cartman   | e_cartman@gmail.com |
|  Kenny     | kenny12@gmail.com   |

<br>

###  Headings

Headings work the same way as we use tags like heading tags in HTML. But here, you just have to use a **#** sign to specify the heading type. 

```
# Heading 1
## Heading 2
### Heading 3
```
<br>

# Heading 1
## Heading 2
### Heading 3

<br><br>


---

*Thanks for reading. And yes, this blog post has been written in Markdown, as well.*





















