# Modules, Exports and Requires

## Modules

> **A reusable block of code whose existence does not accidentally impact other code.**

JavaScript doesn't have this before.

## CommonJS Modules

> **An agreed upon standard for how code modules should be strucutred.**

# First-class functions & Function expressions

## First-class functions

> **Everything you can do with other types you can do with functions.**

You can use funtions like strings, numbers, etc. (i.e. pass them around, set variables equal to them, put them in arrays, and more.)

## An expression

> **A block of code that results in a value.**

Function expressions are possible in JavaScript because functions are first-class.

```JavaScript
// app.js

// function statement
function greet() {
    console.log('hi');
}
greet();
```

invoke this function by typing command ``node app.js``

```JavaScript
// functions are first-class, we can pass them like parameters
function logGreeting(fn) {
    fn(); // here, we invoke this passed-in function
}
logGreeting(greet); // pass a function
```

Function expression is first-class. It can be assigned to a variable, or used as a parameter and passed to an another function.

```JavaScript
// function expression - it's still first-class
var greetMe = function() {   // an object points to a function
    console.log('Hi Tony!');
}
greetMe();

logGreeting(greetMe);

// use a function expression on a fly
logGreeting(function() {
    console.log('Hello Tony!');
})
```

# How do ``modules`` really work? ``Module.exports`` and ``require``

greet.js

```JavaScript
var greet = function() {
    console.log('Hello!');
}

module.exports = greet;
```

app.js

In app.js, we use ``require`` to get that module and it returned the value of ``module.exports``, so we then were able to use that function in our app.js file.

```JavaScript
var greet = require('./greet.js');
greet();
```

> **According to the source sode of JavaScript, ``require`` will load the content of file.**

> ``require`` will assume that if you don't put an extension you mean ``.js``.
>
> If you don't mean ``.js`` file, you should put an extension.

