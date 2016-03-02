---
layout: post
title: "'Hello, Testing!' with Jasmine"
date: 2016-03-02
---

## Goal

In the world of programming a "Hello, world" program allows us to create a very small working program to verify things are working correctly. If you've ever written JavaScript and you found yourself wishing there was a "Hello, world" equivelant for testing, this is it!

## Assumptions

For this tutorial I'm going to assume you have a basic understanding of JavaScriptand will be able to download a few tools we'll need for this tutorial. You're not trying to write code. You're trying to test it! Let's get started.

## Setup

First, let's create a directory that we'll work in for this tutorial.

```
mkdir js-testing && cd js-testing
```

Next, we'll need to install [Node Version Manager](https://github.com/creationix/nvm) (NVM).

```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.0/install.sh | bash
```

Once that has finished, we'll also install [Node.js](https://nodejs.org/en/).

```
nvm install node
```

## Hello!

With that, we're ready to create our first JavaScript file which we will eventually test. Similar to a "Hello, world" program we're going to keep the functionality very simple here. Let's start by creating a file where we'll place our to-be-tested code.

```
touch hello-testing.js
```

Within this file, we're going to create a module with one method that returns a message when called.

```
var Hello = (function() {
  var testing = function() {
    return "Hello, testing!";
  };

  return {
    testing: testing
  };
})();

module.exports = Hello;
```

>If this pattern looks a little odd, I'd highly recommend reading Todd Motto's blog post [Mastering the Module Pattern](https://toddmotto.com/mastering-the-module-pattern/).

## Jasmine

Now that you've created the file we want to test let's install [Jasmine](http://jasmine.github.io/2.4/introduction.html), the testing framework we'll be using for our test.

```
npm install -g jasmine
```

After that's finished installing, you'll need to initialize the project for Jasmine.

```
jasmine init
```

After that, your file structure should look like this:

```
js-testing
+-- hello-testing.js
+-- node_modules
+-- spec
```

>You may notice a `support` directory within `spec` as well. This is for configuring Jasmine but for now we can leave it as is.

The command `jasmine init` created the `spec` directory for us. This is where you'll write your test file. So navigate to that directory and create your first test file.

```
cd spec && touch hello-testing.spec.js
```

The first thing you'll do in this test file is `require()` the code from earlier so that we can actually use and test it and then we'll set up the test case:

```
var Hello = require("../hello-testing");

describe("Hello module", function() {
  it("should return a message", function() {
    expect(Hello.testing()).toEqual('Hello, testing!');
  });
});
```

The `describe` block here describes our suite of tests. Since we want to test the `Hello` module we created, I simply put `Hello module`. After that is our `it` spec which describes what our code should do. You could replace `"Hello, module"` and `"should return a message"` with any string but since it serves as a description of what code is being tested and how we expect it to behave, it's worth the time to make it readable. You'll see shortly why this is important.

Finally, we hit our test's expectation in the form of:

```
expect(THIS).toEqual(THIS);
```

As you can see in the real test we expect the method call, `Hello.testing()`, to equal `"Hello, testing"`. In other words, we expect our function call to be equal to the string we hardcode since we already know the answer.

With that finished you can finally save this file, close it, and run your test!

In the root directory (`js-testing` if you used the same names as me) run:

```
jasmine spec/hello-testing.spec.js
```

If everything is setup correctly, you should see a passing test similar to this:

![Jasmine Passing Test](http://i.imgur.com/eZOOBAH.png)

But how do we know that's actually testing our code? One quick way to find out is to modify our test to *expect a failing case*. Let's do that real quick by changing this line:

```
expect(Hello.testing()).toEqual('Hello, testing!');
```

To this:

```
expect(Hello.testing()).not.toEqual('Hello, testing!');
```

It's like you're writing English, right?

With that change run `jasmine spec/hello-testing.spec.js` again and you should see an error similar to the one below:

![Jasmine Failing Test](http://i.imgur.com/L8HviRq.png)

With that we now know that our test is handling our code correctly. When we `expect` it to return the string we specified, we don't see an error. But with an intentionally failing case, Jasmine let's us know something is wrong so we can update our code accrodingly.

## Bonus

This is the "Hello, world" of testing but I figured it couldn't hurt to add another real world example to our code. We're going to handle the well known [Fizz Buzz Test](http://c2.com/cgi/wiki?FizzBuzzTest). However, we're going to do a slightly modified version.

First, we're goint to call this function with a specified start and end range rather than looping through an arbitrary range of numbers. Instead of logging the numbers we'll also concatenate our result as a string and return that.

So let's add an extra method to `hello-testing.js` which will solve the Fizz Buzz problem.

```
var Hello = (function() {
  var testing = function() {
    return "Hello, testing!";
  };

  var fizzBuzz = function(start, end) {
    var message = "";
    for (var i = start; i <= end; i++) {
      if ((i % 5 === 0) && (i % 3 === 0)) {
        message += "FizzBuzz";
      }
      else if (i % 3 === 0)  {
        message += "Fizz";
      }
      else if (i % 5 === 0) {
        message += "Buzz";
      }
      else {
        message += i;
      }
    }
    return message;
  };

  return {
    testing: testing,
    fizzBuzz: fizzBuzz
  };
})();

module.exports = Hello;
```

With that in place let's add a couple test cases to `hello-testing.spec.js` to test this new code.

```
var Hello = require("../hello-testing");

describe("Hello module", function() {
  it("should return a message", function() {
    expect(Hello.testing()).toEqual('Hello, testing!');
  });

  it("should fizzbuzz correctly", function() {
    expect(Hello.fizzBuzz(1, 10)).toEqual("12Fizz4BuzzFizz78FizzBuzz");
    expect(Hello.fizzBuzz(1, 15)).toEqual("12Fizz4BuzzFizz78FizzBuzz11Fizz1314FizzBuzz");
  });
});
```

Save that file, run `jasmine spec/hello-testing.spec.js`, and you should see two passing tests with zero failures. Now you can test a programming problem you're likely to see at some point in future interviews.

>If you're looking at my `fizzBuzz` code thinking you would have done it differently, that's okay. Our test is not concerned with *how* the code is solving the problem. It simply checks to see if a piece of code being tested returns the result we expect with a given set of inputs, if any are needed. If it looks worthy of a refactor to you, now is the best time. It's already tested so at this point you can make your changes, run the test, and if everything passes you know your code still behaves as expected even if the implementation has changed.

## Summary

Hopefully you now know how to install Jasmine, setup a test file, and test a piece of code. If you have any questions or feedback feel free to reach out to me on Twitter [@atommorgan](http://twitter.com/atommorgan).
