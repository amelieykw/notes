# Immediately Invoked Function Expressions (IIFE)

> a fundamental aspect of how modules actually work

When we talk about ``modules``, we are not only talking about ``reusable of codes``, but also ``code that's protected`` (code written in a module doesn't impact any other code written in other places in this program).

## Scope

> When in code, you have access to particular variable or function.

```JavaScript
var firstname = 'Jane';

(function () {
    var firstname = 'John';
    console.log(firstname);
}());

console.log(firstname);

$ node app.js
John
Jane
```

```JavaScript
function () {

}
```

This creates a function.

```JavaScript
(

());
```

This invokes immediately this function.

Variable inside the immediately invoked function doesn't impact any other code because of JavaScript scope.

### Functional Scope

> Whatever variables, whatever functions you create, within a function, are protected by that function. It's only accessible with that function. It's scoped to that function.

#### How we fake a module by using immediately invoked function expression?

> We can wrap our code inside an immediately invoked function expression to make sure that it doesn't unintentially affect anything outside of it.

pass parameters to immediately invoked function expression

```JavaScript
(function(lastname) {
    var firstname = 'John';
    console.log(firstname);
    console.log(lastname);
}('Doe'));
```

Whatever inside an immediately invoked function expression is protected within the function and cannot be accessed from outside the function. This is the same intent that I might use a module.
