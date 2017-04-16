---
layout: post
title: "Angular Tutorial: Clearing Forms"
date: 2017-04-16
---

I was recently working on an Angular (aka Angular2 aka Angular4) application where a user triggers a modal, completes a form within the modal, and submits. Unfortunately, the form wasn't clearing after a form submission and manually resetting every `ngModel` seemed extremely tedious. Thankfully the solution to this problem is easy and takes just two lines of real code to get the job done.

First, we'll start with a basic component.

```javascript
// src/app.component.ts

import { Component } from '@angular/core';
import { DummyService } from './dummy.service';

@Component({
  selector: 'app-root',
  template: `
    <form #myForm="ngForm" (ngSubmit)="submitForm(myForm.value)">
      <label for="name">Name: </label>
      <input placeholder="Name"
        type="text"
        id="name"
        name="name"
        [(ngModel)]="name">

      <label for="name">Title: </label>
      <input placeholder="Title"
        type="text"
        id="title"
        name="title"
        [(ngModel)]="title">

      <button type="submit">Submit</button>
    </form>
    <span *ngIf="submittedUser">{{submittedUser}}</span>
  `
})
export class AppComponent {
  submittedUser: string;

  constructor(private dummyService: DummyService) { }

  submitForm(form) {
    this.dummyService.create(form).subscribe((res) => {
      this.submittedUser = res.message;
    });
  }

}
```

In this component we've provided a form template with two inputs: `name` and `title`. Within the component class we've defined a private `dummyService` which we'll use as a mock service to "`POST`" our form. When the user clicks "Submit", `submitForm` will call `this.dummyService.create()` and we set the response to the `submittedUser` property which conditionally displays in our view through `<span *ngIf="submittedUser">`.

Complete the form below, click "Submit", and you should see the conditional `<span>` display our response but the form inputs are still populated.

<iframe src="https://embed.plnkr.co/BTT0VX/?show=preview" frameborder="0" width="100%" height="500"></iframe>

To fix this, we'll need to leverage Angular's [ViewChild](https://angular.io/docs/ts/latest/api/core/index/ViewChild-decorator.html) decorator. `ViewChild` allows us to query our view to get an element or directive of our choice. In this case, we want a `ViewChild` for our form.

To do this, we'll need to import `ViewChild`.

```javascript
// src/app.component.ts

import { Component, ViewChild } from '@angular/core';
```

Then in our component class, we can set a `ViewChild` property to our form's template variable `myForm` which we declared earlier: `<form #myForm="ngForm" (ngSubmit)="submitForm(myForm.value)">`.


```javascript
// src/app.component.ts

export class AppComponent {
  @ViewChild('myForm') formValues; // Added this
  submittedUser: string;

  constructor(private dummyService: DummyService) { }

  ...

}
```

Now we can add the one line to clear our form on a successful response.

```javascript
// src/app.component.ts

...

submitForm(form) {
  this.dummyService.create(form).subscribe((res) => {
    this.submittedUser = res.message;
    this.formValues.resetForm(); // Added this
  });
}
```

Try it again below and experience the magic of `resetForm()`.

<iframe src="https://embed.plnkr.co/4WyfEc/?show=preview" frameborder="0" width="100%" height="500"></iframe>


---

Hat tip to [Chris Sevilleja](https://twitter.com/chrisoncode) for introducing me to ViewChild.
