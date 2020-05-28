# Week 7 Goals

#### By the end of the week all developers can:

- Build a front-end app in Javascript
- Work competently in Javascript
- Reason about asynchronous behaviour in Javascript

This week is analogous to week 2 in that developers will be wrestling with some underlying language concepts that are not well understood (by Makers).

Most of the work and interventions we have run historically are meant to 'de-magic' Javascript and force developers to confront the pieces they are glossing over in an attempt to get work done.

## Daily Goals
### Tuesday 26 of May 2020

## Morning Goals

- Code Review for the weekend **Bowling** challenge.
- Attend week 7 *kick off*.
- Set a working plan for the new week

##### Plan:

- Review weekend challenge in pair. Will be reviewing Jo's code.
- Set goals for the new week

### Weekly Morning Goals

- Events and event handlers.
- Manipulating the Document Object Model (DOM).
- Ajax.
- Frontend templating.
- Frontend routing.

#### Explain events and event handlers.

**Plan:**

- Perform research online individually.  
- Describe what **events and event handlers are** and it's usages.
- Summarize and give some practical example.

**Process:**

#### Events

In programming, an event is an action that occurs as a result of the user or another source, such as a mouse click. An event handler is a routine that deals with the event, allowing a programmer to write code that will be executed when the event occurs.

#### Registering onevent handlers

The onevent handlers are properties on certain DOM elements to manage how that element reacts to events. Elements can be interactive (links, buttons, images, forms, and so forth) or non-interactive (such as the base <body> element). HTML Events are actions like:

- Web page has finished loading
- An input field was changed
- Being clicked
- Detecting pressed keys
- Getting focus

The on-event handler is usually named with the event it reacts to, like onclick, onkeypress, onfocus, etc.

#### Events on the Web

Another example of an event is a user clicking on a button within a web page. This action creates what is known as a "click" event. JavaScript could then be used to program a reaction to the event, for instance, you could use the onclick event handler shown in the following box:
```
<form action="#" method="post">
<input type="button" value="Click for a Message" onclick="window.alert('Hello!');">
</form>
```
Or by setting the corresponding property from JavaScript:
```
document.querySelector("button").onclick = function(event) { … }.
```

**What I've Learned:**

> The Web platform provides several ways to be notified of DOM events. Two common approaches are addEventListener() and the specific onevent handlers.  
> HTML events are "things" that happen to HTML elements.  
> When JavaScript is used in HTML pages, JavaScript can "react" on these events.

## Afternoon Challenges  

*Practice pairing and building Web-app.*  
[**"Notes App"**](https://github.com/EdAncerys/Notes-App)

**Plan:** Pair with Dec, Marja and Catriona and keep working on the afternoon challenge for the week - *"Notes App".*

**Process:**

**What I've Learned:**












## Daily Goals
### Wednesday 27 of May 2020

## Morning Goals

#### Explain the **JavaScript module pattern**.

**Plan:**

- Perform research online individually.  
- Describe what **JavaScript module pattern is** and it's usages.
- Summarize and give some practical example.

**Process:**

# JavaScript module pattern

A design pattern to encapsulate your JavaScript code.

## Immediately Invoked Function Expression (IIFE)

Let's start by talking about a part of the module pattern: the Immediately Invoked Function Expression (or IIFE, pronounced "iffy").  At its simplest, it looks like this:

```js
function printHi() {
  console.log("hi");
};

printHi();
```

This code does the same thing the IIFE does.  But it's more verbose. And it creates an unnecessary variable, `printHi` that no one needs.

### Why?

It's pointless to wrap just a call to `console.log` in an IIFE.  We use IIFEs to hide variable and function declarations.  Calling `console.log` doesn't declare any variables, so it doesn't need to be wrapped in an IIFE.

Imagine we wanted to formulate a more complex greeting than just `hi`.

```js
(function () {
  var EXCLAMATION_MARK_COUNT = 5

  function exclaim(string) {
    return string + "!".repeat(EXCLAMATION_MARK_COUNT);
  };

  console.log(exclaim("hi"));
})();
```

Using an IIFE to wrap all this code keeps the details of creating the greeting private. It means none of the rest of the code can access any variables or functions inside the IIFE.  Look what happens when we try to use `exclaim` and `EXCLAMATION_MARK_COUNT` outside the IIFE:

```js
(function () {
  var EXCLAMATION_MARK_COUNT = 5

  function exclaim(string) {
    return string + "!".repeat(EXCLAMATION_MARK_COUNT);
  };
})();

// throws a ReferenceError
exclaim("hi");

// would throw a ReferenceError, if not for the ReferenceError thrown
// by the previous line
console.log(EXCLAMATION_MARK_COUNT);
```

## Using the module pattern in the browser

The module pattern is basically just an IIFE.  But it uses a bit of extra code to export (or expose, or make available to the outside, or show) functions and variables that are part of the public interface of the module.

Here is a tiny module:

```js
(function(exports) {
  var EXCLAMATION_MARK_COUNT = 5

  function exclaim(string) {
    return string + "!".repeat(EXCLAMATION_MARK_COUNT);
  };

  exports.exclaim = exclaim;
})(this);
```

To explore this code, let's try and use the exclaim function:

```js
(function(exports) {
  var EXCLAMATION_MARK_COUNT = 5

  function exclaim(string) {
    return string + "!".repeat(EXCLAMATION_MARK_COUNT);
  };

  exports.exclaim = exclaim;
})(this);

// prints "hi!!!!!"
console.log(exclaim("hi"));

// throws a ReferenceError
console.log(EXCLAMATION_MARK_COUNT);
```

So we can access `exclaim`, but `EXCLAMATION_MARK_COUNT` is hidden. We've made available the function we want people to use, but hidden some implementation details that we don't want to bother them with.  We've also prevented our hidden variables from clashing with variables in the rest of our program.  For example, we could have two modules, each of which define and keep private their own `EXCLAMATION_MARK_COUNT`.

### How does `exports` work?

How have we made `exclaim` available? Grab the code and paste it into your browser console.

Use `console.log` to print the data stored in variables like `this` and `exports`.  Follow the flow of this data through the program.  Some points to help you:

* Notice how the value of `this` is `window`, the place where all globals are stored.

### What if one module needs to require another module?

In the rewrite below, the programmer wants to abstract out the code that can repeat the exclamation marks.  They create a new module, `repeat`.

```js
// repeat.js

(function(exports) {
  function repeat(string, count) {
    return string.repeat(count);
  };

  exports.repeat = repeat;
})(this);
```

```js
// exclaim.js

(function(exports) {
  var EXCLAMATION_MARK_COUNT = 5;

  function exclaim(string) {
    return string + repeat("!", EXCLAMATION_MARK_COUNT);
  };

  exports.exclaim = exclaim;
})(this);
```

This works great.  `repeat` is available because it's a global variable, so all is well.

### A module that doesn't include any other modules

This version of `exclaim` contains the repeat functionality.

```js
(function(exports) {
  var EXCLAMATION_MARK_COUNT = 5

  function exclaim(string) {
    return string + "!".repeat(EXCLAMATION_MARK_COUNT);
  };

  exports.exclaim = exclaim;
})(this);
```

This version that we wrote for the browser works perfectly, without any changes, in Node.js.  Look:

```js
// exclaim-test.js

var exclaim = require("./exclaim").exclaim;

if (exclaim("hi") !== "hi!!!!!") {
  throw new Error("Exclaiming hi should equal hi!!!!!");
} else {
  console.log(".");
}
```

### A module that requires other modules

This version of the `exclaim` module breaks out the `repeat` functionality into a separate module.

```js
// repeat.js

(function(exports) {
  function repeat(string, count) {
    return string.repeat(count);
  };

  exports.repeat = repeat;
})(this);
```

```js
// exclaim.js

(function(exports) {
  var EXCLAMATION_MARK_COUNT = 5;

  function exclaim(string) {
    return string + repeat("!", EXCLAMATION_MARK_COUNT);
  };

  exports.exclaim = exclaim;
})(this);
```

This version of the `exclaim` module won't work in Node.js.  The behaviour of the browser that makes `repeat` available as a global doesn't work in Node.js

Here is one way we can rewrite `exclaim` to support Node.js and the browser.  There are other ways, but this one is simple and clear.

```js
// exclaim.js

(function(exports) {
  var EXCLAMATION_MARK_COUNT = 5;

  function exclaim(repeat, string) {
    return string + repeat("!", EXCLAMATION_MARK_COUNT);
  };

  exports.exclaim = exclaim;
})(this);
```

We have injected the `repeat` function into `exclaim`.  To support this, here are the slightly rewritten `app.js` and Node.js test file `exclaim-test.js`:

```js
// app.js

console.log(exclaim(repeat, "howdy"));
```

```js
// exclaim-test.js

var repeat = require("./repeat").repeat;
var exclaim = require("./exclaim").exclaim;

if (exclaim(repeat, "hi") !== "hi!!!!!") {
  throw new Error("Exclaiming hi should equal hi!!!!!");
} else {
  console.log(".");
}
```

**What I've Learned:**

> JavaScript module pattern is a design pattern to encapsulate your JavaScript code.
This can be done by wrapping function in another function and expose variables we want by doing:

```js
var name = ‘ed’

function(context) {
	var name = ‘notEd’ // Encapsulated variable that can not be accessed outside the function

	function sayGoodMorning(name) {
		return name
	}
	context.sayGoodMorning = sayGoodMorning // context variable & what CAN BE EXPOSED !!!
})(this) // passing to window object

name // returns 'ed'
sayGoodMorning() // returns 'notEd'
```

## Daily Goals
### Thursday 28 of May 2020

## Morning Goals

#### Explain how to **Manipulate the Document Object Model (DOM).**.

**Plan:**

- Perform research online individually.  
- Describe what **Document Object Model (DOM) is** and it's usages.
- Summarize and give some practical example.

**Process:**

The DOM (Document Object Model) is an interface that represents how your HTML and XML documents are read by the browser. It allows a language (JavaScript) to manipulate, structure, and style your website. After the browser reads your HTML document, it creates a representational tree called the Document Object Model and defines how that tree can be accessed.

#### Advantages

By manipulating the DOM, you have infinite possibilities. You can create applications that update the data of the page without needing a refresh. Also, you can create applications that are customisable by the user and then change the layout of the page without a refresh. You can drag, move, and delete elements.

#### Most common elements

- Document: It treats all the HTML documents.
- Elements: All the tags that are inside your HTML or XML turn into a DOM element.
- Text: All the tags’ content.
- Attributes: All the attributes from a specific HTML element. In the image, the attribute class=”hero” is an attribute from the <p> element.

### Manipulating the DOM

#### Methods

The DOM has a lot of methods. They are the connection between our nodes (elements) and events.

getElementById()

This method returns the element that contains the name id passed. As we know, id’s should be unique, so it’s a very helpful method to get only the element you want.

start: id passed. And in case we do not have any id with that specific name, it returns null.
```js
var myStart = document.getElementsById('start');
myStart: Our variable name that looks similar to our id passed.
```

getElementsByClassName()

This method returns an HTMLCollection of all those elements containing the specific name class passed.

.container: our class passed. In case we do not have any class with that specific name, it returns null.
```js
var myContainer = document.getElementsByClassName('container');
myContainer: Our variable name that looks similar to our class passed.
```

getElementsByTagName()

This works the same way as those methods above: it also returns an HTMLCollection, but this time with a single difference: it returns all those elements with the tag name passed.

button: tag name that we want to get.
```js
var buttons = document.getElementsByTagName('button');
buttons: Our variable name that looks similar to our tag name passed.
```

querySelector()

It returns the first element that has the specific CSS selector passed.
```js
var resetButton = document.querySelector('#reset');
resetButton: Our variable name that looks similar to our selector passed.

#reset: selector passed, and if you don’t have any selector that matches it returns null.
```
querySelectorAll()

Very similar to the querySelector() method, but with a single difference: it returns all the elements that match with the CSS selector passed. The selector should also follow the CSS syntax. In case you do not have any selector, it returns null.
```js
var myButtons = document.querySelector('#buttons');
myButtons: Our variable name that looks similar to our selectors passed.

#buttons: selector passed, if you don’t have any selector that matches it returns null.
```
createElement()

Creates a new element in your HTML page
```js
var p_element = document.createElement('p');
```

setAttribute()

Lets you set new attributes for elements HTML.
```js
p_element.setAttribute('id', 'note-body')
```

### Events

The DOM elements have methods, but they also have events. These events are responsible for make interactivity possible in our page. **Events are also methods.**

`click`

One of the most used events is the click event. When the user clicks on a specific element, it will realise some action.
```js
myStart.addEventListener('click', function(event) {
// Do something here.
}, false);
```
The addEventListener() parameters are:

- The type of the event that you want (in this case ‘click’).
- A callback function
- The useCapture by default is false. It indicates whether or not you want to “capture” the event.

`select`

This events is for when you want to dispatch something when a specific element is selected. In that case we’re gonna dispatch a simple alert.
```js
myStart.addEventListener('select', function(event) {
alert('Element selected!');
}, false);
```
These are some of the most commonly used events.

**What I've Learned:**

> When a web page is loaded, the browser creates a Document Object Model of the page, which is an object oriented representation of an HTML document, that acts as an interface between JavaScript and the document itself and allows the creation of dynamic web pages.  

> By manipulating the DOM, you have infinite possibilities. You can create applications that update the data of the page without needing a refresh. Also, you can create applications that are customizable by the user and then change the layout of the page without a refresh.