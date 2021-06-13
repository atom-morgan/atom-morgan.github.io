---
title: 'Code Companion #4: Properties and Methods in JavaScript'
date: '2018-06-12'
layout: post
tag: [tutorial, webdev]
excerpt: >-
  In this tutorial we're going to introduce some of the properties and methods exposed to us by the data types in JavaScript. Finally, we'll wrap up by taking our first look at iteration.
image: '/images/properties-and-methods/tutorial-properties-and-methods.png'
thumb_image: '/images/properties-and-methods/tutorial-properties-and-methods.png'
style: code
---

## Video

<div class="videoWrapper">
  <iframe width="560" height="315" src="https://www.youtube.com/embed/HlGpWv1_lfs" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
</div>

## Introduction

In the [previous tutorial](http://atom-morgan.github.io/data-types-in-javascript/){:target="_blank"} we took a closer look at the data types available to us in JavaScript and the variables, or containers, we use to store those values. We also saw how we can use conditionals, using `if` and `else`, to control the flow of our programs.

In this tutorial we're going to introduce some of the properties and methods exposed to us by the data types in JavaScript. Finally, we'll wrap up by taking our first look at iteration.

{% include discord.html %}

## Setup

First, create a new directory for the code we'll write in this tutorial.

```console
mkdir webdev-properties-and-methods
```

Then use `cd` to move into your new directory.

```console
cd webdev-properties-and-methods
```

Within this directory, create the new file for your code.

```console
touch properties-and-methods.js
```

## Properties

At the end of the [previous tutorial](http://atom-morgan.github.io/data-types-in-javascript/){:target="_blank"} we created a few objects and accessed their properties using dot notation as shown below.

```javascript
let myUser = {
  name: 'Adam Morgan',
  age: 28
};

console.log(myUser.name); // Adam Morgan
```

Well in JavaScript nearly everything is an object, including strings. This means strings, including all the strings we've created up to this point, have information about them that we can access known as properties. 

We'll start by accessing a string's `length` property.

```javascript
let username = 'adam';
console.log(username.length); // 4
```

As you can see, we use dot notation just as we did with objects to print the length of the string `username` using `username.length`. 

Another data type we've used, arrays, also have a `length` property.

```javascript
let languages = ['javascript', 'ruby', 'python'];
console.log(languages.length); // 3
```

So what's a practical application of a property such as `length`? We can use a combination of conditionals, the `length` property, and a comparison operator to create logic you've probably encountered at some point using the Internet.

```javascript
let username = 'adam';

if (username.length < 5) {
  console.log('Your username is too short.');
} else {
  console.log('Your username meets the length requirements!');
}
```

Here we've used the `length` property in the expression of our `if` statement. If the length of `username` is less than (`<`) 5 characters, we print a message to the user notifying them of the length requirement. Otherwise, we let the user know their username is valid.

{% include email-list.html %}

### Properties and Logical Operators

In addition to the [comparison operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comparison_Operators){:target="_blank"} we used in the example above, we also have [logical operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_Operators){:target="_blank"} at our disposal which we can leverage at times like this.

The first operator we'll look at is the logical AND (`&&`) operator. When you're comparing two booleans with `&&`, both have to be `true` for the combination to be `true`.

```javascript
console.log(true && true); // true
```

In this example we've used the `&&` operator to compare two booleans: `true` and `true`. Since they're both `true`, the result of the comparison is also `true`. Let's see how the `&&` operator works with the remaining combinations.

```javascript
console.log(true && true); // true
console.log(true && false); // false
console.log(false && true); // false
console.log(false && false); // false
```

As you can see, unless *both* sides of the `&&` operator are `true`, the result is `false`. The combination of possibilities above is what's known as a ["truth table"](https://en.wikipedia.org/wiki/Truth_table){:target="_blank"}.

Now, we can combine the logical `&&` operator with the `length` property and comparison operator we used earlier for a more comprehensive example.

```javascript
let username = 'adammorgan';
let password = '123';

if (username.length > 5 && password.length > 5) {
  console.log('Your username and password meet the length requirements!');
} else {
  console.log('Your username or password is too short.');
}
```

Here we have both `username` and `password` variables declared and initialized with values. Then within our `if` statement we use the `&&` operator with two expressions.

![If statement with logical operator](/images/properties-and-methods/if-operator.png)

On the left side is `username.length > 5` and on the right side is `password.length > 5`. If *both* expressions evaluate to `true`, the code in our `if` statement is executed. Otherwise, the code in `else` is executed.

Run that code and you should see `Your username or password is too short.` printed to the screen because the `length` of password is too short. We can verify `&&` is working correctly by updating the value of `password` to be greater than 5 characters.

```javascript
let username = 'adammorgan';
let password = 's3cr3t';

if (username.length > 5 && password.length > 5) {
  console.log('Your username and password meet the length requirements!');
} else {
  console.log('Your username or password is too short.');
}
```

<div class="box" markdown="1">
#### The REPL is your friend!

If at anytime you want to verify the result of these expressions just open a REPL and enter the code you want to run.

```javascript
let username = 'adammorgan';
username.length > 5;  // true
```

You can also avoid variables altogether and just reference the `length` property on the string itself.

```javascript
'adammorgan'.length > 5; // true
's3cr3t'.length > 5; // true
```
</div>

The other logical operator available to us is the logical OR (`||`) operator. Unlike `&&`, `||` evaluates to true if *either* expression is `true`. We can see how this works by recreating the truth table from earlier using `||`.

```javascript
console.log(true || true); // true
console.log(true || false); // true
console.log(false || true); // true
console.log(false || false); // false
```

As you can see, only one expression needs to be `true` for the result to be `true`.

We can see how we might use this in a program with the following example where we want to restrict certain features in an application to a type of user such as an administrator or moderator.

```javascript
let user = {
  isAdmin: false,
  isModerator: true
};

if (user.isAdmin || user.isModerator) {
  console.log('I only care about one of these! They are authorized!');
} else {
  console.log('Don\'t let them in!');
}
```

Here we have an object with two properties, `isAdmin` and `isModerator`, set to booleans. Then within our `if` statement we use the `||` operator with two expressions on either side. On the left side is `user.isAdmin` and on the right side is `user.isModerator`.

Run that code and you should see `I only care about one of these! They are authorized!` printed to the screen because only *one* of the two expressions needs to be `true`. Once again we can verify everything is working correctly by updating both values to `false`.

```javascript
let user = {
  isAdmin: false,
  isModerator: false
};

if (user.isAdmin || user.isModerator) {
  console.log('I only care about one of these! They are authorized!');
} else {
  console.log('Don\'t let them in!');
}
```

## Methods

In addition to properties we also have methods available to us as well.

```javascript
let username = 'Adam';

console.log(username.toLowerCase()); // adam
console.log(username.toUpperCase()); // ADAM
```

Unlike properties which are values, a method is a function that we call with parentheses (like we do with `console.log()`) to get the value we want. These are just two of the string methods available to us and you can see a complete list of string methods [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String#Methods_2){:target="_blank"}.

<!-- [Upper case vs lower case](http://3.bp.blogspot.com/__uJC-guN1CM/TJdusqqrE1I/AAAAAAAAARk/qVSelaSBGFc/s1600/9_29+Upper+%26+Lower+Case.JPG). -->

As a programmer, learning how to read documentation is an extremely important skill to develop. Let's explore the documentation for [array methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#Methods_2){:target="_blank"}.

![Array methods](/images/properties-and-methods/array-methods.png)

In this image are just a few of the array methods available to us. Let's take a closer look at the `pop()` method. As the description says `pop()` "removes the last element from an array and returns that element." You can [click the method name](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/pop){:target="_blank"} in the list and see even more details along with a few examples demonstrating how `pop()` works.

We can create our own example to illustrate the effects of `pop()`.

```javascript
let favoriteFruits = ['mango', 'blueberry', 'raspberry'];
let returnedValue = favoriteFruits.pop();

console.log(favoriteFruits); // ['mango', 'blueberry']
console.log(returnedValue); // raspberry
```

Run this program and you'll see that after calling `pop()`, the last element in the `favoriteFruits` array has been removed. We can also see that calling `favoriteFruits.pop()` *returned a value* which we assigned to the variable `returnedValue`.

While we're at it, we can test out the `push()` method listed just below `pop()`. The description for `push()` says it "adds one or more elements to the end of an array and returns the new length of the array."

We can see how this works with the following example.

```javascript
let favoriteFruits = ['mango', 'blueberry', 'raspberry'];
let returnedValue = favoriteFruits.push('passionfruit');

console.log(favoriteFruits); // ['mango', 'blueberry', 'raspberry', 'passionfruit']
console.log(returnedValue); // 4
```

## Iteration

The last topic we're going to cover is known as iteration. Iteration in programming is the technique of repeating one or many statements for a defined number of repetitions with each repetition known as an iteration.

Let's build up to this idea by first taking a look at the `charAt()` [string method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/charAt){:target="_blank"}.

```javascript
let name = 'Adam';

console.log(name.charAt(0)); // A
```

As you can see the `charAt()` method takes a single parameter, an index, and returns the character in the string at that index. We could continue for the remaining characters by copying and pasting the `console.log()` statement incrementing the index each time.

```javascript
let name = 'Adam';

console.log(name.charAt(0)); // A
console.log(name.charAt(1)); // d
console.log(name.charAt(2)); // a
// ...and so on
```

However, that gets old really quick. It's a lot of copying and pasting which is repetitive and error prone.

When we covered data types, we learned about variables that could store values such as `Hello, world!` so that we wouldn't have to repeatedly copy that value over and over. In a similar way, we can avoid duplicating `console.log()` using what's known as a `for` loop. First, let's take a look at a generic `for` loop.

<!-- Well we can avoid duplicating `console.log()` for each character in the string `name` by iterating through the string using a `for` loop. -->

```javascript
for (let i = 0; i < 5; i++) {
  console.log(i);
}

console.log('Done! Outside the for loop!');
```

Similar to an `if` statement, we begin our loop with the keyword `for` followed by a pair of parentheses. Within those parentheses are three expressions seperated by semicolons. Let's break these down one by one.

1. **initialization** - This is where we initialize a counter variable: `let i = 0;`.
1. **condition** - This is an expression that's evaluated *before each iteration*. If this evaluates to `true`, the statement(s) within the curly braces `{}` is executed. If the expression evaluates to `false`, execution skips to the next statement following the `for` loop.
1. **final expression** - This is an expression that's evaluated *after each iteration* and is generally used to increment the counter variable initialized in the initiliazation step above.

So let's walk through the loop code step by step.

```javascript
for (let i = 0; i < 5; i++) {
  console.log(i);
}

console.log('Done! Outside the for loop!');
```

When this code is run, it begins with the initialization step by declaring `i` and setting it to `0`. Then the condition `i < 5` is evaluated. Since it evaluates to `true` the statement(s) within the curly braces `{}` (also referred to as a block statement) is run. At this point `i` is `0` so the number `0` is printed to the screen. Now that the single statement has been run, the third expression in the for statement is evaluated: `i++`. The `i++` syntax simply increments the value of `i` by `1`.

In the next iteration the condition `i < 5` is evaluated again using the new, incremented value for `i` which is `1`. Once again the condition evaluates to `true` so the block statement is run again.

This repeats until `i < 5` evaluates to `false`. Once it does, our code exits the `for` loop running the first statement following `for` which is `console.log('Done! Outside the for loop!');`. Run the code listed above and you should see the following printed to your screen.

```console
0
1
2
3
4
Done! Outside the for loop!
```

Now we can take our previous example and print every character in a string using a single `console.log()` within a `for` loop.

```javascript
let name = 'Adam';

for (let i = 0; i < name.length; i++) {
  console.log(name.charAt(i));
}
```

We can also use a `for` loop with an array to print out every element within the array.

```javascript
let sales = [5, 7, 3.5, 8];

for (let i = 0; i < sales.length; i++) {
  console.log(sales[i]);
}
```

Running that code should print the following.

```console
5
7
3.5
8
```

Rather than print every element within `sales`, why not use the `for` loop to keep a running total instead? We can do that with the following code.

```javascript
let sales = [5, 7, 3.5, 8];
let total = 0;

for (let i = 0; i < sales.length; i++) {
  total = total + sales[i];
}

console.log(total); // 23.5
```

Now rather than printing the value during each iteration we repeatedly increment the value of `total` with `total = total + sales[i]`.

Finally, we can use a `for` loop to do things like modify an array of strings.

```javascript
let exclamations = ['run', 'hide', 'get away'];

for (let i = 0; i < exclamations.length; i++) {
  exclamations[i] = exclamations[i].toUpperCase();
}

console.log(exclamations); // ['RUN', 'HIDE', 'GET AWAY']
```

## Conclusion

Before we finish, add your latest changes with Git.

```console
git add properties-and-methods.js
```

Then add a commit message.

```console
git commit -m "Add properties and methods"
```

Then push these changes up to GitHub.

```console
git push origin master
```

## Exercises

A few `for` loops are listed below that use different data types, methods, and operators we've seen so far. Try guessing the results of each loop before running them to see if you're right.

```javascript
// Exercise #1
let names = ['   john', 'jane   ', '  jeff  '];
names.pop();

for (let i = 0; i < names.length; i++) {
  names[i] = names[i].trim();
}
console.log(names);

// Exercise #2
let languages = ['java', 'javascript', 'html', 'css', 'ruby'];

for (let i = 0; i < languages.length; i++) {
  if (languages[i].includes('java')) {
    console.log(languages[i]);
  }
}

// Exercise #3
let language = 'javascript';

for (let i = 0; i < language.length; i++) {
  console.log(language.charAt(i));
}
console.log('!');

// Exercise #4
let title = 'ceo';

for (let i = 0; i < title.length; i++) {
  console.log(title.charAt(i).toUpperCase());
}
```

For additional practice, try writing a few `for` loops of your own. Pick an [array method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#Methods_2){:target="_blank"} to see how it works.

<!-- In the next tutorial we'll take a look at some of the properties provided to us by data types. We'll also take a closer look at functions by writing some of our own. -->

{% include book-plug.html %}
