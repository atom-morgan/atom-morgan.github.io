---
title: 'Reactive Forms in Angular v6'
date: '2018-11-18'
layout: post
tag: tutorial
excerpt: >-
  In this tutorial we'll learn the basics of Angular's other approach to forms, reactive forms, by creating a user signup page with the same three fields: name, email, and password.
image: '/images/reactive-forms/tutorial-cover.png'
thumb_image: '/images/reactive-forms/tutorial-cover.png'
style: code
---

## Introduction

In a [previous tutorial](https://atom-morgan.github.io/template-driven-forms-in-angular/){:target="_blank"} we took a closer look at template-driven forms. This is one of two approaches available to us as developers to create forms in Angular.

In this tutorial we'll learn the basics of Angular's other approach to forms, reactive forms. Similar to the previous tutorial, we'll do this by creating a user signup page with the same three fields: name, email, and password.

## Setup

If you haven't already, you'll first need to install the [Angular CLI](https://cli.angular.io/){:target="_blank"}.

```console
npm install -g @angular/cli
```

Once the CLI is installed, you can then create a new application.

```console
ng new reactive-forms
cd reactive-forms
```

Once your application has been created, you can run the application.

```console
ng serve
```

Open a browser to `http://localhost:4200` and you should see the default Angular application as shown below.

![Default Angular application](/images/reactive-forms/ng-new.png)

## FormsModule

Now that our application is created and ready, we can start working towards creating a reactive form. Similar to template-driven forms, reactive forms are also in their own module so we'll need to import it.

First, import `ReactiveFormsModule` in `app.module.ts` and then add it to `imports`.

```typescript
import { ReactiveFormsModule } from '@angular/forms';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    ReactiveFormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

With `ReactiveFormsModule` imported we're now ready to move on to creating our form.

{% include request-tutorial.html %}

## Registration Form

Once again we'll begin by updating `app.component.html` with the most basic structure of our reactive form. From there, we'll continue to add more functionality to our form.

```html
<form [formGroup]="reactiveForm">
  <label class="label">Name</label>
  <input class="input"
         type="text"
         formControlName="name">

  <label class="label">Email</label>
  <input class="input"
         type="text"
         formControlName="email">

  <label class="label">Password</label>
  <input class="input"
         type="password"
         formControlName="password">

  <button type="submit">Submit</button>
</form>
```

Within this form we have three inputs for name, email, and password along with a submit button.

Let's breakdown some of the new syntax within this form.

* `formControlName="name"` - This is what's known as a `FormControl` and it's the basic building block of reactive forms. We set it equal to the name of whatever input it is that we're creating. In this case we have three `FormControls`: `name`, `email`, and `password`.
* `[formGroup]="reactiveForm"` - This directive allows us to group the `FormControls` within our form. We add the `[formGroup]` directive to our `form` element and give it a name such as `reactiveForm`.

At this point, our form template is setup correctly but we still have one small issue. Open the app in a browser, open the developer console, and you should see an error message such as "Error: formGroup expects a FormGroup instance. Please pass one in.".

![No formgroup instance](/images/reactive-forms/no-form-group.png)

As mentioned earlier, our form and its fields are composed of a `FormGroup` and `FormControls`. However, in order for our component and template to communicate with one another we need to setup our `FormGroup` and `FormControls` in our component.

## Build Reactive Form

To fix this error, we'll first need to add imports for `FormBuilder` and `FormGroup` to our component. Since `FormBuilder` is a service we inject that into our component's constructor as well.

```typescript
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup } from '@angular/forms';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {
  reactiveForm: FormGroup;

  constructor(private fb: FormBuilder) {

  }

  ngOnInit() {

  }

}
```

In addition to our imports, we also add a component property `reactiveForm` of type `FormGroup`. This property name, `reactiveForm`, must be the same name we provided in our template to the directive `[formGroup]`.

Now that our imports and component property is set we can create the `FormGroup` object to address our earlier error.

```typescript
ngOnInit() {
  this.createForm();
}

createForm() {
  this.reactiveForm = this.fb.group({
    name: [''],
    email: [''],
    password: ['']
  });
}
```

Here we've created a `createForm` function that will create the `FormGroup` object using the `FormBuilder` service. We set our `reactiveForm` property to a new `FormGroup` instance by calling `.group()`. Within this we provide an object of `FormControls`.

The key for each `FormControl` maps to the names we provided in our template to `formControlName`. After each key is an array with its first value, an empty string, being the initial value for the form control.

> It's worth noting you can import `FormControl` and create them explicitly as shown [here](https://angular.io/api/forms/FormControl#usage-notes){:target="_blank"}. However, when using `FormBuilder` you can just use the object passed in to `.group()` as shorthand.

Now when you go back to your browser you should have a form visible with no errors now that our `FormGroup` has been initialized.

![Fix for no formgroup instance](/images/reactive-forms/no-form-group-fix.png)

## Submit handler

Now that our app is back to an error-less state, we can wire up our form's `ngSubmit` event to a component function `onSubmit`.

```html
<form [formGroup]="reactiveForm" (ngSubmit)="onSubmit()">
  <label class="label">Name</label>
  <input class="input"
         type="text"
         formControlName="name">

  <label class="label">Email</label>
  <input class="input"
         type="text"
         formControlName="email">

  <label class="label">Password</label>
  <input class="input"
         type="password"
         formControlName="password">

  <button type="submit">Submit</button>
</form>
```

Then we create our `onSubmit` function in `AppComponent`.

```typescript
onSubmit() {
  console.log('reactiveForm' , this.reactiveForm.value);
}
```

Now when you fill out the form and submit, you should see your form's values logged to the console.

![Form submission](/images/reactive-forms/form-submit.png)

## Validation

We now have a reactive form that's functional and calling a component function when submitted. To wrap up, we'll add some validation to our form to prevent the user from submitting until all fields are valid.

First, we'll need to import `Validators` which will allow us to set specific validation rules for `FormControls`.

```typescript
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
```

Then, we can update the `FormControls` by adding a second value to each array with the value `Validators.required`. As you might be able to guess, this will ensure each field has a value before it's considered "valid".

```typescript
createForm() {
  this.reactiveForm = this.fb.group({
    name: ['', Validators.required],
    email: ['', Validators.required],
    password: ['', Validators.required]
  });
}
```

Now we can leverage these validators to disable our form's submit button until the form is valid.

```html
<button type="submit" [disabled]="!reactiveForm.valid">Submit</button>
```

Now when you go back to your browser, the "Submit" button should be disabled until you enter a value into every required field.

## More validation!

It's not uncommon at all for inputs within a form to have *multiple* requirements to fulfill before they can be considered valid. Thankfully, the `FormBuilder` service makes this easy for us.

```typescript
createForm() {
  this.reactiveForm = this.fb.group({
    name: ['', Validators.required],
    email: ['', [Validators.required, Validators.email]],
    password: ['', [Validators.required, Validators.minLength(6)]]
  });
}
```

To add more validators to a `FormControl` we simply add any additional validators to the desired control's second argument. As you can see, if a `FormControl` has more than one validator we simply provide it an array of validators instead.

> You can see a complete list of Angular's built-in validators [here](https://angular.io/api/forms/Validators){:target="_blank"}.

## Error messages

Now that we have multiple validators on some of our form's controls, it might be a good idea to provide the user some helpful error messages if their provided value isn't valid.

```html
<form [formGroup]="reactiveForm" (ngSubmit)="onSubmit()">
  <label class="label">Name</label>
  <input class="input"
         type="text"
         formControlName="name">
  <p *ngIf="name.invalid && !name.pristine">Name is required.</p>

  <label class="label">Email</label>
  <input class="input"
         type="text"
         formControlName="email">
  <div *ngIf="email.invalid && !email.pristine">
    <p *ngIf="email.errors.email">Valid email is required.</p>
  </div>

  <label class="label">Password</label>
  <input class="input"
         type="password"
         formControlName="password">
  <div *ngIf="password.invalid && !password.pristine">
    <p *ngIf="password.errors.minlength">Password should be at least 6 characters.</p>
  </div>

  <button type="submit" [disabled]="!reactiveForm.valid">Submit</button>
</form>
```

Below each input we've added a conditional error message. For an input like name, we display the message if the value is `invalid` and its control isn't `pristine` meaning its value has changed.

> You can see a full list of `FormControl` properties such as `pristine` and their descriptions [here](https://angular.io/api/forms/AbstractControl#properties){:target="_blank"}.

For inputs such as email and password that have multiple validators there's an extra step. Similar to name we only display the messages if the value is invalid and if the controls' values has changed.

Then we add another conditional for the specific validation error on the `FormControl`'s `errors` property. In the case of email it's `*ngIf="email.errors.email"` and for password it's `*ngIf="password.errors.minlength"`.

> Yes, unfortunately the validator name is `minLength` but the property name on `errors` is `minlength`.

At this point, open up your browser, open the developer console, and you should see an error like "ERROR TypeError: Cannot read property 'invalid' of undefined".

## Getters

This error is due to our `FormControls` such as `name` and `email` not being accessible through our template.  To fix this, we'll create getter methods in our component to retrieve the specific controls within our form.

```typescript
onSubmit() {
  console.log('reactiveForm' , this.reactiveForm.value);
}

get name() { return this.reactiveForm.get('name'); }
get email() { return this.reactiveForm.get('email'); }
get password() { return this.reactiveForm.get('password'); }
```

Now when you go back to your browser, you should see error messages displayed in real-time as you type if they fail to meet the requirements specified by the validators.

![Error messages](/images/reactive-forms/error-messages.png)

## Add CSS

At this point our form is complete but it could use some visual work.

> Since this is outside the scope of this tutorial I won't explain everything in detail but you can read my Bulma tutorial [here](https://atom-morgan.github.io/styling-angular-v6-apps-with-bulma/){:target="_blank"}.

First, we'll update our template with some of Bulma's classes.

```html
<section class="section">
  <div class="columns is-centered">
    <div class="column is-one-third">
      <form [formGroup]="reactiveForm" (ngSubmit)="onSubmit()">
        <div class="field">
          <label class="label">Name</label>
          <input class="input"
                  type="text"
                  formControlName="name">
          <p *ngIf="name.invalid && !name.pristine" class="is-size-6 has-text-danger">Name is required.</p>
        </div>

        <div class="field">
          <label class="label">Email</label>
          <input class="input"
                 type="text"
                 formControlName="email">
          <div *ngIf="email.invalid && !email.pristine">
            <p *ngIf="email.errors.email" class="is-size-6 has-text-danger">Valid email is required.</p>
          </div>
        </div>

        <div class="field">
          <label class="label">Password</label>
          <input class="input"
                 type="password"
                 formControlName="password">
          <div *ngIf="password.invalid && !password.pristine">
            <p *ngIf="password.errors.minlength" class="is-size-6 has-text-danger">Password should be at least 6 characters.</p>
          </div>
        </div>

        <button type="submit" [disabled]="!reactiveForm.valid" class="button is-primary">Submit</button>
      </form>      
    </div>
  </div>
</section>
```

Then we'll install Bulma.

```console
npm install bulma --save
```

Once that has finished, we can add it to our project by updating the `styles` property in `angular.json`.

```json
{
  ...
  "styles": [
    "node_modules/bulma/css/bulma.min.css",
    "src/styles.css"
  ],
  ...
}
```

Restart your app using `ng serve`, open it up in a browser, and you should see your reactive form styled with Bulma.

![Bulma styling](/images/reactive-forms/bulma-styling.png)

## Conclusion

You've now learned the basics of reactive forms in Angular.

If you found this valuable and would like to see more, feel free to let me know <a href="https://twitter.com/atommorgan" target="_blank">on Twitter</a>.

{% include book-plug.html %}