---
title: "Code Companion #1: What is Web Development?"
date: '2018-05-26'
layout: post
tag: [tutorial, cc]
excerpt: >-
  In this tutorial we're going to clarify web development terms such as HTML, CSS, JavaScript, server-side programming, client-side programming, databases, JSON, and so on to give you a grasp of how everything fits together at a high level.
image: "/images/what-is-web-dev/tutorial-what-is-web-dev.png"
thumb_image: "/images/what-is-web-dev/tutorial-what-is-web-dev.png"
style: code
---

## Video

<div class="videoWrapper">
  <iframe width="560" height="315" src="https://www.youtube.com/embed/qgkEjEA23L4" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
</div>

## Goal

I'll never forget the overwhelming feeling when I first started learning web development. There were terms I'd never heard of, multiple programming languages, front-end development, back-end development, and web frameworks.

Learning how each of these various pieces fit together wasn't easy and there was always the fear that I'd be wasting my time learning something I didn't need to learn.

In this tutorial we're going to clarify web development terms such as HTML, CSS, JavaScript, server-side programming, client-side programming, databases, JSON, and so on to give you a grasp of how everything fits together at a high level.

{% include request-tutorial.html %}

## How does the web work?

First we need to start with how the web works.

Let's think about a site like Twitter. When you access someone's Twitter account, let's use @realDonaldTrump as an example, the main content you see is a feed of tweets from Donald Trump. You see a feed of Trump's tweets by accessing his Twitter URL: [https://twitter.com/realDonaldTrump](https://twitter.com/realDonaldTrump){:target="_blank"}.

When you enter this URL into the browser you've made what's known as an HTTP request to Twitter's servers. That HTTP request is then received by Twitter and it's their job to handle this request. In this case Twitter sees you're trying to access @realDonaldTrump's account so they query their database to get any of @realDonaldTrump's data that is needed to view this page such as his name, tweets, following count, follower count, and any other information that's used to populate his page.

Once Twitter finishes querying their database for this information, they respond to the browser's request (the one you originally made when you entered in the URL [https://twitter.com/realDonaldTrump](https://twitter.com/realDonaldTrump){:target="_blank"}) by sending back the data from their database. 

<div class="box" markdown="1">
If you've ever seen or heard the term [JSON](https://en.wikipedia.org/wiki/JSON), that's the file format that's typically used to send this data but more on that in a later tutorial.
</div>

Once this data is received by the browser, the HTML page you see in the browser is then constructed using the data sent back from Twitter.

![Trump's Twitter](/images/what-is-web-dev/trump-twitter.png)

## Client server model

This model—making a request to a site like Twitter and receiving data in response—is what's known as a client-server model. We can see the previous example illustrated in the following image where the client is a web browser and the server is Twitter.

<div class="box" markdown="1">
We use the term "client" here instead of something like "computer" to be a bit more general because the number of devices that can act as clients continues to grow. You can now access Twitter from your laptop, tablet, smartphone, TV, video game consoles, etc. All of these would classify as a "client" in the client-server model.
</div>

![Client-Server User POV](/images/what-is-web-dev/client-server-user.png)

This shows a client-server model at a nearly technology agnostic level. We haven't even talked programming languages or back-end vs front-end yet. So considering that all of these are used to make web applications such as Twitter, how do they fit in to this client-server model?

![Client-Server Developer POV](/images/what-is-web-dev/client-server-dev.png)

As you can see our previous labels for "Browser" and "Twitter" have been replaced with some underlying technologies that are used such as "React" and "Node.js". This is where some of the various frameworks you may have heard of start to come in.

In this hypothetical example, Twitter's front-end application is built using React. This React application makes requests to a Node.js back-end that uses a MongoDB database to store any data.

## Front end vs. back end

Any code that's running on a browser is "front-end" code using frameworks such as React, Angular, Vue.js, or Ember. Front-end developers write code that's executed on a client, such as a web browser. It displays a web page to the user using HTML, CSS, and JavaScript.

Any code that's running on a server is "back-end" code using frameworks such as Ruby on Rails, Node.js, Laravel, or Django. Back-end developers write code that's executed on a server responding to requests made by clients. Back-end developers will also work with a database if necessary to fulfill those requests.

The distinction between front-end and back-end is delineated by the client that's **<em>making the request</em>** and the server that's sending a **<em>response to the request</em>** back to the client.

Front-end is what you see, back-end is what you don't see.

{% include email-list.html %}

## Frequently Asked Questions

Once the division between front-end and back-end has been established, there are a couple common questions about programming languages and frameworks that I typically see such as these:

**Should I learn a language before I learn a framework?**

Yes. If you're going to learn a framework it's best to start off with the programming language that's used within the framework first.

If you're considering front-end then learning HTML, CSS, and JavaScript is a must-have. Since so much of front-end development happens in a browser, you have to learn how to work with browser technology such as HTML, CSS, and JavaScript. Once you've gotten a grasp of these then you can move on to one of the popular front-end frameworks listed below: 

* [Angular](https://angular.io/){:target="_blank"}
* [React](https://reactjs.org/){:target="_blank"}
* [Ember.js](https://www.emberjs.com/){:target="_blank"}
* [Vue.js](https://vuejs.org/){:target="_blank"}

If you're considering back-end you don't have to worry about HTML and CSS as much since your code isn't directly interfacing with a browser. Instead, you'll likely be replacing this with knowledge of databases since you're going to be saving, updating, and retrieving data from databases. Here are a few popular options for databases:

* [PostgreSQL](https://www.postgresql.org/){:target="_blank"}
* [SQL Server](https://www.microsoft.com/en-us/sql-server/sql-server-2017){:target="_blank"}
* [MySQL](https://www.mysql.com/){:target="_blank"}
* [MongoDB](https://www.mongodb.com/){:target="_blank"}

In addition to that, you will also need to learn a programming language along with a framework. A few of the current popular options are below:

* [Ruby on Rails](https://rubyonrails.org/){:target="_blank"} (Ruby)
* [Express/Node.js](https://expressjs.com/){:target="_blank"} (JavaScript)
* [Laravel](https://laravel.com/){:target="_blank"} (PHP)
* [Django](https://www.djangoproject.com/){:target="_blank"} (Python)
* [ASP.NET](https://www.asp.net/){:target="_blank"} (VB, C#, etc)
* [Spring MVC](https://spring.io/){:target="_blank"} (Java)

**If I learn Angular do I need to learn React?**

No. Once you're proficient with a framework, you have the skillset you need to do back-end or front-end work with whatever framework it is that you know.

<div class="box" markdown="1">
While there are some differences between frameworks, the job the frameworks were created to do is the exact same job so frameworks are interchangeable. If you know Ruby on Rails, you don't need to learn Express. They do the exact same thing using different programming languages.
</div>

**So if I know JavaScript, why do I need to learn a framework? What does a framework even do?**

If you've ever played a video game with a built-in level creator, you can think of frameworks as a level creator for the web. In a video game's level creator, there are certain assets available for you to use such as structures, textures, enemies, items, and so on. You may even be given the option to build a structure from base components to create a new structure that you can re-use and even share with others. Web frameworks essentially do the same thing. But rather than providing us textures and structures that exist within the game's universe, we're given the various tools we need to create modern web applications such as routing, form builders, form validation, and services to abstract many of the details of making HTTP request from us. It provides us the basic building blocks for creating a web application.

This is the reason why it's always recommended to learn a language first, then a framework. Walk first, then run.

**What is a tech stack?**

A tech stack is simplay any variation of the three groups we described above: databases, back-end frameworks, and front-end frameworks. Since each framework or database generally does the same thing, that gives you (or a development team) the freedom to choose whatever tool you prefer for the job.

Here are a few more sample "tech stacks" using some of the options listed earlier.

* MongoDB — Ruby on Rails — Angular
* PostgreSQL — Express — Vue.js
* MySQL — Ruby on Rails — Ember.js

**I'm not sure if I want to do front-end or back-end. What should I do first so I don't waste any time learning?**

Learn the fundamentals of programming and a programming language. Since frameworks (both back-end and front-end) are largely interchangeable, the underlying capabilities of the languages are mostly the same.

This is why you may hear programmers occasionally say once you know one language you know them all. This isn't 100% true but there is some truth to the saying. Programmers from one language can usually at least read and figure out what's going on in a language they've never used. This is because the fundamentals between programming languages, and frameworks, are largely the same.

## Conclusion

At this point, I hope you have a better understanding of how the web works at a high level and how the various pieces of web development fit together.

We've seen how front-end developers write code that's executed on clients which make requests to servers. Once this request is received by a server, back-end developers write code to handle those requests sending back data to create web applications.

We also introduced some of the various frameworks used by front-end and back-end developers as well as some of the databases you can expect to work with as a back-end developer.

In the [next tutorial](https://atom-morgan.github.io/hello-world-in-javascript/){:target="_blank"} we'll begin our introduction to web development by writing a simple "Hello, world!" program using HTML and JavaScript.

{% include book-plug.html %}