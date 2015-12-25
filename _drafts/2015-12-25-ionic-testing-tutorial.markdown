---
layout: post
title: "Add Automated Testing to Your Ionic App"
date: 2015-12-25
---

I recently started a cool project at work building out a mobile app using the Ionic framework. With this project, integrating automated testing was an absolute necessity. A surprising number of development teams I've worked with in the past had little to no automated testing built into their applications and I finally wanted this to change. So I got my hands dirty, entered the world of automated testing, and decided to share what I've learned with anyone who needs it.

Since the majority of my testing experience has been on this Ionic project, I'll be showing you how to integrate automated testing with an application using the Ionic framework.

For our tests, we'll be using Karma and Protractor as our test runners **and ........**

##### What To Expect

By the end of this tutorial you should be able to create an Ionic application, setup Karma and Protractor, and add test files (unit and end-to-end) that will be captured by Karma or Protractor to help test your application. This initial tutorial will not be a comprehensive testing tutorial. I'm trying to give you a push in the right direction so you're not overwhelmed by all of the setup and configuration that's necessary to write your first test.

Think of it as a to-do app tutorial for testing. Those tutorials show you how to set everything up and highlight a few basic features of the framework. I'll be doing the same here. I'll help you set everything up and test the existing features in one of the starter apps Ionic provides us. From there, I plan to provide more in-depth testing tutorials.

##### What You Should Know

Ionic apps are built using the [AngularJS](https://angularjs.org/) framework so a basic understanding of AngularJS, and all the JavaScript that goes along with it, is really all you need to know. You don't even need prior experience with Ionic.

##### Setup and Installation

Since Cordova and Ionic require Node.js, [installing Node](https://nodejs.org/) will be your first step.

Once Node is installed, install Cordova and Ionic.

```
npm install -g cordova ionic
```

You can verify everything was installed correctly by checking each installation's version numbers.

```
node -v
cordova -v
ionic -v
```

##### Create Your Project

Next, we'll use one of Ionic's pre-made app templates to get us started. We're focusing on testing for this tutorial so building an Ionic app from scratch will be reserved for another tutorial.

```
ionic start myApp tabs
```

Feel free to replace `myApp` in the command above with whatever name you wish to name your project.

##### Run Your Project

Once your project has been created, `cd` into that directory and verify everything is working correctly by serving it up in a browser.

```
cd myApp
ionic serve
```

After you run `ionic serve` a browser window should automatically open displaying the starter application your just created.

![Ionic Starter App](http://i.imgur.com/o2jyyHZ.png)

##### Karma Setup

First you'll need to install Karma and its dependencies along with the Karma CLI. Since the testing suite is a part of your project, we'll add `--save-dev` so it's added to your project's `package.json`'s `devDependencies`.

```
npm install karma --save-dev
npm install karma-jasmine karma-chrome-launcher --save-dev
npm install karma -g karma-cli
```

Now we need to setup our configuration file for Karma. This specifies the testing framework we're going to use (Jasmine - we installed it in the step above) along with location of the files we want to test.

To create the Karma configuration file we can simply run `karma init` in our project's root directory. This will display a series of questions to you to help setup your Karma configuration file. I've listed the options I selected below. If the area to the right of the `*` is empty, I simply hit `return`.

```
Which testing framework do you want to use?
*Jasmine

Do you want to use require.js?
*No

Do you want to capture any browsers automatically?
*Chrome
*

What is the location of your source and test files?
*

Should any of the files included by the previous patterns be excluded?
*

Do you want Karma to watch all the files and run the tests on change?
*yes
```

After providing answers to these questions, you should see a message stating that your file has been created. Open it up and you should see something similar to this:

*Note: I removed the default comments within the file to keep this short*

```
module.exports = function(config) {
  config.set({
    basePath: '',
    frameworks: ['jasmine'],
    files: [
    ],
    exclude: [
    ],
    preprocessors: {
    },
    reporters: ['progress'],
    port: 9876,
    colors: true,
    logLevel: config.LOG_INFO,
    autoWatch: true,
    browsers: ['Chrome'],
    singleRun: false,
    concurrency: Infinity
  })
}
```

##### Karma File Setup

Earlier during the Karma setup we were asked for the location of our source and test files. We skipped it then but now we need to add those files so our tests will run correctly. We'll have to supply it the Angular source code, UI Router, **Angular Mocks?**, our Ionic application, and finally our test files.

Thanks to some work on Ionic's end, we can add Angular's source code along with UI router with one line. So open up your `karma.conf.js` file and update the `files` property with its first file path:

```
files: [
  "www/lib/ionic/js/ionic.bundle.js"
],
```

If you're curious, open that file in a text editor and you should see a comment at the top. Ionic lets us know this file is a concatenation of many files including `angular.js` and `angular-ui-router.js`, just what we need.

In addition to that, we'll also need to include our Ionic application file.

```
files: [
  "www/lib/ionic/js/ionic.bundle.js",
  "www/js/app.js"
],
```

##### Hello, Testing World

We're finally ready to create our first test file and write our first test. Similar to a "Hello, world" application, we're not going to test our application just yet. We just want to add a test file and write a very simple test to make sure everything is working. To do this create a new file `controllers.test.js` in `www/js/`. Your `www` directory should resemble the one below:

```
|-www
  |--js
    |--app.js
    |--controllers.js
    |--controllers.test.js
    |--services.js
```

If everything is correct, open `controllers.test.js` in a test file and write your first test. We want a very simple test here just so we know Karma is running everything correctly. Add this and save your file.

```
describe("My first test file", function() {
  it("should pass with basic math", function() {
    expect(2 + 2).toEqual(4);
  });
});
```

Now run `karma start` in your project's root directory and you should see a passing test.

![First Karma Test](http://i.imgur.com/j4tWOS0.png)

This is great but how do we know that's not a false positive? Let's add another test we *expect* to fail just to give us some peace of mind with our very first test. With our new intentionally broken test our `controllers.test.js` file now looks like this.

```
describe("My first test file", function() {
  it("should pass with basic math", function() {
    expect(2 + 2).toEqual(4);
  });

  it("should fail with Orwellian math", function() {
    expect(2 + 2).toEqual(5);
  });
});
```

Save the file, run `karma start` again, and you should now see a failing test. Jasmine has no Party influence.

![First Failed Karma Test](http://i.imgur.com/GWF0Ii5.png)

*Note: The text I'm passing as my first argument into `describe` and `it` are written in a way to make the log outputs easy to read. When a test fails, the `describe` and `it` text are concatenated together into the red text after your tests finish running so you know the exact test that failed.*
