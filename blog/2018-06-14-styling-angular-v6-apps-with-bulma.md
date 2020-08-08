---
title: 'Styling Angular v6 Apps with Bulma'
date: '2018-06-14'
layout: post
tag: tutorial
excerpt: >-
  In this tutorial we'll learn how to generate a new Angular project using the Angular CLI and style it using the Bulma framework.
image: '/images/angular-and-bulma/tutorial-angular-and-bulma.png'
thumb_image: '/images/angular-and-bulma/tutorial-angular-and-bulma.png'
style: code
---

## Introduction

In this tutorial we'll learn how to generate a new Angular project using the [Angular CLI](https://github.com/angular/angular-cli){:target="_blank"} and style it using the [Bulma](https://bulma.io/){:target="_blank"} framework.

* Angular CLI 6.0.0
* Bulma 0.7.1
* Font Awesome 5.0.13

{% include request-tutorial.html %}

## Angular Setup

The first thing you'll need to do is install the latest version of the Angular CLI.

```console
npm install -g @angular/cli@latest
```

Once this has finished installing you can verify you are on the right version by running:

```console
ng --version
```

Run that and you should see an output similar to this:

```console
Angular CLI: 6.0.8
Node: 8.9.0
OS: darwin x64
Angular:
...

Package                      Version
------------------------------------------------------
@angular-devkit/architect    0.6.8
@angular-devkit/core         0.6.8
@angular-devkit/schematics   0.6.8
@schematics/angular          0.6.8
@schematics/update           0.6.8
rxjs                         6.2.1
typescript                   2.7.2
```

Now that the latest version of the CLI is installed you can scaffold a new application using `ng new`.

```console
ng new angular-and-bulma
```

Once that's finished, you can move into the directory and run the application.

```console
cd angular-and-bulma
ng serve -o
```

![New Angular app](/images/angular-and-bulma/new-app.png)

## Bulma Setup

Now that our Angular project has been created, we can move on to [Bulma](https://bulma.io/){:target="_blank"}. First, we'll need to install it.

```console
npm install bulma --save
```

Once that has finished, we'll need to install [Font Awesome](https://www.npmjs.com/package/@fortawesome/angular-fontawesome){:target="_blank"}.

```
npm install @fortawesome/fontawesome-svg-core --save
npm install @fortawesome/free-solid-svg-icons --save
npm install @fortawesome/angular-fontawesome --save
```

Now that Bulma and Font Awesome are installed, we can update the `styles` property in `angular.json` to include the new stylesheet for Bulma.

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

Then within `app.module.ts` we can add the following imports for Font Awesome.

```typescript
import { AppComponent } from './app.component';
// Add these
import { FontAwesomeModule } from '@fortawesome/angular-fontawesome';
import { library } from '@fortawesome/fontawesome-svg-core';
import { fas } from '@fortawesome/free-solid-svg-icons';
library.add(fas);

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    FontAwesomeModule // Add this
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

<div class="box" markdown="1">
In the example above we imported *all* of the icon styles which may not be necessary depending on your needs. If you only need a certain subset you can specify those as [stated in the documentation](https://www.npmjs.com/package/@fortawesome/angular-fontawesome#using-the-icon-library){:target="_blank"}.
</div>

Finally, we can see Bulma's styling in action by updating `app.component.html` with the following markup.

```html
<section class="section">
  <div class="container">
    <div class="columns is-centered">
      <div class="column is-half">
        <h3 class="title is-3">Bulma Form</h3>
        <div class="field">
          <label class="label">Name</label>
          <div class="control">
            <input class="input" type="text" placeholder="Text input">
          </div>
        </div>

        <div class="field">
          <label class="label">Username</label>
          <div class="control has-icons-left has-icons-right">
            <input class="input is-success" type="text" placeholder="Text input" value="bulma">
            <span class="icon is-small is-left">
              <fa-icon icon="coffee"></fa-icon>
            </span>
            <span class="icon is-small is-right">
              <fa-icon icon="check"></fa-icon>
            </span>
          </div>
          <p class="help is-success">This username is available</p>
        </div>

        <div class="field">
          <label class="label">Email</label>
          <div class="control has-icons-left has-icons-right">
            <input class="input is-danger" type="email" placeholder="Email input" value="hello@">
            <span class="icon is-small is-left">
              <fa-icon icon="envelope"></fa-icon>
            </span>
            <span class="icon is-small is-right">
              <fa-icon icon="exclamation-triangle"></fa-icon>
            </span>
          </div>
          <p class="help is-danger">This email is invalid</p>
        </div>

        <div class="field">
          <label class="label">Subject</label>
          <div class="control">
            <div class="select">
              <select>
                <option>Select dropdown</option>
                <option>With options</option>
              </select>
            </div>
          </div>
        </div>

        <div class="field">
          <label class="label">Message</label>
          <div class="control">
            <textarea class="textarea" placeholder="Textarea"></textarea>
          </div>
        </div>

        <div class="field">
          <div class="control">
            <label class="checkbox">
              <input type="checkbox">
              I agree to the <a href="#">terms and conditions</a>
            </label>
          </div>
        </div>

        <div class="field">
          <div class="control">
            <label class="radio">
              <input type="radio" name="question">
              Yes
            </label>
            <label class="radio">
              <input type="radio" name="question">
              No
            </label>
          </div>
        </div>

        <div class="field is-grouped">
          <div class="control">
            <button class="button is-link">Submit</button>
          </div>
          <div class="control">
            <button class="button is-text">Cancel</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>
```

Run `ng serve` again and you should now see your Angular application styled with Bulma.

![Styled Angular app with Bulma](/images/angular-and-bulma/bulma-app.png)

{% include book-plug.html %}