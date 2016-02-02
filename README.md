# Event based Directives

## Overview

Now that we've talked about what a directive actually is, let's go into what an event directive actually does for us.

## Objectives

- Describe event based Directives
- Use native events to bind events to a set of list items
- Manually bind/unbind events on list items
- The reason for Angular events
- Use event based Directives to handle events
- Create an event function in the Controller
- Assign the event function to a Directive
- Update the View after event function has been invoked

## Event based directives

If you've ever done a lot of JavaScript before, you would have come into contact with event listeners. These are functions that get called whenever a specific behaviour happens that we are looking out for. Either of the following might be familiar to you:

```js
input.addEventListener('keydown', function (e) {
});

// or

input.onkeydown = function (e) {
};
```
```html
<!-- or this -->

<input onkeydown="myFunction()" />
```

These would all fire off of a function when a key was pressed inside of our input.

## Angular's equivalent

Instead, in Angular, we use the built-in `ng-keydown` directive, and pass in the function we want to be called.

```html
<input ng-keydown="vm.myFunction()" />
```

## Why do we do this?

We already have a way to do events in JavaScript without any plugins. Why do we have to do things differently inside Angular?

Angular allows us to pass all the event handling over to them. We don't have to worry about browser compatibility or even unbinding our events at the sacrifice of using the built-in directives for event handling instead. A small price to pay for consistent event handling in all browsers. 

Angular also has the concept of a "digest cycle". This, at a high level, is a function that keeps all of our view and model in sync with each other (we will be going into this in a lot more detail later on). The built-in directives ensure that the digest cycle is ran, keeping our view and model synchronised if we were to update our model in response to an event.

## Calling our own functions

As you can see in our example above, we are calling `vm.myFunction()`. An example setup of the controller might look like this:

```js
function ExampleController() {
  this.myFunction = function () {
    console.log('Hey!');
  };
}
```

Which would result in `Hey!` being printed to the console whenever the user types in the input.

As the function is defined in the controller, we also have complete access to all services/model items, allowing us to manipulate them. For example, we might want a button that increments a counter every time it's clicked.

First of all, we'd use the `ng-click` directive provided to us by Angular.

Our HTML would look like this:

```html
<button ng-click="vm.incrementCounter()">Increment the counter!</button>

{{ vm.counter }}
```

And our controller would look like this:

```js
function CounterController() {
  this.counter = 0;
  
  this.incrementCounter = function () {
    this.counter++;
  };
}
```

What's happening here? Well, whenever the user clicks on our button, our `incrementCounter` function gets called. We can then access our model value (`this.counter`) inside that function, and update it itself plus one. Angular then notices that the model has changed, and will update our view to reflect this.