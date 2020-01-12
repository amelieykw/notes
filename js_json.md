# What is JSON?

> "JavaScript Object Notation"
> \- A standard for **structuring data** that is inspired by JavaScript object literals.

JavaScript is built to understand it.

JSON is just a way of writting text, of writting data so that other systems that understand JSON can receive your data, and if your system understands data, it can send you data.

```JSON
{
    "firstname": "John",
    "lastname": "Doe",
    "address": {
        "street": "101 Main St.",
        "city": "New York",
        "state": "NY"
    }
}
```

JSON object is just like an object litteral (a collection of name/value pairs), however, we don't put functions here, because this is just pure data being sent across the wire. And we also need to put names in quotes.

JavaScript and the V8 engine can take this as text and convert this to a
JavaScript objectjust as if we had used ``object literal notation``, or take objects to convert them to JSON.

