---
title: 'How to Test Angular CanActivate Guards'
date: '2018-08-28'
layout: post
tag: tutorial
excerpt: >-
  In this tutorial you'll learn how to test a CanActivate route guard in Angular v6.
image: '/images/angular-canactivate/tutorial-cover.png'
thumb_image: '/images/angular-canactivate/tutorial-cover.png'
style: code
---

## Introduction

In this tutorial you'll learn how to test a [CanActivate](https://angular.io/api/router/CanActivate){:target="_blank"} route guard within Angular. We'll create two separate routed components and apply this guard to one of the routes that will restrict it to authorized users.

## Setup

First, we'll create a new application using the [Angular CLI](https://cli.angular.io/){:target="_blank"} adding the `--routing` flag to generate a routing module for us.

```console
ng new guard-testing --routing
```

From here, we'll clean up a few things within this scaffolded application that we don't need. First, we'll remove all of the boilerplate in `app.component.html` leaving just the `router-outlet` directive.

```html
<router-outlet></router-outlet>
```

Then we'll update `app.component.spec.ts` by removing the bottom two tests that are no longer needed as a result of our template cleanup.

```typescript
describe('AppComponent', () => {
  beforeEach(async(() => {
    TestBed.configureTestingModule({
      imports: [
        RouterTestingModule
      ],
      declarations: [
        AppComponent
      ],
    }).compileComponents();
  }));
  it('should create the app', async(() => {
    const fixture = TestBed.createComponent(AppComponent);
    const app = fixture.debugElement.componentInstance;
    expect(app).toBeTruthy();
  }));
});
```

## Create components

From here we'll create two components to serve as our two views for a "restricted" and "unrestricted" portion of our application.

```console
ng g component restricted
ng g component unrestricted
```

Then we can update `app-routing.module.ts` to create the new routes for these components.

```typescript
import { RestrictedComponent } from './restricted/restricted.component';
import { UnrestrictedComponent } from './unrestricted/unrestricted.component';

const routes: Routes = [
  { path: '', component: UnrestrictedComponent },
  { path: 'restricted', component: RestrictedComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

First we import the two components. Then we update `routes` setting `UnrestrictedComponent` as the root path of the application and `RestrictedComponent` to the `/restricted` path.

Run your application with `ng serve` and you should now see the default templates for these two components when you access their routes.

{% include request-tutorial.html %}

## Auth service

Before we get to the guard, we'll create a simple auth service that will be used to simulate authentication within our app.

First, we create the service.

```console
ng g service services/auth/auth
```

Then we'll add an `isLoggedIn` method to the service with a hard-coded return value. For now, we'll set it to `true`.

```typescript
isLoggedIn() {
  return true;
}
```

## Create guard

Now that our dummy `AuthService` is setup, we can move on to the route guard.

Once again, we'll use the CLI to generate this guard.

```console
ng g guard guards/auth/auth
```

Within `auth.guard.ts`, update the imports to include `Router` and `AuthService`.

```typescript
import { CanActivate, Router } from '@angular/router';
import { AuthService } from '../../services/auth/auth.service';
```

Then add a constructor to `AuthGuard` injecting `AuthService` and `Router` into the class.

```typescript
constructor(private authService: AuthService, private router: Router) { }
```

Since we don't need the default parameters of `canActivate`, you can remove those as well.

```typescript
canActivate(): Observable<boolean> | Promise<boolean> | boolean {
  return true;
}
```

Now that `AuthGuard` is setup, we can implement the `canActivate` method.

```typescript
canActivate(): Observable<boolean> | Promise<boolean> | boolean {
  if (this.authService.isLoggedIn()) {
    return true;
  } else {
    this.router.navigate(['/']);
    return false;
  }
}
```

If `authService.isLoggedIn` is `true`, we return `true`. Otherwise, we redirect users to the root path of our application.

Now we can move on to the tests in `auth.guard.spec.ts`. For a guard such as this with dependencies that aren't all that complex, I find it easier to test without `TestBed`, creating instances of any classes we need manually.

```typescript
import { AuthGuard } from './auth.guard';

class MockRouter {
  navigate(path) {}
}

describe('AuthGuard', () => {
  describe('canActivate', () => {
    let authGuard: AuthGuard;
    let authService;
    let router;
  });
});
``` 

We first create a mock for `Router` with a navigate method. Then we create a new `describe` for the `canActivate` method.

From here we'll add our first test for when `isLoggedIn` returns true, or when a user accessing a route with this route guard "can activate" the route.

```typescript
describe('AuthGuard', () => {
  describe('canActivate', () => {
    let authGuard: AuthGuard;
    let authService;
    let router;

    it('should return true for a logged in user', () => {
      authService = { isLoggedIn: () => true };
      router = new MockRouter();
      authGuard = new AuthGuard(authService, router);

      expect(authGuard.canActivate()).toEqual(true);
    });
  });
});
```

First we mock `authService` with a stubbed return value of `true`. Then we create an instance of our `MockRouter` and an instance of `AuthGuard` using our two mocks.

We then add our expectation that calling `canActivate` should return `true`.

Now we'll add our second test case for when `canActivate` returns `false`.

```typescript
it('should return true for a logged in user', () => {
  ...
});

it('should navigate to home for a logged out user', () => {
  authService = { isLoggedIn: () => false };
  router = new MockRouter();
  authGuard = new AuthGuard(authService, router);
  spyOn(router, 'navigate');

  expect(authGuard.canActivate()).toEqual(false);
  expect(router.navigate).toHaveBeenCalledWith(['/']);
});
```

Here we update the return value of `isLoggedIn` to `false` and create the instance of our classes just as we did earlier. We also add a spy to the `navigate` method of `router`.

Then we add our expectation that calling `canActivate` should return `false` and that the `navigate` method will be called to redirect users to the root path.

At this point when you run `ng test` you should see passing tests for this guard.

## Add guard to routing

At this point all that's left to do is apply the `AuthGuard` to the `/restricted` route.

```typescript
import { AuthGuard } from './guards/auth/auth.guard';

const routes: Routes = [
  { path: '', component: UnrestrictedComponent },
  { path: 'restricted', component: RestrictedComponent, canActivate: [ AuthGuard ] }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

First we import `AuthGuard`. Then we add the `canActivate` property to the `restricted` path with our new guard.

At this point you can update the hard-coded value for `isLoggedIn` to return `false` in `auth.service.ts`.

```typescript
export class AuthService {

  constructor() { }

  isLoggedIn() {
    return false;
  }
}
```

Now when you try accessing `/restricted` and you should be redirected to the root path of the application, `UnrestrictedComponent`.

> If you found this valuable and would like to see more, feel free to let me know <a href="https://twitter.com/atommorgan" target="_blank">on Twitter</a>.

{% include book-plug.html %}
