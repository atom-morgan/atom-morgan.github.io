---
title: "Clearing Forms in Angular v6"
date: '2017-04-16'
layout: post
tag: tutorial
image: '/images/clearing-forms-in-angular/tutorial-clearing-forms.png'
thumb_image: '/images/clearing-forms-in-angular/tutorial-clearing-forms.png'
excerpt: >-
  Thankfully the solution to this problem is easy and takes just two lines of real code to get the job done.
style: code
---

I was recently working on an Angular (aka Angular2 aka Angular4 aka Angular 6) application where a user triggers a modal, completes a form within the modal, and submits. Unfortunately, the form wasn't clearing after a form submission and manually resetting every `ngModel` seemed extremely tedious. Thankfully the solution to this problem is easy and takes just two lines of real code to get the job done.

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
    <span *ngIf="submittedUser">{% raw %}{{submittedUser}}{% endraw %}</span>
  `
})
export class AppComponent {
  submittedUser: string;
  name: string;
  title: string;

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

<iframe src="https://stackblitz.com/edit/clearing-forms-problem?ctl=1&embed=1&file=src/main.ts&hideExplorer=1&view=preview" frameborder="0" width="100%" height="500"></iframe>

To fix this, we'll need to leverage Angular's [ViewChild](https://angular.io/docs/ts/latest/api/core/index/ViewChild-decorator.html){:target="_blank"} decorator. `ViewChild` allows us to query our view to get an element or directive of our choice. In this case, we want a `ViewChild` for our form.

{% include request-tutorial.html %}

To do this, we'll need to import `ViewChild` (Thanks to [Chris Sevilleja](https://twitter.com/chrisoncode){:target="_blank"} for introducing me to ViewChild).

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
  name: string;
  title: string;

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

<iframe src="https://stackblitz.com/edit/clearing-forms-solution?ctl=1&embed=1&file=src/main.ts&hideExplorer=1&view=preview" frameborder="0" width="100%" height="500"></iframe>

{% include book-plug.html %}
