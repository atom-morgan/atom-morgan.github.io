---
title: 'Code Companion #3: Data Types in JavaScript'
date: '2018-06-05'
layout: post
tag: [tutorial, webdev]
excerpt: >-
  In this tutorial we're going to take a closer look at the JavaScript and introduce some of the data types that are available to us.
image: '/images/data-types/tutorial-data-types.png'
thumb_image: '/images/data-types/tutorial-data-types.png'
style: code
---

## Video

<div class="videoWrapper">
<iframe width="560" height="315" src="https://www.youtube.com/embed/jaoqJG1xEOI" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
</div>

## Introduction

In the [previous tutorial](http://atom-morgan.github.io/hello-world-in-javascript/){:target="_blank"} we took our first look at JavaScript by writing three variations of a "Hello, world" program using JavaScript in a `.html` file, JavaScript in a REPL, and finally JavaScript in a `.js` file. In those programs we briefly introduced the concept of a string by passing the string `Hello, world!` to `console.log()`.

In this tutorial we're going to take another look at the JavaScript we wrote and introduce some of the other data types that are available to us.

{% include discord.html %}

## Comments

It's worth mentioning that similar to the comments we used in our `.html` file, we can also add comments in JavaScript as well.

You can add comments in JavaScript using one of two ways.

```javascript
// Log a name
console.log('Adam');
```

This is what's known as a single line comment. Just like our HTML comments this text is ignored when we run our code and it's invisible to the user. It's only visible to us and any other developers who look at this code.

```javascript
/* 
Sometimes you just
need a little extra
room :)
*/
console.log('Adam');
```

This is a multi-line comment. If a single line comment is getting a bit too long, use this syntax.

<div class="box">
When you're just starting out, feel free to make liberal use of comments. Don't hesitate to add a plain English explanation of any code that's still a bit confusing to you.
</div>

## Variables

Before we get into variables and data types in JavaScript, create a new file for the code we're going to write in this tutorial. You can either create a new directory or add this new file to the same directory as last time.

```console
touch data-types.js
```

Then add some code that's similar to where we left off last time.

```javascript
console.log('Hello, world!');
```

In this program, we're passing a string as an argument to `console.log()`. However there's a part of JavaScript (and programming languages in general) that allows us to essentially store this string in a container. These containers are known as variables. Let's update the program above to use a variable for the greeting message `Hello, world!`.

```javascript
// Declare a variable and assign it a value
let greeting = 'Hello, world!';
console.log(greeting);
```

Now our program is utilizing a variable named `greeting` which has been assigned the value `Hello, world!`. Similar to our function call, we can break down this variable declaration into a few parts.

{% include request-tutorial.html %}

![Variable declaration](/images/data-types/variable-declaration.png)

When we create variables we begin with what's known as a "declaration". In this case we've used the keyword `let` for our variable declaration. Following that is the "identifier" or what's more commonly referred to as the name for the variable: `greeting`. After the variable name we add the assignment operator, `=`, and set it to its value. In this example the value that is assigned to `greeting` is the string `Hello, world!`. Finally, we end our statement with a semicolon.

It's also possible to simply declare a variable without assigning a value to it. We can see this in the following example.

```javascript
let greeting;
console.log(greeting);
greeting = 'Hello, world!';
console.log(greeting);
```

First we declare a variable `greeting` without assigning it a value. Then we attempt to `console.log()` the value of `greeting`, assign a string to `greeting`, and finally we attempt to `console.log()` the value of `greeting` again.

Run this and you'll notice the code runs without any errors. However the first `console.log()` call prints out the value `undefined`. This is a value in JavaScript that's automatically assigned to variables that haven't been assigned a value. We'll see how this `undefined` value can be used to our advantage shortly when we get to control flow.

![Logging a variable with no assigned value](/images/data-types/logging-undefined.png)

So why do we need variables in the first place?

Working with variables in programming allows us to work with symbolic identifiers rather than actual values. Imagine the greeting `Hello, world!` was used multiple times throughout a program. Rather than repeatedly typing `Hello, world!` every time we need that value we can instead assign it to a variable such as `greeting` and then reference `greeting` instead.

Our new variable would also give us some flexibility in the future in the event that we decided to change the content of our greeting. Without variables, we'd have to find and replace every instance of our old greeting. With a variable, we make the update once and any references to the variable `greeting` will now use the updated greeting message.

### Strings

We've already been working with strings up to this point but it is worth mentioning a few more details.

First, you can create strings in JavaScript using either single or double quotes.

```javascript
let firstName = 'Adam'; // single
let lastName = "Morgan"; // double
console.log(firstName);
console.log(lastName);
```

Enter `node data-types.js` to run this program and you'll see both `firstName` and `lastName` are printed to the screen even though one string was enclosed in single quotes and the other in double quotes.

<div class="box" markdown="1">
The identifiers here are written using what's known as [Camel case](https://en.wikipedia.org/wiki/Camel_case){:target="_blank"}. This helps make identifiers a bit more legible at first glance. For example, compare the identifier `firstname` with `firstName`. Both identifiers are valid identifiers but it's common within the JavaScript world to use camel case.
</div>

The one scenario where strings can get a little weird is when you have single or double quotes inside of a string.

```javascript
// This doesn't work
let message = 'You can't be serious!';
console.log(message);
```

Run that code and you should see an error along the lines of `SyntaxError: Unexpected identifier` that causes the program to crash. In a text editor such as Visual Studio Code you may have even noticed something was wrong due to the syntax highlighting.

![Syntax error](/images/data-types/syntax-error.png)

So how do we fix this? When a string enclosed in single quotes *contains a single quote*, you can tell JavaScript to treat this as a part of the string by preceding the single quote with a backslash: `\`. This is what's known as an "escape character".

```javascript
// This does work
let message = 'You can\'t be serious!';
console.log(message);
```

When a string enclosed in double quotes *contains double quotes*, such as dialogue, you can escape the double quote with a backslash as well.

```javascript
let message = "He asked, \"Where are we?\"";
console.log(message);
```

Another alternative is to update the enclosing quotes to single quotes.

```javascript
let message = 'He asked, "Where are we?"';
console.log(message);
```

Neither option is right or wrong, it's purely a matter of personal preference. You're bound to see both styles used as you continue programming depending on who you work with.

### Number

The next data type available to us is the number type. Unlike strings which have no real parallel in the real world (we don't refer to characters, words, or sentences as strings) numbers are a bit more straightforward.

```javascript
let pi = 3.14;
let age = 28;

console.log(pi);
console.log(age);
```

Here we've created two variables set to numbers. The first variable, `pi`, is set to what's known in programming as a "floating point" number but both are considered to be a number in JavaScript.

Along with numbers are some of the mathematical operators you'd expect to be able to use with numbers.

```javascript
console.log(1 + 1); // 2
console.log(2 - 1); // 1
console.log(2 * 2); // 4
console.log(3 / 4); // 0.75
```

<div class="box" markdown="1">
These are just some of the mathematical operators available to us in JavaScript and you can see a more complete list [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators){:target="_blank"}. We'll start to introduce more of these as we progress.
</div>

### Boolean

Next on this list is the data type known as boolean. A boolean is a data type that has one of two values: `true` or `false`. When assigning a boolean to a variable, **do not** use quotes. Otherwise you're assigning strings to the variable instead of the intended boolean.

```javascript
let isDrinkingAge = true;
let isSeniorCitizen = false;

console.log(isDrinkingAge);
console.log(isSeniorCitizen);
```

As you may be able to infer from the example above, booleans are useful in programming when a variable can have one of two values. In the case of `isDrinkingAge` a person is either 21 or older or they're younger than 21.

A boolean is useful in this case because it controls the flow of logic. If you're a bouncer at a bar and someone is under 21, you turn them away. If they're 21 or older, you let them in.

You've likely encountered this online as well. If you view the website of a brewery, there's usually a popup asking you for your age. If you're of legal drinking age you get to see the website. Otherwise you're turned away. Booleans in action!

### Arrays

Up until now every variable we've created has had a single value such as a string, number, or boolean. What if we need to store multiple values in a single variable? That's where arrays come in.

{% include email-list.html %}

Arrays are an ordered collection of data that is used to store multiple values.

```javascript
let favoriteFruits = ['mango', 'raspberry', 'blueberry'];
console.log(favoriteFruits);
```

Run this and you should see the array of strings printed to the terminal. If we need to get a single value from this array we can do this using what's known as an index.

An index is what's used to reference a specific element, or value, within the array. Arrays in JavaScript are zero-indexed meaning the first element in the array begins at index 0 with each subsequent element in the array incrementing the index by 1.

So if we wanted to print the first element in the array we would do it like this:

```javascript
let favoriteFruits = ['mango', 'raspberry', 'blueberry'];
console.log(favoriteFruits);

// Print the first element
console.log(favoriteFruits[0]);
```

As you can see the first element in the array was accessed by adding an open and close bracket (`[]`) to the end of the variable identifier (also known as bracket notation). Within the open and close bracket is where we add the index. Let's update it by adding the remaining elements in the array.

```javascript
let favoriteFruits = ['mango', 'raspberry', 'blueberry'];
console.log(favoriteFruits);

// Print each element
console.log(favoriteFruits[0]);
console.log(favoriteFruits[1]);
console.log(favoriteFruits[2]);
```

<!-- Note about data typically being related? -->

### Object

Finally we have the powerhouse of JavaScript data types: the object. Objects are a list of property names and values enclosed in curly braces (`{}`). They're somewhat similar to arrays but rather than using integers as an index, objects use strings.

```javascript
let userObject = { name: 'Adam', age: 28, favoriteFruits: ['mango', 'raspberry', 'blueberry'] };
```

We can format this object into a more readable form with each property on a new line.

```javascript
let userObject = {
  name: 'Adam',
  age: 28,
  favoriteFruits: ['mango', 'raspberry', 'blueberry']
};
```

Unlike arrays which typically contain values of the same data type, objects often contain a mix of many or all data types. In the object above we've used a number for my age, a string for my name, and an array of strings for my favorite fruits.

We can access these values using what's known as dot notation.

```javascript
let userObject = {
  name: 'Adam',
  age: 28,
  favoriteFruits: ['mango', 'raspberry', 'blueberry']
};

console.log(userObject.name); // Adam
console.log(userObject.age); // 28
console.log(userObject.favoriteFruits); // [ 'mango', 'raspberry', 'blueberry' ]
console.log(userObject.favoriteFruits[0]); // mango
```

Similar to arrays you can also use bracket notation to access the values but dot notation is by far more common.

```javascript
let userObject = {
  name: 'Adam',
  age: 28,
  favoriteFruits: ['mango', 'raspberry', 'blueberry']
};

console.log(userObject['name']); // Adam
console.log(userObject['age']); // 28
console.log(userObject['favoriteFruits']); // [ 'mango', 'raspberry', 'blueberry' ]
console.log(userObject['favoriteFruits'][0]); // mango
```

You can even nest objects *within* other objects.

```javascript
let userObject = {
  name: 'Adam',
  age: 28,
  favoriteFruits: ['mango', 'raspberry', 'blueberry'],
  car: {
    make: 'Honda',
    model: 'Civic',
    isFunctional: true
  }
};

console.log(userObject.car.make); // Honda
console.log(userObject.car.model); // Civic
console.log(userObject.car.isFunctional); // true
```

Another key difference is that objects aren't ordered like arrays. Since our values here are referenced through a property name rather than an index the order of these properties doesn't matter.

### Conditionals

The last thing we'll cover isn't a data type but it does help us showcase some of the new things we can do with variables and data types. Here we'll take a look at the `if` and `else` statement available to us in JavaScript that allows us to control the flow of our program. Even if you haven't done much programming you may be familiar with the keywords `if` and `else`.

We'll start with an example using our previous `userObject` and the `age` property within it.

```javascript
let userObject = {
  name: 'Adam',
  age: 28,
  favoriteFruits: ['mango', 'raspberry', 'blueberry'],
  car: {
    make: 'Honda',
    model: 'Civic',
    isFunctional: true
  }
};

if (userObject.age >= 21) {
  console.log('This user can drink!');
} else {
  console.log('This user cannot drink!');
}
```

Run this program and you should see `This user can drink!` logged to the screen. Update the value of age to `18`, run it again, and you'll see `This user cannot drink!` logged to the screen. So how does this work?

<!-- *figure for if...else statement* -->

The `if` statement begins with the keyword `if` followed by an open and close parenthesis. Within these parenthesis is where we provide an expression that evaluates to either a "truthy" or "falsy" value (more on that in a bit).

In our example, we provided the expression `userObject.age >= 21` using the new "greater than or equal to" operator. Assuming `userObject.age` is the original value of `28` our condition evaluates to `28 >= 21` which is "truthy". As a result, the code within the `if` statement is executed which logs `This user can drink!`. If the condition is "falsy" the code within the `if` statement is ignored and the code within `else` is executed logging `This user cannot drink!` instead.

<div class="box" markdown="1">
The operators we saw earlier (`+`, `-`, `*`, `/`) are known as [arithmetic operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators). Those are operators we use to perform math within JavaScript. Operators such as `>=` are known as comparison operators which compares two values. You can see a complete list [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comparison_Operators).
</div>

So what is meant by a condition evaluating to a "truthy" or "falsy" value? Let's take a look at this example.

```javascript
let user = 'Adam';

if (user) {
  console.log('Goodbye, ' + user + '!');
} else {
  console.log('Goodbye!');
}
```

Run this program and you should see `Goodbye, Adam!` printed to the screen.

<div class="box" markdown="1">
Just a minute ago a link was provided containing a complete list of [arithmetic operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators) including `+`. However, we can see in the example above that the `+` operator is being used with strings.

That's because the `+` operator in JavaScript can be used to do math and to manipulate strings. In this case, since the `+` operator is being used with strings, the operator performs *concatenation* rather than *addition* which joins the strings together as one.
</div>

The reason for this is that the condition in our `if` statement only needs to be "truthy", not necessarily `true`. In our case, we have a variable that's been assigned a value which classifies as "truthy" in JavaScript. Had we left the variable declaration *without assigning it a value* we'd see a change in our program's behavior.

```javascript
let user;

if (user) {
  console.log('Goodbye, ' + user + '!');
} else {
  console.log('Goodbye!')
}
```

Run this program and you should see `Goodbye!` printed to the screen.

As you can now see, removing the value assignment from our variable declaration has resulted in `user` now being evaluted as "falsy" rather than "truthy". That's a result of `user` now being `undefined` which is a "falsy" value in JavaScript. The list below is what's ["falsy" in JavaScript](https://developer.mozilla.org/en-US/docs/Glossary/Falsy){:target="_blank"}.

```javascript
// Falsy values in JavaScript
if (false)
if (null)
if (undefined)
if (0)
if (NaN)
if ('')
if ("")
```

## Conclusion

Before we finish, add your latest changes with Git.

```console
git add data-types.js
```

Then add a commit message.

```console
git commit -m "Add data types"
```

Then push these changes up to GitHub.

```console
git push origin master
```

## Exercises

To practice, create an object for yourself or a historical figure. Update existing properties or add new properties and experiment with the different data types available to you. Then verify that you can access these values by printing them to the screen with `console.log()`.

A slightly modified `userObject` is below with a few `console.log()` statements at the end. Try guessing the results of each statement before running the code to see if you're right.

```javascript
let userObject = {
  name: 'Adam',
  age: 28,
  favoriteFruits: ['mango', 'raspberry', 'blueberry'],
  car: {
    make: 'Honda',
    model: 'Civic',
    isFunctional: true
  },
  computer: [
    {
      manufacturer: 'Apple',
      type: 'laptop'
    },
    {
      manufacturer: 'Dell',
      type: 'laptop'
    }
  ]
};

console.log(userObject.favoriteFruits[2]); // ?
console.log(userObject.car.isFunctional); // ?
console.log(userObject.computer[0].manufacturer); // ?
console.log(userObject.computer[1].manufacturer); // ?
```

In the [next tutorial](http://atom-morgan.github.io/properties-and-methods/){:target="_blank"} we'll take a look at some of the properties provided to us by data types. We'll also take a closer look at functions by writing some of our own.

{% include book-plug.html %}
