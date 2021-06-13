---
title: 'Code Companion #5: Functions'
date: '2018-06-20'
layout: post
tag: [tutorial, webdev]
excerpt: >-
  Up until this point we've been calling functions (also referred to as methods) like console.log() and .charAt(). In this tutorial we're going to switch from calling functions to writing them as well.
image: '/images/functions/tutorial-functions.png'
thumb_image: '/images/functions/tutorial-functions.png'
style: code
---

## Video

<div class="videoWrapper">
  <iframe width="560" height="315" src="https://www.youtube.com/embed/5xSLd57kWJs" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
</div>

## Introduction

In the [previous tutorial](https://atom-morgan.github.io/properties-and-methods/){:target="_blank"} we introduced properties and methods such as `.length` and `.charAt()` as well as the logical operators AND and OR. We finished by writing our first iterative code using the `for` loop.

Up until this point we've been calling functions (also referred to as methods) like `console.log()` and `.charAt()`. In this tutorial we're going to switch from *calling* functions to *writing* them as well.

{% include email-list.html %}

## Setup

First, create a new directory for the code we'll write in this tutorial.

```console
mkdir webdev-functions
```

Then use `cd` to move into your new directory.

```console
cd webdev-functions
```

Within this directory, create the new file for your code.

```console
touch functions.js
```

{% include request-tutorial.html %}

## Creating functions

In JavaScript, function calls consists of zero or more arguments enclosed in parentheses. Two examples illustrating this are listed below:

```javascript
// One argument
console.log('Hello, world!');

// Zero arguments
let sales = [5, 2.50, 3];
sales.pop();
```

Declaring or defining our own functions is one of the most important concepts in programming. It allows us to structure our programs and reduce repetition by isolating code into a single, reusable function.

So let's write our first function that determines whether or not a string was passed as an argument to the function call.

```javascript
function evaluateString(string) {
  if (string) {
    return 'The string has a value!';
  } else {
    return 'What string?';
  }
}
```

To declare a function we begin with the name of the function, `evaluateString`, followed by a comma separated list of parameters enclosed in parentheses. In this function declaration, we have a single parameter named `string`. Following the function name and parameters are the open and close braces (`{}`). Within this block is where we write the code we want to be executed when our function is called.

<!--![Function declaration](/images/functions/function-declaration.png)-->

In our example we first check if the parameter `string` is truthy (has a value) and use `return` to specify a *return value* of the function: `The string has a value!`. If it's falsy, the function's return value is `What string?`.

Now that we have our first function declaration, we need to call it.

```javascript
function evaluateString(string) {
  if (string) {
    return 'The string has a value!';
  } else {
    return 'What string?';
  }
}

let first = evaluateString('');
let second = evaluateString('This one!');
console.log(first); // What string?
console.log(second); // The string has a value!
```

Here we declare two new variables, `first` and `second`, and set them equal to the *return value* of the function call `evaluateString()`. In the first function call we call `evaluateString` and pass it an empty string and in the second call we provide it the string `This one!`. Run that code and you should see the two return values printed to your terminal window.

<div class="box" markdown="1">
If you don't want to set the return value to a variable and then print it using `console.log()` you can just pass in your *function call as the argument* to `console.log()` instead.

Feel free to use either one because the end result is the same. The takeaway here is that functions can *return values* and we can set those return values to variables if needed.

```javascript
// Option 1
console.log(evaluateString(''));
console.log(evaluateString('This one!'));

// Option 2
let first = evaluateString(''); 
let second = evaluateString('This one!'); 
console.log(first); // What string?
console.log(second); // The string has a value!
```
</div>

### Parameter vs. Argument

Up to this point we've used the terms parameter and argument without clarifying the distinction between the two. The difference is small and one that even seasoned programmers get wrong.

We can see a condensed version of the previous example below:

```javascript
function evaluateString(string) { // parameter
  ...
}

evaluateString('This one!'); // argument
```

An argument is the value(s) or variable(s) that's used when *calling a function*. When we call `evaluateString('This one!')` the argument is the string `This one!`.

A parameter is the name of the value that's *passed in to the function*. When we defined our function `evaluateString` we specified a single parameter `string`.

So what does this mean in practice? The name of our parameter is basically irrelevant from the *function caller's* point-of-view. That means we can update our parameter name to something silly like `lol` and everything still works just fine!

```javascript
function evaluateString(lol) { // Updated parameter name
  if (lol) {
    return 'The string has a value!';
  } else {
    return 'What string?';
  }
}

console.log(evaluateString('')); // What string?
console.log(evaluateString('This one!')); // The string has a value!
```

The parameter name is merely a placeholder for the value that's passed in to the function but it's still a best practice to make your parameter names descriptive.

Let's continue with functions by writing another one to square a number.

```javascript
function square(number) {
  return number * number;
}

console.log(square(2)); // 4
let result = square(3);
console.log(result); // 9
```

Here we defined a function `square` with a single parameter `number` that simply returns `number` multiplied by itself. We then call the function two times, printing the results to the screen.

Now let's move on to writing a function that gives us some additional...functionality...to JavaScript's `Date` object which represents a specific moment in time. For example, we can create a new instance of the `Date` object using the `new` operator and with this object we can then call the `.getDay()` method to get the current day of the week.

```javascript
let now = new Date();
console.log(now.getDay()); // A number between 0 and 6
```

Run that code and you should see a number between 0 and 6 printed to the screen where Sunday is 0 and Saturday is 6.

<div class="box" markdown="1">
We'll get to the specifics of objects like `Date` and terms like "instance" and `new` in a later tutorial. Before we get there, we need to solidify the concept of functions.
</div>

Unfortunately, a number isn't exactly how humans refer to the days of the week. So let's write a function that when given the numerical result of `.getDay()` will return us the day of the week as a string.

```javascript
function dayOfWeek(date) {
  const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
  return days[date.getDay()];
}
```

First we declare our function's name `dayOfWeek` and its parameter `date`. Then we declare a variable `days` using the `const` keyword (more on that in a minute) that's set to an array of strings containing the days of the week. We end our function by returning a single element within `days` using the index given to us by calling `date.getDay()`.

We can see this in action by calling the `dayOfWeek` function using the `Date` object.

```javascript
function dayOfWeek(date) {
  const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
  return days[date.getDay()];
}

let now = new Date();
console.log('Happy ' + dayOfWeek(now) + '!');
```

Run that and you should see something like `Happy Wednesday!` printed to the screen depending on what day of the week it is when you're working through this tutorial.

### Const vs. let

In our previous example we saw a new keyword, `const`, that was used when declaring the variable `days`. The keyword `const`, like `let`, is another keyword available to us when declaring variables.

The keyword `const` stands for constant and it's a variable whose value doesn't change. We can illustrate this with the following code:

```javascript
let name = 'Adam';
name = 'Morgan';
console.log(name);

const pi = 3.14;
pi = 4.14;
console.log(pi);
```

Run that code and you should see an error like this:

```console
/Users/adammorgan/Desktop/webdev-functions/functions.js:6
pi = 4.14;
   ^

TypeError: Assignment to constant variable.
```

As you can see since we already assigned a value to the constant `pi` when it was declared, our code crashes once it hits that line because we're attempting to update the value of a constant which isn't allowed.

So constants are great for values like `days` in our previous example where `days` was set to the days of the week—a value we don't want to change.

### Template literals

Before moving on to our last example I want to briefly address a slightly cleaner way of combining variables and strings. Using concatentation, things can start to get messy quick.

```javascript
let name = 'Adam';
console.log('My name is ' + name + '.');
```

With multiple values and a longer string, the string can be a bit difficult to read for a developer who may be making updates. Instead, we can use a template literal with placeholders.

```javascript
let name = 'Adam';
console.log(`My name is ${name}.`);
```

Now instead of concatenating strings together with plus signs (`+`) we can use the dollar sign and curly brace syntax (`${expression}`) instead.

### Multiple parameters

We'll wrap up with one more example of a function that has two parameters instead of one.

```javascript
function calculateSalesTax(price, tax) {
  console.log(`price is ${price}`);
  console.log(`tax is ${tax}`);
  return price * tax;
}

let result = calculateSalesTax(10, 0.15);
console.log(`Sales tax is $${result}`);
```

Here we've declared a function `calculateSalesTax` with two parameters: `price` and `tax`. Within the function we simply print both parameters using `console.log()` and return the value of the two multiplied together. Run that code and you should see the following printed to your screen:

```console
price is 10
tax is 0.15
Sales tax is $1.5
```

Earlier we mentioned that the names of the parameters doesn't matter. For example, we can update the names for `price` and `tax` to far less descriptive names and see our code still works. 

```javascript
function calculateSalesTax(a, b) {
  console.log(`a is ${a}`);
  console.log(`b is ${b}`);
  return a * b;
}

let result = calculateSalesTax(10, 0.15);
console.log(`Sales tax is $${result}`);
```

Run that and you should still see the same output as before with the updated variable names.

```console
price is 10
tax is 0.15
Sales tax is $1.5
```

Now what happens if we switch the ordering of our *arguments* when we *call* our function?

```javascript
function calculateSalesTax(price, tax) {
  console.log(`price is ${price}`);
  console.log(`tax is ${tax}`);
  return price * tax;
}

let result = calculateSalesTax(0.15, 10); // Swapped these values
console.log(`Sales tax is $${result}`);
```

Run that and you should see a slightly different output.

```console
price is 0.15
tax is 10
Sales tax is $1.5
```

So while the *naming* of our parameters doesn't matter the ordering of arguments *when calling functions* does matter. Take a look at the more robust version of `calculateSalesTax` below:

```javascript
function calculateSalesTax(prices, tax) {
  let result = [];
  for (let i = 0; i < prices.length; i++) {
    result.push(prices[i] * tax);
  }
  return result;
}

console.log(calculateSalesTax([1, 5, 10], 0.15));
console.log(calculateSalesTax(0.15, [1, 5, 10]));
```

Here we iterate through `prices` using a `for` loop multipling each element by `tax`. We then add each result to the `result` array using the `.push()` method. Once the loop has finished we then return the value of `result`.

Below our function are two function calls with their arguments swapped. In the first function call an array of prices is followed by the tax rate whereas the second function call has the tax rate first and the array or prices second.

Run that code and the results should illustrate why the order of arguments is important when calling a function.

```javascript
[ 0.15, 0.75, 1.5 ] // The result we expect
[]                  // ...not so much
```

## Conclusion

Before we finish, add your latest changes to Git.

```console
git init
git add .
```

Then add a commit message.

```console
git commit -m "Add functions"
```

Then push these changes up to GitHub.

```console
git push origin master
```

## Exercises

To practice finish implementing the `greetByDay` function so that when it's called it prints the greeting message we created earlier, `Happy Wednesday!`, where `Wednesday` is whatever day of the week it is.

```javascript
function dayOfWeek(date) {
  const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
  return days[date.getDay()];
}

function greetByDay() {
  // Add your code here
}

let now = new Date();
greetByDay(now); // Happy Wednesday!
```

{% include discord.html %}

For this exercise update the functionality of `calculateSalesTax` to return an array containing the price with the *sales tax added to the original price*.

```javascript
function calculateSalesTax(prices, tax) {
  // Add your code here
}

console.log(calculateSalesTax([1, 5, 10], 0.15)); // [ 1.15, 5.75, 11.5 ]
```

{% include book-plug.html %}
