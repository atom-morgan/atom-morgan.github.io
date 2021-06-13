---
title: 'Template-Driven Forms in Angular v6'
date: '2018-11-11'
layout: post
tag: [tutorial, angular, webdev]
excerpt: >-
  In this tutorial we'll learn the basics of template-driven forms in Angular by creating a user signup page with three fields: name, email, and password.
image: '/images/template-forms/tutorial-cover.png'
thumb_image: '/images/template-forms/tutorial-cover.png'
style: code
---

## Introduction

Whether you're creating a Twitter account or paying your bills online, at some point you've interacted with a form on the web. In Angular, we're given two different approaches to developing forms: template-driven and reactive.

In this tutorial we'll learn the basics of template-driven forms in Angular by creating a user signup page with three fields: name, email, and password.

## Setup

If you haven't already, you'll first need to install the [Angular CLI](https://cli.angular.io/){:target="_blank"}.

```console
npm install -g @angular/cli
```

Once the CLI is installed, you can then create a new application.

```console
ng new template-forms
cd template-forms
```

Once your application has been created, you can run the application.

```console
ng serve
```

Open a browser to `http://localhost:4200` and you should see the default Angular application as shown below.

![Default Angular application](/images/template-forms/ng-new.png)

## FormsModule

Now that our application is created and ready, we can start working towards creating a template-driven form. Since template-driven forms are in their own module, we'll need to import it.

First, import `FormsModule` in `app.module.ts` and then add it to `imports`.

```typescript
import { FormsModule } from '@angular/forms';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    FormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

With `FormsModule` imported we're now ready to move on to creating our form.

{% include request-tutorial.html %}

## Registration Form

We'll begin by updating `app.component.html` with the most basic structure of our template-driven form. From there, we'll continue to add more functionality to our form.

```html
<form #templateForm="ngForm" (ngSubmit)="onSubmit(templateForm.value)">
  <div class="field">
    <label class="label" for="name">Name</label>
    <div class="control">
      <input class="input"
            type="text"
            [(ngModel)]="formUser.name"
            name="name"
            #name="ngModel"
            required>
    </div>
  </div>

  <div class="field">
    <label class="label" for="email">Email</label>
    <div class="control">
      <input class="input"
            type="text"
            [(ngModel)]="formUser.email"
            name="email"
            #email="ngModel"
            required>
    </div>
  </div>

  <div class="field">
    <label class="label" for="password">Password</label>
    <div class="control">
      <input class="input"
            type="password"
            [(ngModel)]="formUser.password"
            name="password"
            #password="ngModel"
            required>
    </div>
  </div>  

  <button type="submit" class="button is-primary">Submit</button>
</form>
```

Within this form we have three inputs for name, email, and password along with a submit button.

Let's breakdown some of the new syntax within this form from top to bottom.

* `#templateForm="ngForm"` - By default, Angular attaches the `ngForm` directive to HTML forms. Here, we create a reference to the form with our template reference variable `#templateForm`. This allows us to access the form's values and state which we'll see shortly.
* `(ngSubmit)="onSubmit(templateForm.value)"` - This is what makes our form useful. Here we use Angular's `ngSubmit` event property which is fired when our form is submitted. When the `ngSubmit` event is triggered, it will call the `onSubmit` method (which we'll create in our component) with our form's values. Note how our form's values are passed into `onSubmit` using the template reference variable we created earlier, `templateForm`.
* `[(ngModel)]="formUser.name"` - Fields in our form are bound to properties in our component class using this `[(ngModel)]` syntax which is known as "banana in a box" syntax. This creates two-way data binding between our template and component meaning that if a value is updated in our component, it will be reflected in our template and vice versa.
* `name="email"` - The `name` attribute is a requirement by template-driven forms in Angular for any form field using the `ngModel` directive. I personally prefer to set the `name` attribute to the same name as the `ngModel` value (`[(ngModel)]="formUser.email"`) but this isn't required by Angular.
* `#password="ngModel"` - Similar to our previous template reference variable which set a reference to our form, this creates references for the individual fields within our form which we can use for tracking valid input. Once again, the name doesn't have to match the values for `ngModel` or `name` but I prefer to have them all the same.

At this point, our form template is setup correctly but we still have one small issue. Open the app in a browser, open the developer console, and you should see an error message such as "ERROR TypeError: Cannot read property 'name' of undefined".

![Cannot read property name](/images/template-forms/no-component-property.png)

As mentioned earlier, our form's fields are bound to properties in our component class using the banana in a box syntax, `[(ngModel)]`. Each `ngModel` directive in our form references an object `formUser` which doesn't exist yet.

To fix this we simply need to create a `formUser` property in our component class.

```typescript
export class AppComponent {
  title = 'template-forms'; // You can delete this if you want
  formUser = {};
}
```

Now when you go back to your browser you should have a form visible with no Angular errors displaying in the console.

![Fix for cannot read property name](/images/template-forms/no-component-property-fix.png)

## Event Handler

Now that our form isn't throwing any errors, we can work on handling the `ngSubmit` event of our form. As defined in our form, the `ngSubmit` event will call an `onSubmit` method which we'll add to our component.

```typescript
onSubmit(form) {
  console.log('form values ', form);
}
```

Here we've created a simple `onSubmit` function that logs out the value of `form`. Remember, we receive these form values using our template reference variable in the form of `templateForm.value` which is passed to `onSubmit`.

Now you can go back to your browser, complete the form, click submit, and you should see your input logged to the console.

![Form submission](/images/template-forms/form-submit.png)

## Form State

We now have a working form that successfully calls our `onSubmit` method with the form's values. However, there are some shortcomings to our form.

First, a user can submit the form without completing any fields. Second, if a user submits the form with empty fields, there's no helpful error message to notify them of required fields.

We can get some help with this by leveraging the template reference variables we created earlier. Immediately after our closing form tag, you can add the following markup to get real-time data about our form such as the values and validity of the form.

```html
<pre>Form values: {% raw %}{{ templateForm.form.value | json }}{% endraw %}</pre> <!-- Value of whole form -->
<pre>Valid form? {% raw %}{{ templateForm.form.valid | json }}{% endraw %}</pre> <!-- Validity of whole form -->
```

In addition to the form and its template reference variable, we can also use the reference variables we created for each input as well.

```html
<pre>Form values: {% raw %}{{ templateForm.form.value | json }}{% endraw %}</pre> <!-- Value of whole form -->
<pre>Valid form? {% raw %}{{ templateForm.form.valid | json }}{% endraw %}</pre> <!-- Validity of whole form -->
<pre>Name: {% raw %}{{ name.value }}{% endraw %}</pre> <!-- Value of name field -->
<pre>Email: {% raw %}{{ email.value }}{% endraw %}</pre> <!-- Value of email field -->
```

Finally, in addition to inspecting the values and validity of our form or inputs we can access the state of our inputs as well.

```html
<pre>Name value: {% raw %}{{ name.value }}{% endraw %}</pre>
<pre>Name valid?: {% raw %}{{ name.valid }}{% endraw %}</pre>
<pre>Name pristine?: {% raw %}{{ name.pristine }}{% endraw %}</pre>
<pre>Name touched?: {% raw %}{{ name.touched }}{% endraw %}</pre>
```

Here's a quick description of these states:

* `valid` - The control's value is valid.
* `pristine` - The control's value has not changed.
* `touched` - The control has been focused and then blurred.

Open your browser and interact the form (clicking and typing) to see how these values change as a user interacts with your form.

![Tracking form values](/images/template-forms/tracking-form-values.png)

## Styling and States

Now that we've seen the various states available to us using our template reference variables, let's use them to add some functionality to our form.

First, we'll disable our submit button until the form is valid.

```html
<button [disabled]="!templateForm.form.valid" type="submit" class="button is-primary">Submit</button>
```

To do this we simply set the button's `disabled` property to `!templateForm.form.valid` so that it's only enabled once the form is valid.

Go back to your browser and you should see a disabled submit button. Complete the form and you'll see the button enabled once all fields have a value.

In addition to disabling the submit button, we can use `valid` and `pristine` to show and hide error messages as well.

Just below our input for name we'll add a `p` tag that will be conditionally displayed depending on the state of our input.

```html
<div class="field">
  <label class="label" for="name">Name</label>
  <div class="control">
    <input class="input"
          type="text"
          [(ngModel)]="formUser.name"
          name="name"
          #name="ngModel"
          required>
  </div>
  <p [hidden]="name.valid || name.pristine" class="is-size-6 has-text-danger">Name is required.</p>
</div>
```

Here we set the `hidden` property to the values of `.valid` or `.pristine` meaning if the control's values are valid *or* unchanged our message will not display.

From here we can update our other inputs with similar error messages.

```html
<div class="field">
  <label class="label" for="name">Name</label>
  <div class="control">
    <input class="input"
          type="text"
          [(ngModel)]="formUser.name"
          name="name"
          #name="ngModel"
          required>
  </div>
  <p [hidden]="name.valid || name.pristine" class="is-size-6 has-text-danger">Name is required.</p>
</div>

<div class="field">
  <label class="label" for="email">Email</label>
  <div class="control">
    <input class="input"
          type="text"
          [(ngModel)]="formUser.email"
          name="email"
          #email="ngModel"
          required>
  </div>
  <p [hidden]="email.valid || email.pristine" class="is-size-6 has-text-danger">Email is required.</p>
</div>

<div class="field">
  <label class="label" for="password">Password</label>
  <div class="control">
    <input class="input"
          type="password"
          [(ngModel)]="formUser.password"
          name="password"
          #password="ngModel"
          required>
  </div>
  <p [hidden]="password.valid || password.pristine" class="is-size-6 has-text-danger">Password is required.</p>
</div>
```

Now you should be able to type into your form, remove values, and see error messages appear notifying users of required fields.

![Error messages](/images/template-forms/error-messages.png)

## Add CSS

At this point our form is complete but it could use some visual work.

> Since this is outside the scope of this tutorial I won't explain everything in detail but you can read my Bulma tutorial [here](https://atom-morgan.github.io/styling-angular-v6-apps-with-bulma/){:target="_blank"}.

First, we'll update our template with some of Bulma's classes.

```html
<section class="section">
  <div class="columns is-centered">
    <div class="column is-one-third">
      <form #templateForm="ngForm" (ngSubmit)="onSubmit(templateForm.value)">
        <div class="field">
          <label class="label" for="name">Name</label>
          <div class="control">
            <input class="input"
                  type="text"
                  name="name"
                  [(ngModel)]="formUser.name"
                  #name="ngModel"
                  required>
          </div>
          <p [hidden]="name.valid || name.pristine" class="is-size-6 has-text-danger">Name is required.</p>
        </div>
      
        <div class="field">
          <label class="label" for="email">Email</label>
          <div class="control">
            <input class="input"
                  type="text"
                  name="email"
                  [(ngModel)]="formUser.email"
                  #email="ngModel"
                  required>
          </div>
          <p [hidden]="email.valid || email.pristine" class="is-size-6 has-text-danger">Email is required.</p>
        </div>

        <div class="field">
          <label class="label" for="password">Password</label>
          <div class="control">
            <input class="input"
                  type="password"
                  [(ngModel)]="formUser.password"
                  name="password"
                  #password="ngModel"
                  required>
          </div>
          <p [hidden]="password.valid || password.pristine" class="is-size-6 has-text-danger">Password is required.</p>
        </div>
      
        <button [disabled]="!templateForm.form.valid" type="submit" class="button is-primary">Submit</button>
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

Restart your app using `ng serve`, open it up in a browser, and you should see your template-driven form styled with Bulma.

![Bulma styling](/images/template-forms/bulma-styling.png)

## Conclusion

You've now learned the basics of template-driven forms in Angular. In the next tutorial we'll recreate this form using Angular's reactive form approach.

If you found this valuable and would like to see more, feel free to let me know <a href="https://twitter.com/atommorgan" target="_blank">on Twitter</a>.

{% include book-plug.html %}