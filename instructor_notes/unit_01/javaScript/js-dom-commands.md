---
title: The DOM and JS in the Browser
type: lesson
duration: "1:30"
creator:
  name: Colin Hart
edited: 
  name: Maren Woodruff
competencies: Programming
---

## Learning Objectives

* Access elements in the DOM using Vanilla JS
* Add and remove elements to the DOM using Vanilla JS
* Manipulate existing elements in the DOM using Vanilla JS
* Conceptualize load order in the Browser and how we account for it

<br />

## Document Object Model (5m)

The [**D**ocument **O**bject **M**odel](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction) is the in browser representation of your HTML document.  And manipulating the DOM is like taking a recipe and making it your own.

The DOM makes the HTML available for us to manipulate, and this object is structured like a tree:

Like this:

![](http://www.tuxradar.com/files/LXF118.tut_grease.diagram.png)

Or this:

```
html
└── head
│   ├──title
│   ├──meta
│   ├──link[rel="stylesheet"]
|   └──script[type="text/javascript"]
|
└── body
    ├── header
    │   ├── h1
    │   └── nav
    └── section.simplicity
    |   └── h2
    │   └── article
    ├── section.life
    |   └── h2
    │   └── article
    │       └── block_quote
    │       └── block_quote
    └── footer
```

Let's create a web page and begin to inspect its structure.

<br />

![](http://i.imgur.com/ylb6WX9.gif)

- Open your terminal
- `cd` into your `ga` folder
  - (`cd ~/Desktop/ga`)
- Create a new directory called `dom-intro-lesson`
  - (`mkdir dom-intro-lesson`)
- `cd` into it and create a new file called `index.html`
  - `cd dom-intro-lesson`
  - `touch index.html` 
- Open the file in Sublime
  - `subl .`
- Copy this code into the file then open it in Chrome:
  - to open in Chrome, right click on the html page and, go down to open in browser

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Sample HTML5 Page</title>
    <meta name="description" content="Sample HTML Page">
  </head>

  <body>
    <section id="main">
      <h1>The DOM Rocks!</h1>
      <p>When this document gets loaded into the browser's memory, it is transformed from a static HTML document to a dynamic DOM tree. At this point we can use JavaScript to inspect and manipulate the nodes in the DOM tree.</p>
    </section>
  </body>
</html>
```

Here is the DOM tree for the above HTML document:

![DOM Tree](https://i.imgur.com/8goR1EO.png)

<br />

### Everything is a Node
In the HTML DOM (Document Object Model), everything is a node:

* The document itself is a document node
* All HTML elements are element nodes
* All HTML attributes are attribute nodes
* Text inside HTML elements are text nodes
* Comments are comment nodes

<br />

![YOU DO](http://i.imgur.com/ylb6WX9.gif)

### Doc Dive (15m)
Each group researches the methods they were assigned on MDN. 

Each group will research and test each method in the Chrome developer tools and write a brief explanation about what each method does. 

We will then come back together as class. Each group will briefly demo and explain what each method does. 

* **Search** 
  * document.getElementById
  * document.getElementsByClassName
  * document.getElementsByName
  * document.querySelector
* **Creation** 
  * document.createElement
  * node.style
* **Traversal** 
  * node.childNodes
  * node.children
  * node.firstChild
* **DOM editing** 
  * node.appendChild
  * node.removeChild
  * node.innerText
  * node.setAttribute
* **Node editing** 
  * node.innerHTML
  * node.id
  * node.classList

<br />

## What are the ways that we can add JS to our HTML page?

### Inside the `<head>` tag

We can run JS inside of `<script></script>` tags inside of our head tags.

### Script tags before the closing `</body>` tag

We can run JS inside of `<script></script>` tags before the closing `</body>`tag of an html document.

### Exercise: (10m) I'll demonstrate this in front of you first, and then you can try on your own.

Open up the HTML file from earlier. At the end of the `<body>` tag write a script tag with the following JS inside of it.
    
```html
<script>
  var response = prompt("Hi! What is your favorite food?");
  alert(`I wish I liked ${response} too. Sometimes, it is sad to be a computer. :( `);  
</script>   
```

To run this code in Chrome, in index.html file, you can right click and scroll to the 'Open in Browser' option.

<br />

### External JS file, similar to how we linked our css

Instead of the `<link>` tag we will keep using our script tag. 

### Exercise: (10m)

1. Touch a file called main.js
2. Let's move the JS code that you wrote inside of the script tag in the index.html, to your main.js file. However, we will change it up a bit and add our message to the DOM via a new Element:

```javascript
var response = prompt("Hi! What's your favorite food?");

var newElement = document.createElement("p");
newElement.textContent = `I wish I liked ${response} too. Sometimes, it is sad to be a computer. :( `;
document.body.appendChild(newElement);    
```
    
### Before the closing `body` tag in the `index.html`

We can run JS inside of `<script type="text/javascript" src="main.js"></script>` tags before the closing `</body>`tag of an html document.  These script tags would point to an external JS file called `main.js`.  Why is this the best option?

### Exercise (5m)
    
```html
<script src='main.js'></script>
```

This is going to load our main.js file into the browser.

<br />

### Load Order

Here's what happens when a browser loads a website:

1. It makes a request for and fetches the HTML page (e.g. index.html)
2. It starts parsing the HTML, i.e. building the dom.
3. If the script tag is in the head of your html document, the parser sees a `<script>` tag referencing an external script file.
4. The browser makes a second request for the script file. Meanwhile, the parser stops and and waits. This is called **Blocking**.
5. Once the script is downloaded and executed, the parser continues parsing the rest of the HTML document.

There are several advanced techniques that load our JS, but for now we can just make sure that our script tag is at the end of our html so that the DOM loads before our script runs.  You should therefore make sure to put the script tags just before your closing body tag.

<br />

### window.onload

There is a pattern we can follow to help our page load properly, and execute it in the right order.  Although we should still keep the script tags just before the closing body tag.

We can surround our JavaScript in a function called `window.onload = function() {}`. This function will wait until the entire window/dom is loaded before allowing any of our JavaScript to run.

In your `main.js` file, wrap your JavaScript code in the following function:

```js
window.onload = function() {
  var response = prompt("Hi! What's your favorite food?");

  var newElement = document.createElement("p");
  newElement.textContent = `I wish I liked ${response} too. Sometimes, it is sad to be a computer. :( `;
  document.body.appendChild(newElement); 
}
```

Refresh your window and make sure your script is still running!

<br />

<!-- If you have time, you can add the lab back in, but usually the class exercise/presentations take a few minutes -->
<!-- ---

![Labtime](http://i.imgur.com/WzTTdIe.jpg)

## Independent Practice

##### Exercise #1 - GA DOM Mod

[GA Dom Instructions (using Vanilla Javascript)](https://github.com/ATL-WDI-Curriculum/atl-wdi-10/tree/master/labs/unit_01/javaScript/ga-dom.md)

<br /> -->

---

## References

* [DOM Reference](https://developer.mozilla.org/en-US/docs/DOM/DOM_Reference)
* [DOM CheatSheet](http://christianheilmann.com/stuff/JavaScript-DOM-Cheatsheet.pdf)

---

### Some of my favorite JS books
* [A Smarter Way to Learn JS](http://www.cpp.edu/~jcmcgarvey/513_2016/ASmarterWaytoLearnJavaScript.pdf)
* [A Smarter Way to Learn jQuery](https://github.com/JideLambo/javascript-books/blob/master/A%20Smarter%20Way%20to%20Learn%20jQuery%20-%20Mark%20Myers.pdf)
* [JavaScript + JQuery](https://www.dropbox.com/s/05je29f3oxj7oa0/JavaScript%20and%20JQuery%20Interactive%20Front-End%20Web%20Development%202014.pdf?dl=0) - So good! Might be worth buying...
* [JavaScript the Good Parts](http://bdcampbell.net/javascript/book/javascript_the_good_parts.pdf)
* [Eloquent JS](http://eloquentjavascript.net/)
* [DOM Enlightenment](http://domenlightenment.com/#1.1)
* [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS)
* [You Don't Know ES6](https://github.com/getify/You-Dont-Know-JS/tree/master/es6%20%26%20beyond)
