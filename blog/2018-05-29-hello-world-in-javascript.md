---
title: 'Code Companion #2: "Hello, world" in JavaScript'
date: '2018-05-29'
layout: post
tag: [tutorial, cc]
excerpt: >-
  Learn web development with an introduction to JavaScript by creating three variations of the classic "Hello, world" program.
image: "/images/hello-world/tutorial-hello-world.png"
thumb_image: "/images/hello-world/tutorial-hello-world.png"
style: code
---

## Video

<div class="videoWrapper">
<iframe width="560" height="315" src="https://www.youtube.com/embed/Llze9Pq76sE" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
</div>

## Introduction

In this tutorial we'll take our first glance at web development by creating three variations of the classic ["Hello, world" program](https://en.wikipedia.org/wiki/%22Hello,_World!%22_program){:target="_blank"} using JavaScript. First we'll do this in a web browser. Then we'll create the same program using what's known as a REPL. Finally, we'll wrap up by writing the program in a JavaScript file.

Once you've finished, you'll have three different methods at your disposal to start writing JavaScript that we'll continue to explore in future tutorials.

{% include discord.html %}

## Setup

Before we get started there's a few things you'll need to download.

### Text editor

One of the first things you'll need before we begin writing code is a text editor to write your code in. Two popular choices at the moment are [Visual Studio Code](https://code.visualstudio.com/){:target="_blank"} and [Sublime Text](https://www.sublimetext.com/3){:target="_blank"}. Either one of these will do just fine—they're largely a matter of personal preference.

Visual Studio Code is 100% free and open-source. Sublime Text is free to download as well but there is an $80 fee for a license which you'll inevitably see in a prompt if you use Sublime Text long enough.

### Terminal

Next you'll need to get a terminal emulator for the command-line work we'll be doing. If you're on Mac, I'd recommend [iTerm2](https://www.iterm2.com/){:target="_blank"} but you're welcome to use the default terminal application provided by Apple.

### Git

The last thing you'll need to do is install [Git](https://git-scm.com/){:target="_blank"}. Git is an open source version control system which allows you to track changes in source code and coordinate working on these files among a team. You'll be using Git to push your code to [Github](https://github.com){:target="_blank"} and to clone (or download) existing code repositories that other developers have created. For now, we'll be using Git and Github to manage the code we write in this tutorial.

<div class="box">
Don't stress too much about Git at the moment. The commands you'll need to use Git will be provided for you in this tutorial. We'll take a closer look at Git in a later tutorial.
</div>

Once you've installed Git you can verify it was installed by running the following command in a terminal window:

```console
git --version
```

![Git version](/images/hello-world/git-version.png)

### Project directory

Now that Git is installed and ready to go, we can create a folder for the code we're about to write. First, create the new directory.

```console
mkdir webdev-intro
```

Then `cd` (which stands for change directory) into the new directory.

```console
cd webdev-intro
```

Now we can create the HTML page for our code using the `touch` command.

```console
touch index.html
```

At this point, we'll initialize our new directory as a Git repository.

```console
git init
```

Then we add our files using `git add .` and commit our changes with a message: "Initialize repository".

```console
git add .
git commit -m "Initialize repository"
```

{% include email-list.html %}

## Web browser

At this point we're ready to take a look at our "Hello, world" program using the `index.html` file we just created. Open that file in a text editor such as Visual Studio Code or Sublime Text and add the following text to the file.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Code Companion - Hello, world</title>
    <meta charset="utf-8">
  </head>
  <body>
    <h1>Hello, world!</h1>
  </body>
</html>
```

This is an extremely basic HTML page which displays a message: "Hello, world!". Open this file in a web browser and you should see the message within the page.

![HTML hello world](/images/hello-world/screenshot-hello-world.png)

At this point there is no JavaScript in our HTML. Let's change this by adding a few more lines of code to `index.html` just below `<meta charset="utf-8">`.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Code Companion - Hello, world</title>
    <meta charset="utf-8">
    <!--start javascript-->
    <script>
      alert('Hello, world!');
    </script>
    <!--end javascript-->
  </head>
  <body>
    <h1>Hello, world!</h1>
  </body>
</html>
```

Now when you view this page in a browser you should see the previous message, "Hello, world!", along with an alert that displays the same message.

![HTML hello world with alert](/images/hello-world/screenshot-hello-world-alert.png)

So how does this work? Let's start by looking at the five lines we just added.

```html
<!--start javascript-->
<script>
  alert('Hello, world!');
</script>
<!--end javascript-->
```

This section of code begins and ends with what's known as an [HTML comment](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Getting_started#HTML_comments){:target="_blank"}. This is a way for us to write text in our code which are both ignored by the browser and invisible to the user. In this case, we've added two comments to show us where our JavaScript code begins and ends.

Following that is our `<script>` element which is what allows us to embed JavaScript code within HTML. We put any JavaScript code we want to execute below `<script>` and above its closing tag `</script>`. In this case, we've added the following code:

```javascript
alert('Hello, world!');
```

This piece of code calls what's known as a JavaScript function. In this case, the name of the function is `alert`.

When we call a function there's a certain structure to how it's called. First there's the function's name followed by an open parenthesis. Afer this open parenthesis are zero or more "arguments" followed by a closing parenthesis and a semicolon to end the line.

![Function anatomy](/images/hello-world/function-anatomy.png)

In our case we began with our function name `alert` followed by an open parenthesis. Then we provided our function a single argument in the form of a [string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String){:target="_blank"}: `Hello, world!`. After the single argument we finish our function call with a closing parenthesis and a semicolon.

We've now written our first program in an HTML page. In the next section, we'll introduce another way to write and run JavaScript code using what's known as a [REPL](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop){:target="_blank"}.

{% include request-tutorial.html %}

## REPL

In our second example of the "Hello, world" program we're going to use an interactive programming environment known as a REPL (pronounced "repple") which stands for read, evaluate, print, and loop. The REPL environment that we'll be using first **reads** our input, then it **evaluates** our input, then it **prints** any results and finally loops back to the first **read** step. Let's see what this looks like in action.

First, open a browser such as Chrome and open the developer tools. You can do this with the keyboard shortcut `Cmd + option + J` or manually through the browser's menu by clicking View > Developer > JavaScript Console.

![Developer tools](/images/hello-world/chrome-dev-tools.png)

Within this window you can now call the `alert` function as we did earlier to see the REPL in action.

![Developer tools](/images/hello-world/chrome-repl.png)

If you'd like to avoid an alert modal every time you call `alert` you can print the results directly into the REPL instead using the function call `console.log()`.

![Developer tools](/images/hello-world/chrome-console-log.png)

<div class="box" markdown="1">
If you're wondering why `console.log()` has a period in the middle while `alert()` is just one word, that's totally normal and it's good you noticed the difference between the two function calls. Since this is an introduction, the difference between the two is out of scope for this tutorial but it will be covered when we get to [functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions){:target="_blank"} and [objects](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Basics){:target="_blank"}.
</div>

## JavaScript file

We'll finish by writing our "Hello, world" program one more way using a JavaScript file. As a web developer, writing JavaScript in files such as these using a text editor is how most "real programming" is done.

Before we do this, you'll first need to install Node.js to run any JavaScript files. You can do this by visiting the [Node.js website](https://nodejs.org) and downloading the Node.js installer which will walk you through the process.

Once you've finished the installation process, you can double check that everything has been installed correctly by opening a terminal window and running the following command:

```console
node -v
```

Now that Node.js is installed, we can create the new JavaScript file for our code in our previous `webdev-intro` directory.

```console
touch main.js
```

Within this file we'll add some of the code we used earlier with the REPL. However, since this code isn't being run in a web browser the `alert` function, a function provide to us by web browsers, is no longer available to us. Instead we'll use `console.log`.

```javascript
console.log('Hello, world!');
```

Save this and within a terminal window navigate to the `webdev-intro` directory and run the following command to run the JavaScript file.

```console
node main.js
```

At this point you should now see the argument to `console.log()` printed to the terminal window.

![Node hello world](/images/hello-world/node-hello-world.png)

Feel free to update the file with a few more function calls to see what happens.

```javascript
console.log('Hello, world!');
console.log('Hello, world?');
```

![Node hello world 2](/images/hello-world/node-hello-world-2.png)

## Conclusion

Before we finish, head over to [GitHub](`https://github.com/`) and create an account. From there, click "Start a project" and provide a repository name. Your directory name (`webdev-intro`) and repository name don't have to be the same but it may help so that you can keep the two aligned. Then, click "Create repository".

![Creating a GitHub repository](/images/hello-world/github-create-repo.png)

Now that your repository has been created you need to add the repository url to your project directory. Within the `webdev-intro` directory run the following command replacing `your-username-here` with your own GitHub username.

```console
git remote set-url origin https://github.com/your-username-here/webdev-intro.git
```

<div class="box">
The exact command listed above should be provided to you by GitHub after you create your repository so feel free to just copy and paste that command instead.
</div>

At this point you should see your updated URL by running the following command:

```console
git remote -v
```

Now that the GitHub remote URL is set, add the latest updates along with a commit message.

```console
git add .
git commit -m "Add hello world"
```

Then push these changes up to GitHub.

```console
git push origin master
```

<div class="box" markdown="1">
Now that these changes have been pushed to GitHub your code is accessible to you at any time (thanks to the "cloud") even if you delete the files on your computer. Once again, Git and GitHub will be covered in more detail in a later tutorial.
</div>

## Exercises

At this point it would be worth spending some time exploring the HTML page, the REPL, and the JavaScript file you just created. Think of these as your personal sandbox. Just play around and see what happens.

Experiment with arguments other than `Hello, world!`. What happens if you pass two strings to `alert()`? What happens if you pass two strings to `console.log()`?

```javascript
alert('First', 'Second');
console.log('First', 'Second');
```

What happens if you pass a [number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number){:target="_blank"} to `alert()` or `console.log()`?

```javascript
alert(1);
console.log(1);
```

In the [next tutorial](http://atom-morgan.github.io/data-types-in-javascript/){:target="_blank"} we'll take a look at some of the data types (we've already used strings and numbers) available to us in JavaScript.

{% include book-plug.html %}
