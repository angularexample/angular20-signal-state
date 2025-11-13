# Angular Signals State Management

Example shows how to use Angular Signals to manage state in Angular 20, following the Redux pattern.

## Table of Contents

* [If You Want to Use the Code](#if-you-want-to-use-the-code)
  * [Click the Star Button and Follow the Author](#click-the-star-button-and-follow-the-author)
* [See the Article on Medium](#see-the-article-on-medium)
* [Date Published](#date-published)
* [Versions Used](#versions-used)
* [About The Author](#about-the-author)
* [Demo Overview](#demo-overview)
* [How to Run](#how-to-run)
* [The Redux Pattern](#the-redux-pattern)
* [The Store](#the-store)
* [The State Object](#the-state-object)
  * [The State is a Writable Signal](#the-state-is-a-writable-signal) 
* [The Actions](#the-actions)
* [The Reducers](#the-reducers)
* [The Effects](#the-effects)
* [The Selectors](#the-selectors)
* [The Facade Pattern](#the-facade-pattern)
* [A Shorter Version](#a-shorter-version)

## If You Want to Use the Code

You can clone the repo and use the code as you see fit. Or you can follow the instructions in this article to create your own project.

In return, Please:

### Click the Star Button and Follow the Author

* Go to the Repository https://github.com/angularexample/angular20-signal-state and click the **Star** button at the top right.
* Go to my GitHub page https://github.com/angularexample and click the **Follow** button on the left side.

This will promote the repo and help others to find this solution.

## See the Article on Medium

You can read the article on Medium for this project:
[Angular Signals State Management](https://medium.com/@info_35389/angular20-signal-state-5704d2d408c6)


## Date Published

October 6, 2025

## Versions Used

At the time of this writing, we used the latest version of Angular

* Angular 20.3.0

## About The Author

`JC Lango` is a UI Architect and Developer for many large-scale web applications at several well-known Fortune 500 companies.

He is an expert in **Angular** and **React** and maintains these sites at GitHub:

* **AngularExample** [https://github.com/angularexample](https://github.com/angularexample)
* **ReactJSExample** [https://github.com/reactjsexample](https://github.com/reactjsexample)

JC may be available to work remotely and can be contacted at these links:

* LinkedIn: [https://linkedin.com/in/jclango](https://linkedin.com/in/jclango)
* Email: [jobs@jclango.com](mailto:jobs@jclango.com)

## Demo Overview

The demo has four pages:

* Home - List of features in this code example
* Users - List of users
* Posts - List of posts for selected user
* Post Edit - Update a selected post

The demo uses a simple REST API to fetch data.

## How to Run

To start a local development server, run:

```bash
ng serve
```

## The Redux Pattern

The Redux pattern is a popular way to manage state in Angular applications.

Normally this is done with NgRx, but in this example we use Angular Signals to manage state.

Using Angular Signals means that no third party packages are needed. Everything is done using Angular.

This design does follow the same pattern as Redux, or NgRx. If you are already familiar with NgRx, you will see that code is segmented into the same basic sections.

## The Store

The store is a class object that is used access the state data.

It has all the properties and methods to get and set the state.

It also contains all the logic that controls the behavior of the view.

The store is made from 5 parts:

* The State Object
* The Actions
* The Reducers
* The Effects
* The Selectors

## The State Object

Just like in Redux, all the properties to hold the state are contained in a single object, which we refer to as the state.

The state object is defined as an Interface. It would be done exactly the same as in NgRx.

Here is the state object for the Users page:

```typescript
export interface XxxUserState {
  isUsersLoading: boolean;
  selectedUserId: number | undefined;
  users: XxxUserType[];
}
```

### The State is a Writable Signal

In this design, the state is a writable signal.

```typescript
  private userState: WritableSignal<XxxUserState> = signal<XxxUserState>(xxxUserInitialState);
```

## The Actions

The actions are the functions that are used to change the state.

For example, the action to select a user is:

```typescript
  setSelectedUserIdAction(userId: number): void {
    this.setSelectUserIdReducer(userId);
    this.setSelectUserIdEffect();
}
```

The action is called by the view.

Following the Redux pattern, the action calls first the reducer, then the effect.

## The Reducers

The reducers are the functions that change the state.

The reducers are the only functions that can change the state.

Reducers cannot be called directly from the view. They are called by the actions.

Here is the reducer for the `setSelectedUserIdAction`:

```typescript
  private setSelectUserIdReducer(userId: number): void {
  this.userState.update(state =>
    ({
      ...state,
      selectedUserId: userId
    })
  )
}
```

## The Effects

The effects are the functions that are called by the actions.

This is where we run any services.

Some examples of services used in effects are:

* Fetching data from an API
* Sending data to an API
* Navigating to a new page
* Showing a dialog

Here is the effect for the `setSelectedUserIdAction`:

```typescript
  private setSelectUserIdEffect(): void {
    void this.router.navigateByUrl('/post')
}
```

## The Selectors

The selectors are the functions that are used to read properties or data from the state.

They are the only functions that can read the state.

In this design, all selectors are readonly Signals.

Here is the selector to read the selected user id:

```typescript
  readonly selectSelectedUserId: Signal<number | undefined> = computed(() => this.userState().selectedUserId);
```

As you can see, the selector is created by using the Angular Signals `computed` function.

Inside the `computed` function, the state is read using the `userState()` function, which returns the state object.

## The Facade Pattern

This example app uses the Facade pattern.

The Facade pattern is used to hide the complexity of the state management. It decouples the view from the state management.

Advantages of the Facade pattern:

* Allows the view to be coded and tested independently of the state management.
* Allows the state management to be changed without affecting the view.
* Allows the view to be changed without affecting the state management.

For example, at a later date, we could change the state management to use NgRx or SignalStore.

During development, we can code and test the view without having to worry about the state management.

In fact you can code all your view components without knowing anything about the state management.

By using mocks in the facade, the state management does not need to exist. So you completely code the view first, and then code the state management later.

A huge advantage of this design is that it allows you to easily make changes to the view, without making any changes to the rest of the app.

And you can make changes to your data access or logic, without making any changes to the view.

## A Shorter Version

You may have noticed that, in the store, we could eliminate many of the functions, by collapsing them into a single function.

However, the shortened version has some disadvantages:

* It is not so clear to see the Redux pattern.
* The functions are much larger.

At the same time, we could also eliminate the redux pattern, and instead use the store directly in the view.

I have done exactly those things in the [Angular Signals State Management (Shorter Version)](https://github.com/angularexample/angular20-signal-state-shorter-version) example.

