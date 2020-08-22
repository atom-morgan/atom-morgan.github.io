---
title: 'Code Companion #7: Automated Testing'
date: '2018-07-04'
layout: post
tag: [tutorial, cc]
excerpt: >-
  In this tutorial we're going to take a look at automated testing. Automated testing is a way for us to automate the testing of our code—something we've been doing manually up until this point.
image: '/images/automated-testing/tutorial-cover.png'
thumb_image: '/images/automated-testing/tutorial-cover.png'
style: code
---

## Video

<div class="videoWrapper">
  <iframe width="560" height="315" src="https://www.youtube.com/embed/M02PkvLeCmY" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
</div>

## Introduction

In this tutorial we're going to take a look at automated testing. Automated testing is a way for us to automate the testing of our code—something we've been doing manually up until this point.

Think about the functions we've written so far. We write a bit of code, we run it, and then we manually verify the code is giving us the result we expect. With automated testing, we can automate *ourselves* out of this process.

We'll be taking a [test-driven development](https://en.wikipedia.org/wiki/Test-driven_development){:target="_blank"} (TDD) approach meaning we'll write our tests first. These tests will initially be failing (referred to as red in TDD) because the actual code hasn't been written yet. Then we'll write the code, run the test again, and (hopefully) verify that our tests are passing (referred to as green in TDD).

{% include email-list.html %}

## Setup

Before we get into the code, create a new directory for the code we'll write in this tutorial.

```console
mkdir webdev-testing
```

Then use `cd` to move into your new directory.

```console
cd webdev-testing
```

Within this directory, create the new file for your code.

```console
touch webdev-testing.js
```

{% include request-tutorial.html %}

## Jasmine

To test our code we'll need to leverage a testing framework. A testing framework provides us the tools we need to test our code without having to write all of it ourselves. The one we'll be using for this tutorial is [Jasmine](https://jasmine.github.io/){:target="_blank"}.

Since our code has a dependency, Jasmine, we'll start by creating a `package.json` file using the `npm init` command (npm was installed way back when we installed Node.js in [Code Companion #2](https://atom-morgan.github.io/hello-world-in-javascript/){:target="_blank"}). Run the following command in the `webdev-testing` directory.

```console
npm init
```

Run that and you should see a prompt asking you for input such as the project's name, version, description, git repository, etc. Feel free to simply hit `Enter` to these to leave them with their default values.

![npm init prompt](/images/automated-testing/npm-init.png)

After that is finished you should now see a `package.json` file in your directory. This file contains details about our code such as its dependencies (like Jasmine) and metadata such as the project's description and author. Assuming another developer looked at this project they wouldn't need to ask which testing framwork was used. They could simply look at `package.json` and see for themselves.

So now let's install Jasmine and add it to `package.json`. First we'll install Jasmine globally to give us access to the Jasmine CLI.

```console
npm install jasmine -g
```

Then we'll add Jasmine to our project's development dependencies using the `-D` flag.

```console
npm install jasmine -D
```

A development dependency simply means that the dependency is necessary for development purposes. Testing code is something that's done in *development* before code is released to the world (production). So the `-D` flag is a way to signify that this dependency isn't needed for our code to actually run, only to aid us as we're writing the code.

From here we can initialize the project for Jasmine by running `jasmine init` to create our testing directory.

```console
jasmine init
```

After that has finished your directory should now look like this:

```console
webdev-testing
+-- node_modules
+-- package.json
+-- spec
+-- webdev-testing.js
```

The `package.json` file was created earlier when we ran `npm init`. Then when we installed Jasmine as a project dependency, it created the `node_modules` folder which contains Jasmine and all of *its* dependencies. Finally, we have the new `spec` directory that was created with `jasmine init`. This is where we will write our tests.

Move into this directory and create a new file. Unlike the file for our code, `webdev-testing.js`, this file should have the extension `.spec.js`. This is the file extension Jasmine looks for when it runs tests.

```console
cd spec
touch webdev-testing.spec.js
```

With that file in place you can now run `jasmine` in your terminal to run the tests.

```console
jasmine
```

![Jasmine no tests](/images/automated-testing/no-specs.png)

The output from that command should say something like "No specs found". That's because we haven't written any tests. So let's start with our first automated test.

## Our first test

We begin our automated tests by calling the `describe` function provided to us by Jasmine. This function is for naming a collection of tests and we provide it two arguments: the name of our collection of tests and a function.

Within the function we call the `it` function. This is what's known in Jasmine as a "spec". Similar to `describe` it takes two arguments: the name or title of the spec and a function.

Within the function passed into `it` is where we write our tests as shown below:

```javascript
describe('A suite', function() {
  it('contains a spec with an expectation', function() {
    // Test code here
  });
});
```

At the moment there isn't any code. Let's add something simple to show Jasmine in action.

```javascript
describe('A suite', function() {
  it('contains a spec with an expectation', function() {
    let myValue = true;
    expect(myValue).toBe(false);
  });
});
```

Here we create a variable `myValue` and assign it the value `true`. Then we call the `expect` function passing it what's known as an "actual". This is the bit of code we want to test.

Following that is what's known as a "matcher", `toBe()`. The value provided here is the "expected value". So our test in plain English is, "Expect `myValue` to be `false`".

<div class="box" markdown="1">
The matcher `toBe()` is just one of many matchers provided to us by Jasmine which you can see [here](https://jasmine.github.io/api/edge/matchers.html){:target="_blank"}.
</div>

To run this test open a terminal and navigate to the `webdev-testing` directory. Then run `jasmine`.

![First test, failing state](/images/automated-testing/first-test-failing.png)

You should now see Jasmine highlighting errors in the test. On line 4 it expects `true` to be `false`. Our test is currently in a failing (red) state. So let's get it to pass.

```javascript
describe('A suite', function() {
  it('contains a spec with an expectation', function() {
    let myValue = false;
    expect(myValue).toBe(false);
  });
});
```

With the value of `myValue` updated to `false` we can now run Jasmine again with the `jasmine` command.

![First test, passing state](/images/automated-testing/first-test-passing.png)

## Updating Date

Let's continue by writing a test for a new and better `Date`, specifically a function to provide us with the current day of the week in English as we did in [Code Companion #5](https://atom-morgan.github.io/functions/){:target="_blank"}.

First, we'll start by writing a failing (red) test for our code.

```javascript
describe('BetterDate', function() {
  it('should return the day of the week', function() {
    let betterDate = new BetterDate('02 Jul 2018');
    expect(betterDate.getDay()).toBe('Monday');
  });
});
```

As you can see, we begin by creating a new instance of `BetterDate` passing it a date in string form. Then we call the `.getDay()` function within `expect()` and chain it with the `.toBe()` matcher providing it the expected value `Monday`.

Run the `jasmine` command and you should see an error for our failing test because `BetterDate` is not defined.

![First test, failing state](/images/automated-testing/date-failing.png)

It's worth noting *how* Jasmine prints errors as well. The first item under "Failures" reads "BetterDate should return the day of the week". The strings we passed to `describe` and `it` combine to read like an English sentence. As these files grow in size, it's helpful for identifying where the error occurs and giving us an easily digestible way of understanding *why* our code is failing.

Now we can implement the constructor function `BetterDate` to get this test to pass. Just above `describe` add the following code.

```javascript
function BetterDate(date) {
  this.now = new Date(date);
  this.getDay = function() {
    const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
    let day = this.now.getDay();
    return days[day];
  };
}

describe('BetterDate', function() {
  // Code below omitted for brevity
  ...
});
```

In this constructor function is a `now` property that's set to an instance of `Date` using the `date` parameter. Then we define the `getDay` function with an implementation that should look similar to one we wrote in [Code Companion #5](https://atom-morgan.github.io/functions/){:target="_blank"}.

Run `jasmine` and you should now see this test in a passing (green) state.

![First test, passing state](/images/automated-testing/date-passing.png)

However, when we write code we typically don't add our code to test files. We have test files to *test* code but it doesn't need to be in the exact same file. Instead, we'll put the code for `BetterDate` into its own file and import it within our test file `webdev-testing.spec.js`.

To do this, first move the `BetterDate` function into `webdev-testing.js` and add another line at the bottom to export it.

```javascript
function BetterDate(date) {
  this.now = new Date(date);
  this.getDay = function() {
    const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
    let day = this.now.getDay();
    return days[day];
  };
}

module.exports = BetterDate;
```

The line `module.exports = BetterDate;` is the line that exports, or exposes, our code. It's a special object provided to us by Node.js specifically for this purpose. Adding `BetterDate` to `module.exports` allows consumers of this code, such as our testing file, to use this constructor function.

Unfortunately moving this function has reverted our test back to a failing (red) state. Running `jasmine` should now fail because `BetterDate` is no longer defined. To fix this we'll need to import the file we just created.

```javascript
let BetterDate = require('../webdev-testing');

describe('BetterDate', function() {
  it('should return the day of the week', function() {
    let betterDate = new BetterDate();
    expect(betterDate.getDay()).toBe('Monday');
  });
});
```

Here we use the `require()` function (also provided to us by Node.js) to import the file we just created setting it to the variable `BetterDate`. We provide `require()` the path to the file `webdev-testing.js` (the `.js` in `require` is optional).

Run `jasmine` again and the test should now be back to a passing (green) state.

## File paths

You may be wondering about the `../` that precedes `webdev-testing` in `require()`.

Since our testing file `webdev-testing.spec.js` lives in `webdev-testing/spec` we need to move up one directory to access `webdev-testing.js`. For that reason we add `../` which gets us one directory higher than where the file lives.

```console
webdev-testing
+-- node_modules
+-- package.json
+-- spec
  +-- webdev-testing.spec.js
+-- webdev-testing.js
```

You can see this in command-line form in your terminal. If you're in `webdev-testing` run the following commands:

```console
cd spec
cd ..
cd spec
cd ..
```

Once you're in `spec` the `..` allows you to move back into `webdev-testing`. The `../` in `require('../webdev-testing');` is doing the same thing.

## Conclusion

Before we finish, add your latest changes to Git.

```console
git init
git add .
```

Then add a commit message.

```console
git commit -m "Add automated testing"
```

Then push these changes up to GitHub.

```console
git push origin master
```

## Exercises

For this exercise, create another test file in the `spec` directory.

```console
touch spec/trainer.spec.js
```

In that file add the following tests.

```javascript
let Trainer = require('../trainer');

describe('Trainer', function() {
  it('should have a name', function() {
    let trainer = new Trainer('Adam');
    expect(trainer.identify()).toEqual('Trainer is Adam');
  });

  it('should capitalize the trainer\'s name', function() {
    let trainer = new Trainer('adam');
    expect(trainer.identify()).toEqual('Trainer is Adam');
  });

  it('should have tasks', function() {
    let trainer = new Trainer('adam', ['Get a Pokemon']);
    expect(trainer.printTasks()).toEqual('Remaining tasks: Get a Pokemon');
  });

  it('should default to an empty array if no tasks are provided', function() {
    let trainer = new Trainer('adam');
    expect(trainer.tasks).toEqual([]);
  });

  it('should print a message if no tasks remain', function() {
    let trainer = new Trainer('adam');
    expect(trainer.printTasks()).toEqual('No tasks left!');
  });

  it('should add new tasks to the tasks array', function() {
    let trainer = new Trainer('adam', ['Get a Pokemon']);
    trainer.addTask('Visit Professor Oak');
    expect(trainer.printTasks()).toEqual('Remaining tasks: Get a Pokemon, Visit Professor Oak');
  });

  it('should remove a task from a tasks', function() {
    let trainer = new Trainer('adam', ['Get a Pokemon', 'Visit Professor Oak']);
    trainer.removeTask('Get a Pokemon');
    expect(trainer.printTasks()).toEqual('Remaining tasks: Visit Professor Oak');
  });

  it('should verify the task exists before attempting to remove it', function() {
    let trainer = new Trainer('adam', ['Get a Pokemon', 'Visit Professor Oak']);
    trainer.removeTask('Beat Brock');
    expect(trainer.printTasks()).toEqual('Remaining tasks: Get a Pokemon, Visit Professor Oak');
  });
});
```

Then create another file in `webdev-testing`.

```
touch trainer.js
```

Within that file, add the rough scaffolding for `Trainer` that's shown below.

```javascript
function Trainer() {
  // Your code here
}

module.exports = Trainer;
```

Run `jasmine` and you'll see 8 failing tests. Write the code for `Trainer` to make all the tests pass.

{% include discord.html %}

If you only want `jasmine` to run the tests within `trainer.spec.js` you can update the `describe` function to `fdescribe`. This will "focus" Jasmine to this set of tests ignoring the tests in other files such as `webdev-testing.spec.js`.

```javascript
let Trainer = require('../trainer');

fdescribe('Trainer', function() {
  ...
});
```

Just remember to change it back to `describe` once you're done to see *all* of your tests in a passing state!

{% include book-plug.html %}

<!-- ANSWER

```javascript
function Trainer(name, tasks = []) {
  this.name = name;
  this.tasks = tasks;
  this.identify = function() {
    return `Trainer is ${this.capitalize(this.name)}`;
  };
  this.capitalize = function(word) {
    return word.slice(0, 1).toUpperCase() + word.slice(1);
  };
  this.printTasks = function() {
    if (this.tasks.length) {
      return `Remaining tasks: ${this.tasks.join(', ')}`;
    } else {
      return 'No tasks left!';
    }
  };
  this.addTask = function(task) {
    this.tasks.push(task);
  };
  this.removeTask = function(task) {
    if (this.tasks.indexOf(task) !== -1) {
      this.tasks.splice(this.tasks.indexOf(task), 1);
    }
  };
}

module.exports = Trainer;
``` -->