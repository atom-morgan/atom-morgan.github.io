---
title: 'How to Mock an API in Angular v6'
date: '2018-07-15'
layout: post
tag: tutorial
excerpt: >-
  In this tutorial you'll learn how to setup an Angular v6 application using the Angular CLI and mock an observable API for your application.
image: '/images/mocking-angular-api/tutorial-cover.png'
thumb_image: '/images/mocking-angular-api/tutorial-cover.png'
style: code
---

## Introduction

In this tutorial you'll learn how to setup an Angular v6 application using the Angular CLI and mock an observable API for your application.

When I first started learning front-end development using AngularJS, I would work through tutorials and felt I had a grasp on the basics to start working on my own applications. However, most tutorials use synchronous data and I at least wanted the *feeling* of hitting an API even if I wasn't. If you're looking to do the same, this tutorial is for you.

## Setup

If you haven't already, you'll first need to install the [Angular CLI](https://cli.angular.io/){:target="_blank"}.

```console
npm install -g @angular/cli
```

If you never worked with AngularJS, you have no idea how much easier this CLI makes it for beginners who are just starting out. I wish there was an equivalent back then!

Once the CLI is installed, you can then create a new application.

```console
ng new mocked-api
cd mocked-api
```

Once your application has been created, you can run the application.

```console
ng serve
```

Open a browser to `http://localhost:4200` and you should see the default Angular application as shown below.

![Default Angular application](/images/mocking-angular-api/starter-app.png)

## Creating a service

Now we can move on to creating a service for our observable which will serve as our mocked API.

```console
ng g service services/fighters/fighters
```

Here we've created a new Angular service prefixing our service name with a new path: `services/fighters`. I prefer to keep my services grouped together within a `services` directory. Without it, Angular would default to placing services into `src/app` and that directory can get polluted rather quickly.

Open our new service in `src/app/services/fighters` and you should see the auto-generated service below.

```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class FightersService {

  constructor() { }
}
```

Within this file we need to import `of` from `rxjs`. This is what allows us to create an observable—the [default return value](https://angular.io/tutorial/toh-pt6#http-methods-return-one-value){:target="_blank"} in Angular for HTTP requests.

```typescript
import { of } from 'rxjs';
```

From here, we add a new `fighters` property to the service containing the data we want to serve as our mocked response. Then we add a `get` method that returns `fighters` as an observable by leveraging `of`.

```typescript
export class FightersService {
  fighters: Array<object> = [
    { name: 'Conor McGregor', wins: 21, losses: 3 },
    { name: 'Tony Ferguson', wins: 23, losses: 3 },
    { name: 'Max Holloway', wins: 19: losses: 3 },
    { name: 'Jon Jones', wins: 22, losses: 1 },
    { name: 'Daniel Cormier', wins: 21, losses: 1 },
    { name: 'Brock Lesnar', wins: 5, losses: 3 }
  ];

  constructor() { }

  get() {
    return of(this.fighters);
  }
}
```

Now our service is ready to be consumed by our application. Within `app.component.ts` we can import our new service as well as the `OnInit` lifecycle hook from `@angular/core`.

```typescript
import { Component, OnInit } from '@angular/core';
import { FightersService } from './services/fighters/fighters.service';
```

Now we can update `AppComponent` to consume our service.

```typescript
export class AppComponent implements OnInit {
  title = 'app';
  fighters: Array<object>;

  constructor(private fightersService: FightersService) { }

  ngOnInit() {
    this.fightersService.get().subscribe(res => {
      this.fighters = res;
    });
  }
}
```

First we create a new `fighters` property which we'll set to the result of our service call. Then we use dependency injection to inject our service into the component's constructor.

Then we update `AppComponent` to implement `OnInit` and add the `ngOnInit` lifecycle hook. Within `ngOnInit` we call our service method, subscribe to the observable, and set the result to `fighters`.

{% include request-tutorial.html %}

Now that our component has data to render to the view, we can update the template in `app.component.html` (I add this in place of the existing "links to help you start". Feel free to replace the entire template if you want).

```html
<table>
  <thead>
    <th>Name</th>
    <th>Wins</th>
    <th>Losses</th>
  </thead>
  <tbody>
    <tr *ngFor="let fighter of fighters">
      <td>{% raw %}{{fighter.name}}{% endraw %}</td>
      <td>{% raw %}{{fighter.wins}}{% endraw %}</td>
      <td>{% raw %}{{fighter.losses}}{% endraw %}</td>
    </tr>
  </tbody>
</table>
```

To wrap this up, we'll add some CSS to `app.component.css` to clean up our table a bit.

```css
table {
  width: 50%;
  margin: 0 auto;
  border-collapse: collapse;
}

td, th {
  border: 1px solid #DDD;
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #DDD;
}
```

Save that and you should now see a table displaying data using our mocked API.

![Angular application displaying mocked data](/images/mocking-angular-api/final-app.png)

## Conclusion

You've now learned how to create an app using the CLI, create a new service, and mock an API using `of`.

If you found this valuable and would like to see more, feel free to let me know <a href="https://twitter.com/atommorgan" target="_blank">on Twitter</a>.

{% include book-plug.html %}