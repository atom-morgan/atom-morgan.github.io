---
title: 'Code Companion #9: DOM Manipulation'
date: '2018-07-19'
layout: post
tag: [tutorial, cc]
excerpt: >-
  In this tutorial we're going to begin working with HTML to add some interactivity to our programs with a UI.
image: '/images/dom-manipulation/tutorial-cover.png'
thumb_image: '/images/dom-manipulation/tutorial-cover.png'
style: code
---

## Video

<div class="videoWrapper">
  <iframe width="560" height="315" src="https://www.youtube.com/embed/p8ytrJ1ryBQ" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
</div>

## Introduction

In this tutorial we're going to begin making our programs a bit more web-like. Up until this point we've been writing JavaScript and logging the results of our programs to a terminal or browser console using `console.log()`.

Now we're going to begin working with HTML to add some interactivity to our programs with a UI.

## Setup

First we'll make a new folder for our project.

```console
mkdir dom-manipulation
cd dom-manipulation
```

Within this directory we'll create two new files, an HTML file and a JavaScript file similar to what we did way back in [Code Companion #2](https://atom-morgan.github.io/hello-world-in-javascript/){:target="_blank"}.

```console
touch main.js index.html
```

{% include request-tutorial.html %}

## Event Listener

Our goal for this application is to present the user a button that when clicked displays a prompt to the user for input. Once the user enters some text, we'll then add that text to the HTML to display it back to the them.

First we'll start with a simple HTML page.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Code Companion - DOM Manipulation</title>
    <meta charset="utf-8">
    <script src="main.js"></script>
  </head>

  <body>

  </body>
</html>
```

The noteworthy part here is the `script` element (sometimes referred to as tags) with the `src` attribute set to the `main.js` file we created. Unlike our `script` tag in [Code Companion #2](https://atom-morgan.github.io/hello-world-in-javascript/){:target="_blank"}, we'll be writing our JavaScript in a separate file.

<div class="box" markdown="1">
If you want a description of the various elements in our HTML page such as `html`, `head`, and `<!DOCTYPE html>`, the Mozilla Developer Network's [Anatomy of an HTML Document](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/HTML_basics#Anatomy_of_an_HTML_document){:target="_blank"} has a great list describing each of these in detail.
</div>

To verify everything is working correctly, we can update `main.js` with a "Hello, world" using `console.log()`.

```javascript
console.log('Hello, world!');
```

Save these files, open `index.html` in Chrome, and you should see `Hello, world` logged within the console of Chrome's developer tools. You can access this by going to View > Developer > JavaScript Console or use the keyboard shortcut `Cmd + Alt + J`.

![HTML and JavaScript, hello world](/images/dom-manipulation/hello-world.png)

Now that our HTML and JavaScript file are linked together, we can add a button to the page for the user to click which will display the prompt.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Code Companion - DOM Manipulation</title>
    <meta charset="utf-8">
    <script src="main.js"></script>
  </head>

  <body>
    <button id="displayPromptButton">Click here!</button>
  </body>
</html>
```

Here we've added a `button` element with its `id` attribute set to an id name of our choice. In this case it's `displayPromptButton`. But at the moment, our button doesn't do anything. To fix this, we'll need to use an "event listener" to do something once the button is clicked.

Within `main.js` add the following code.

```javascript
let displayPromptButton = document.getElementById('displayPromptButton');

displayPromptButton.addEventListener('click', function() {
  console.log('clicked!');
});
```

First we declare a variable `displayPromptButton` which is an object that represents the button within our HTML. We create this object by using the `getElementById` function which lets us find an element within our HTML page's DOM or Document Object Model.

The DOM is an application programming interface (API) which allows us to interact with our HTML as an object.

We access the DOM using `document` which represents our document or HTML. As with other objects we've seen, this `document` object has methods exposed to us like `getElementById()` which allows us to provide it an id. This returns us an object representing that specific element.

```javascript
let displayPromptButton = document.getElementById('displayPromptButton');
```

So this line accesses the DOM for our button (via its id) and sets it to our local `displayPromptButton` variable.

After that, we use our new object to create our event listener.

```javascript
displayPromptButton.addEventListener('click', function() {
  console.log('clicked!');
});
```

The `addEventListener` method takes two arguments. The first is the type of event to listen for and the second is a function that will run when this event occurs. So once the button is clicked it will log `clicked` to the console.

Open the file within Chrome again (or refresh if it's already open) and you should see an error related to our event listener.

![Event listener error](/images/dom-manipulation/event-listener-null.png)

The reason for this is that when our `main.js` file is loaded (which contains the event listener) the content within our document hasn't finished loading yet. As a result, the element with the id `displayPromptButton` doesn't exist yet which return us `null`. When `addEventListener` is called on `null`, we get the error we see above.

To fix this we'll need to add *another* event listener to `main.js`.

```javascript
document.addEventListener('DOMContentLoaded', function() {
  let displayPromptButton = document.getElementById('displayPromptButton');

  displayPromptButton.addEventListener('click', function() {
    console.log('clicked!');
  });
});
```

Here we've updated our previous code by adding a new event listener for `DOMContentLoaded` which, as you may be able to guess, listens for when the DOM content is loaded. Within this event listener we add our previous code so that it doesn't execute until the `DOMContentLoaded` event has fired.

Go back to your browser, click the button, and you should now see `clicked!` logged to the console.

![Button click event](/images/dom-manipulation/event-listener-working.png)

## User Prompt

Now that our event listener is working correctly we can move on to adding a prompt for a user's input.

```javascript
document.addEventListener('DOMContentLoaded', function() {
  let displayPromptButton = document.getElementById('displayPromptButton');

  displayPromptButton.addEventListener('click', function() {
    let input = prompt('What is your favorite Pokemon?');
    console.log('input is ', input);
  });
});
```

First we remove the previous `console.log()`. In its place we use the `prompt()` function which [prompts the user for input](https://developer.mozilla.org/en-US/docs/Web/API/Window/prompt){:target="_blank"}. We pass it a string containing a message we want displayed to the user within the prompt and set its return value to the variable `input`. Then we `console.log()` the user's input.

Go to your browser, click the button, and you should see the prompt displayed to you asking for input. Enter some text, click OK, and you should see your input logged to the console.

![Prompt window](/images/dom-manipulation/prompt-window.png)

![Prompt result](/images/dom-manipulation/prompt-result.png)

## Add to DOM

Now that we're successfully getting input from a user, we can work towards displaying this within the DOM. First, we'll add a new `p` element to `body` with an `id` set to `pokemon`.

```html
<body>
  <button id="displayPromptButton">Click here!</button>
  <p id="pokemon"></p>
</body>
```

Then we update `main.js` to add the input to this new element.

```javascript
document.addEventListener('DOMContentLoaded', function() {
  let displayPromptButton = document.getElementById('displayPromptButton');
  let view = document.getElementById('pokemon');

  displayPromptButton.addEventListener('click', function() {
    let input = prompt('What is your favorite Pokemon?');
    view.innerHTML = input;
  });
});
```

First we create another object, `view`, using `document.getElementById()` once again to access our new element. Then within the event handler for `click` we access the `innerHTML` property of view and set it to `input`. (A full list of element properties can be found [here](https://developer.mozilla.org/en-US/docs/Web/API/Element){:target="_blank"}.)

{% include discord.html %}

Go back to your browser, enter text into the prompt, and you should see the result added to the page.

![Prompt result](/images/dom-manipulation/innerhtml.png)

## List of Pokemon

Now that we can successfully add a user's input to the DOM, we're going to update the HTML removing the `p` tag in favor of `ul` so that we can have a list of multiple Pokemon instead.

```html
<body>
  <button id="displayPromptButton">Click here!</button>
  <ul id="pokemon"></ul>
</body>
```

Then we'll update `main.js` for this new addition.

```javascript
let displayPromptButton = document.getElementById('displayPromptButton');
let view = document.getElementById('pokemon');

displayPromptButton.addEventListener('click', function() {
  let input = prompt('What is your favorite Pokemon?');
  let listItem = document.createElement('li');
  listItem.innerHTML = input;
  view.appendChild(listItem);
});
```

First we use `document.createElement()` to create a new `li` element for an individual item within the unordered list (`ul`). We then access its `innerHTML` property and set it to `input`. Finally, we call `.appendChild()` to add `listItem` to the end of the unordered list using our `view` object.

Go back to your browser and you should now be able to add multiple Pokemon to the view.

![Unordered list](/images/dom-manipulation/unordered-list.png)

Before wrapping up we'll make one last change to our code by moving some of our functionality into a function.

```javascript
function addListItem(input, target) {
  let listItem = document.createElement('li');
  listItem.innerHTML = input;
  target.appendChild(listItem);
}

document.addEventListener('DOMContentLoaded', function() {
  let displayPromptButton = document.getElementById('displayPromptButton');
  let view = document.getElementById('pokemon');

  displayPromptButton.addEventListener('click', function() {
    let input = prompt('What is your favorite Pokemon?');
    addListItem(input, view);
  });
});
```

Here we've defined a function, `addListItem`, that takes an `input` and `target` for where to add the input. Within this function we added the code for creating a new `li` element, adding the user's input to the element, and appending it to the target DOM object.

## Conclusion

Before we finish, add your latest changes to Git.

```console
git init
git add .
```

Then add a commit message.

```console
git commit -m "Add dom manipulation"
```

Then push these changes up to GitHub.

```console
git push origin master
```

## Exercise

For some extra practice, see what happens when you click "Cancel" within the prompt or click "OK" without entering any text.

See if you can fix this by adding some updates to `main.js` to avoid adding empty list elements to the DOM.

{% include book-plug.html %}
