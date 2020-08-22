---
title: 'Code Companion #6: Object Oriented JavaScript'
date: '2018-06-27'
layout: post
tag: [tutorial, cc]
excerpt: >-
  In this tutorial, we're going to look at JavaScript by creating objects that we can instantiate using the new keyword as we saw in the last tutorial when we were working with Date.
image: '/images/object-oriented-javascript/tutorial-cover.png'
thumb_image: '/images/object-oriented-javascript/tutorial-cover.png'
style: code
---

## Video

 <div class="videoWrapper">
  <iframe width="560" height="315" src="https://www.youtube.com/embed/zFvxKn3DXqY" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
</div>

## Introduction

We've covered a lot of the basics of JavaScript up to this point. These are the building blocks—the fundamentals needed for programming. But we're now at a point where we can start using all of this to write simplified versions of programs you and I might actually use.

In this tutorial, we're going to look at JavaScript by creating objects that we can instantiate using the `new` keyword as we saw in the [last tutorial](https://atom-morgan.github.io/functions/){:target="_blank"} when we were working with `Date`.

It won't be much longer before we're adding some of this functionality to an HTML page.

{% include email-list.html %}

## Setup

Before we get into the code, create a new directory for the code we'll write in this tutorial.

```console
mkdir oo-javascript
```

Then use `cd` to move into your new directory.

```console
cd oo-javascript
```

Within this directory, create the new file for your code.

```console
touch oo-javascript.js
```

{% include request-tutorial.html %}

## Function declarations and expressions

Before we get to our new type of objects we have to cover a slightly different way of defining functions. Up until this point, we've been creating function declarations as shown below:

```javascript
// Function declaration
function getArea(width, height) {
  return width * height;
}

console.log(getArea(20, 10)); // 200
```

However, you can also create what's known as a function expression by assigning a function to a variable.

```javascript
// Function expression
let getArea = function(width, height) {
  return width * height;
};

console.log(getArea(20, 10)); // 200
console.log(getArea(10, 5)); // 50
```

While there are a few more technical details between the two, the primary difference we're concerned with for now is that you can omit a function's *name* from a function expression as shown in the example above by assigning it to a variable instead.

As you progress as a programmer, you'll see both function declarations and function expressions. But for the remainder of this tutorial, we'll be leveraging function expressions.

## Creating object literals

Up until this point we've been creating what are known as object literals. An example of one is shown below:

```javascript
let user = {
  name: 'Adam',
  age: 28
};

console.log(user.name); // Adam
console.log(user.age); // 28
```

Then in the [previous tutorial](https://atom-morgan.github.io/functions/){:target="_blank"} we saw a new way of creating objects using the `new` keyword as we did with `Date`.

```javascript
let now = new Date();
console.log(now.getDay());
console.log(now.getMinutes());
```

So what's going on here? Why aren't we always using the `new` keyword?

## Constructor Functions

A function like `Date` is what's known as a *constructor function*. When we call a function like `Date` and prefix it with the `new` keyword we're telling JavaScript we would like to instantiate a new object using that constructor function. Let's see what this looks like in code.

```javascript
function Trainer(name) {
  this.name = name;
}

let trainer = new Trainer('Adam');
console.log(trainer.name); // Adam
```

Here we've defined a constructor function `Trainer` with a single parameter `name`. Inside the constructor (or constructor function) the keyword `this` references the new object that's being created. So in the example a new `name` property is set to the value of the `name` parameter.

Then we *instantiate the object* with the line: `let trainer = new Trainer('Adam');`. After that, `trainer` behaves just like any other object we've created so far.

<div class="box" markdown="1">
It's a popular convention in JavaScript to capitalize constructor functions. Any other function that isn't a constructor function is camel cased like `getArea`.
</div>

## Adding functionality to Constructor Functions

Now let's add some functionality to our constructor function by adding a function to print the name of the trainer.

```javascript
function Trainer(name) {
  this.name = name;
  this.identify = function() {
    console.log(`The trainer is ${this.name}`);
  };
}

let trainer = new Trainer('Adam');
trainer.identify(); // The trainer is Adam
```

Here we create a new `identify` property set to a function expression. Within that function, we print out the trainer's name by accessing the object's `name` property using `this.name`. Call `trainer.identify()` and you should see the message printed to your screen.

## Creating multiple instances

Now it's time to see what makes this constructor function so powerful. With a constructor function we can instantiate multiple objects using the same constructor function. Let's see how this works.

```javascript
function Trainer(name) {
  this.name = name;
  this.identify = function() {
    console.log(`The trainer is ${this.name}`);
  };
}

let ash = new Trainer('Ash');
let gary = new Trainer('Gary');
ash.identify(); // The trainer is Ash
gary.identify(); // The trainer is Gary
```

We've left our constructor `Trainer` as it was before. But now we create *two* instances of `Trainer`, `ash` and `gary`, both using the `new` keyword. Now when we call `.identify()` on our two objects, each object will print their own respective name.

The constructor function is essentially a blueprint and calling the function with `new` brings it to life as an object. 

## Default parameters

Similar to traditional functions we can create constructor functions with multiple parameters as well. Let's add a new parameter and property for an array of tasks that our trainers must complete.

```javascript
function Trainer(name, tasks = []) {
  this.name = name;
  this.tasks = tasks;
  this.identify = function() {
    console.log(`The trainer is ${this.name}`);
  };
}

let ash = new Trainer('Ash');
let gary = new Trainer('Gary', ['Visit Professor Oak']);
console.log(ash.tasks); // []
console.log(gary.tasks); // ['Visit Professor Oak']
```

Here we added a new `tasks` parameter specifying a default value of an empty array if no value is passed in.

Then we create two instances of `Trainer`, one without a second argument for `tasks` and another with it. Printing the contents of `tasks` for each trainer shows our default parameter in action.

## Add tasks

Let's continue by adding an `addTask` function to the constructor function `Trainer` to update the `tasks` property.

```javascript
function Trainer(name, tasks = []) {
  this.name = name;
  this.tasks = tasks;
  this.identify = function() {
    console.log(`The trainer is ${this.name}`);
  };
  this.addTask = function(task) {
    this.tasks.push(task);
  };
}

let ash = new Trainer('Ash');
ash.addTask('Get a Pokemon');
console.log(ash.tasks); // [ 'Get a Pokemon' ]
```

Here we've added a new `addTask` function with a single `task` parameter. Within the function, we call the `push()` method to add the new `task` to the existing array.

Then we create our new object, `ash`, and call `.addTask()` with the new task `Get a Pokemon`.

## List tasks

We'll finish with one last function to list the existing tasks.

```javascript
function Trainer(name, tasks = []) {
  this.name = name;
  this.tasks = tasks;
  this.identify = function() {
    console.log(`The trainer is ${this.name}`);
  };
  this.addTask = function(task) {
    this.tasks.push(task);
  };
  this.printTasks = function() {
    console.log(`Remaining tasks: ${this.tasks}`);
  };
}

let ash = new Trainer('Ash');
ash.addTask('Get a Pokemon');
ash.printTasks(); // Remaining tasks: Get a Pokemon
```

Just below `addTask` we added a new `printTasks` function that uses `console.log()` to print the `tasks` array.

However, we want to add a small update to this. We want a small motiviational message to be displayed if the length of `tasks` is `1` (the trainer is nearly finished with their tasks).

## Comparison Operators

To do this, we'll use what are known as [equality operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comparison_Operators#Equality_operators){:target="_blank"}.

What we're concerned with specifically are the equality (`==`) and strict equality (`===`) operators.

```javascript
// Equality
console.log(1 == 1);    // true
console.log("1" == 1);  // true

// Strict equality
console.log(1 === 1);   // true
console.log("1" === 1); // false
```

The equality operator (`==`) converts both sides of the operator *if they are not the same type* then compares the two values returning `true` if they're the same.

The strict equality operator (`===`) compares both sides of the operator with *no type conversion*.

In other words, equality is only concerned with values whereas strict equality compares values and data types.

We can combine these with the logical NOT (`!`) operator which converts our equality operators to inequality operators.

```javascript
// Inequality
console.log(1 != '1');  // false
console.log(1 != 2);    // true

// Strict inequality
console.log(1 !== '1'); // true
console.log(1 !== 1);   // false
```

Within the JavaScript community the strict equality (`===`) operator is the preferred equality operator. Many advise you shouldn't use equality (`==`) unless you have a good reason to do so since the equality operator (`==`) only compares values and not data types.

Let's now apply these operators to `printTasks` to print a message to the trainer if they're nearly finished with their tasks.

```javascript
function Trainer(name, tasks = []) {
  this.name = name;
  this.tasks = tasks;
  this.identify = function() {
    console.log(`The trainer is ${this.name}`);
  };
  this.addTask = function(task) {
    this.tasks.push(task);
  };
  this.printTasks = function() {
    if (this.tasks.length === 1) {
      console.log('Nearly finished!');
    }
    console.log(`Remaining tasks: ${this.tasks}`);
  };
}

let ash = new Trainer('Ash');
ash.addTask('Get a Pokemon');
ash.printTasks();
// Nearly finished!
// Remaining tasks: Get a Pokemon
```

The addition here now checks if `tasks.length` is equal to `1`. If it is, we print out the message `Nearly finished!` before printing the remaining tasks in `tasks`.

## Conclusion

Before we finish, add your latest changes to Git.

```console
git init
git add .
```

Then add a commit message.

```console
git commit -m "Add object oriented JavaScript"
```

Then push these changes up to GitHub.

```console
git push origin master
```

## Exercises

For this exercise, add an additional method to `Trainer` to remove a task from `tasks`. A `Trainer` object is created below which calls the `removeTasks()` function you should implement.

{% include discord.html %}

Two array methods worth looking into to accomplish this are [`indexOf()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf){:target="_blank"} and [`splice()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice){:target="_blank"}.

```javascript
function Trainer(name, tasks = []) {
  this.name = name;
  this.tasks = tasks;
  this.identify = function() {
    console.log(`The trainer is ${this.name}`);
  };
  this.addTask = function(task) {
    this.tasks.push(task);
  };
  this.printTasks = function() {
    if (this.tasks.length === 1) {
      console.log('Nearly finished!');
    }
    console.log(`Remaining tasks: ${this.tasks}`);
  };
}

let ash = new Trainer('Ash', ['Get a Pokemon', 'Leave Pallet Town']);
ash.removeTask('Get a Pokemon'); // Get this to work
ash.printTasks();
// Nearly finished!
// Remaining tasks: Leave Pallet Town
```

### Bonus

Without the proper validation, `removeTask()` could remove the last element from an array by mistake as illustrated below.

```javascript
let tasks = ['laundry', 'dishes', 'vacuum'];
tasks.splice(tasks.indexOf('oil change'), 1);
console.log(tasks); // [ 'laundry', 'dishes' ]
```

Since the task `oil change` doesn't exist in `tasks` the `splice()` method instead removes an element from the end of the array as described in the documentation for the `start` parameter of `splice()`.

> Index at which to start changing the array (with origin 0). If greater than the length of the array, actual starting index will be set to the length of the array. **If negative, will begin that many elements from the end of the array (with origin -1)** and will be set to 0 if absolute value is greater than the length of the array. [Source](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice#Parameters){:target="_blank"}

See if you can fix this bug. If the task doesn't exist in `tasks`, print a message notifying the user of the non-existent task instead.

{% include book-plug.html %}
