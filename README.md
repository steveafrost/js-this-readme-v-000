# JavaScript This

## Objectives
+ Explain what `this` is and what it can refer to
+ Use `this` to refer to an object that called a function
+ Use `this` to refactor jQuery event handlers
+ Explain how `this` refers to different objects based on scope

## Intro

We're very familiar with `self` in Ruby, and how to use self to refer to the current object when we're manipulating that object.

For example, given a class Dog, with `name` and `owner` attributes:

```ruby
class Dog

  attr_accessor :name, :owner

  def initialize(name)
    @name = name
  end

  def bark
    "Woof!"
  end

  def get_adopted(owner_name)
    self.owner = owner_name
  end

end
```

In the `get_adopted` method, `self` is referencing the dog that is getting adopted.

In JavaScript, a similar concept applies, but instead of using `self`, we use the keyword `this`. The biggest difference between `this` and `self` is that `self` always refers to an object, but in JavaScript, `this` can refer to whatever called the function: an event, action, or object that called the function.
Every new function gets its own `this` value.

There are four main ways to define the value of `this`, which is determined by the execution context of the function call.

four rules and order of precedence:
+ default binding - in strict mode - default to undefined, default to global object

## Global Scope

If we're referencing `this` in the global scope (outside of a function), it can refer to `window` or `document`.

```js
this === window;
// returns true

this === document;
// returns true

function myFunk(){
  console.log(this + " is funky");
}
myFunk();
// prints out "[object Window] is funky" because the function is called on the window object.
```
### Strict Mode

If your function is in strict mode (a mode which allows for better error checking by prohibiting the use of implicitly declared variables, duplicate parameter names, and other potentially bug-causing behavior), then the value of `this` becomes `undefined` instead of `window`:

```js
function sayHi(){
  'use strict';
  alert("Hey!! " + this);
}
//alerts "Hey!! undefined"
```

## Function Scope

Let's take an example of a function using the `load` event handler:
```html
<img class="pix" src="dog.jpg">
<img class="pix" src="cat.jpg">
<img class="pix" src="pig.jpg">
```
```js
function frameIt(){
  $('.pix').on('load', function(){
    $('.pix').addClass("tasty");
  });
}
```

There is actually a really great way to refactor the above example using `this`. We're calling the `on` function on the class `pix`. Because of that fact, we have access to `this` inside of the function, and can replace part of`$('.pix').addClass("tasty");` with `this`:

```js
function frameIt(){
  $('.pix').on('load', function(){
    $(this).addClass("tasty");
    alert("added class!");
  });
}
```
This is super cool, because `this` refers to each of the three images individually as they load. We would see the alert `added class!` appear three times if we loaded this in the browser.


It's important to keep track of the scope in which `this` is being used. A change in scope will cause a change in the object `this` is referencing. 

## Resources

+ [MDN This](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)

+ [JavaScript is Sexy](http://javascriptissexy.com/understand-javascripts-this-with-clarity-and-master-it/)

+ [StackOverFlow This Quiz](http://stackoverflow.com/questions/3127429/how-does-the-this-keyword-work)