# [The Basics of Web Workers](https://www.html5rocks.com/en/tutorials/workers/basics/)

- [The Basics of Web Workers](#the-basics-of-web-workers)
  - [1. The Problem: JavaScript Concurrency](#1-the-problem-javascript-concurrency)
    - [Context - Why do we need to think about &quot;concurrency&quot; in JS ?](#context---why-do-we-need-to-think-about-quotconcurrencyquot-in-js)
    - [Concurrency VS Asynchronous](#concurrency-vs-asynchronous)
  - [2. Introducing Web Workers: Bring Threading to JavaScript](#2-introducing-web-workers-bring-threading-to-javascript)
    - [two kinds of Web Workers](#two-kinds-of-web-workers)
  - [3. Getting Started](#3-getting-started)
    - [Communicating with a Worker via Message Passing](#communicating-with-a-worker-via-message-passing)
  - [4. Transferrable objects](#4-transferrable-objects)
  - [5. The Worker Environment](#5-the-worker-environment)
    - [Worker Scope](#worker-scope)
    - [Features Available to Workers](#features-available-to-workers)
    - [Loading External Scripts](#loading-external-scripts)
    - [Subworkers](#subworkers)
  - [6. Inline worker](#6-inline-worker)
    - [full example](#full-example)
  - [7. Handling Errors](#7-handling-errors)
  - [8. A Word on Security](#8-a-word-on-security)
  - [9. Use Cases](#9-use-cases)
  - [10. Examples](#10-examples)
  - [11. Tutorial with a full example](#11-tutorial-with-a-full-example)

## 1. The Problem: JavaScript Concurrency

### Context - Why do we need to think about "**``concurrency``**" in JS ?

There are a number of bottlenecks preventing interesting applications from being ported (say, from server-heavy implementations) to client-side JavaScript (browser compatibility, static typing, accessibility, and performance).

One thing that's remained a hindrance for JavaScript is actually the language itself.

> JavaScript is a **single-threaded** environment, meaning ***multiple scrits cannot run at the same time***.

As an example, imagine a site that needs to handle UI events, query and process large amounts of API data, and manipulate the DOM. Script execution happens within a single thread.

### **``Concurrency``** VS **``Asynchronous``**

Developers mimic '**``concurrency``**' by using techniques like ``setTimeout()``, ``setInterval()``, ``XMLHttpRequest``, and ``event handlers``. Yes, all of these features run **asynchronously**, but **non-blocking doesn't necessarily mean concurrency.**

> **Asynchronous events are processed after the current executing script has yielded.**
>
> The good news is that HTML5 gives us something better than these hacks!

=> [The Basics of Web Workers](#the-basics-of-web-workers)

## 2. Introducing Web Workers: Bring Threading to JavaScript

Web Workers allow you to do things like **fire up long-running scripts to handle computationally intensive tasks, but without blocking the UI or other scripts to handle user interactions**.

Workers utilize thread-like message passing to achieve parallelism.

### two kinds of Web Workers

1. **Dedicated** Workers
2. **Shared** Workers

(This article will only cover dedicated workers and I'll refer to them as 'web workers' or 'workers' throughout.)

=> [The Basics of Web Workers](#the-basics-of-web-workers)

## 3. Getting Started

> Web Workers **run in an isolated thread.**
>
> As a result, the code that they execute needs to be contained **in a separate file**.

STEPs:

1. the first thing to do is **create a new Worker object in your main page**. The constructor takes the name of the worker script:

```JavaScript
var worker = new Worker('task.js');
```

- If the specified file exists, the browser will spawn a new worker thread, which is downloaded asynchronously.
  - The worker will not begin until the file has completely downloaded and executed.
- If the path to your worker returns an 404, the worker will fail silently.

2. After creating the worker, start it by calling the **``postMessage()``** method:

```JavaScript
worker.postMessage(); // Start the worker.
```

### Communicating with a Worker via Message Passing

---

Below is a example of using a string to pass 'Hello World' to a worker in doWork.js. The worker simply returns the message that is passed to it.

Main script:

```HTML
<button onclick="sayHI()">Say HI</button>
<button onclick="unknownCmd()">Send unknown command</button>
<button onclick="stop()">Stop worker</button>
<output id="result"></output>

<script>
  function sayHI() {
    worker.postMessage({'cmd': 'start', 'msg': 'Hi'});
  }

  function stop() {
    // worker.terminate() from this script would also stop the worker.
    worker.postMessage({'cmd': 'stop', 'msg': 'Bye'});
  }

  function unknownCmd() {
    worker.postMessage({'cmd': 'foobard', 'msg': '???'});
  }

  var worker = new Worker('doWork2.js');

  worker.addEventListener('message', function(e) {
    document.getElementById('result').textContent = e.data;
  }, false);
</script>
```

doWork.js (the worker):

```JavaScript
self.addEventListener('message', function(e) {
  var data = e.data;
  switch (data.cmd) {
    case 'start':
      self.postMessage('WORKER STARTED: ' + data.msg);
      break;
    case 'stop':
      self.postMessage('WORKER STOPPED: ' + data.msg + '. (buttons will no longer work)');
      self.close(); // Terminates the worker.
      break;
    default:
      self.postMessage('Unknown command: ' + data.msg);
  };
}, false);
```

> **Note:** There are two ways to stop a worker: by calling ``worker.terminate()`` from the main page or by calling ``self.close()`` inside of the worker itself.

The object is being passed directly to the worker even though it's running in a separate, dedicated space. In actuality, what is happening is that the object is being serialized as it's handed to the worker, and subsequently, de-serialized on the other end. 

The page and worker do not share the same instance, so the end result is that a duplicate is created on each pass. Most browsers implement this feature by automatically JSON encoding/decoding the value on either end.

=> [The Basics of Web Workers](#the-basics-of-web-workers)

## 4. Transferrable objects

Most browsers implement the structured cloning algorithm, which allows you to pass more complex types in/out of Workers such as **File, Blob, ArrayBuffer**, and **JSON objects**.

However, when passing these types of data using ``postMessage()``, a copy is still made. Therefore, if you're passing a large 50MB file (for example), there's a noticeable overhead in getting that file between the worker and the main thread.

To use transferrable objects, use a slightly different signature of ``postMessage()``:

```JavaScript
worker.postMessage(arrayBuffer, [arrayBuffer]);
window.postMessage(arrayBuffer, targetOrigin, [arrayBuffer]);
```

=> [The Basics of Web Workers](#the-basics-of-web-workers)

## 5. The Worker Environment

### Worker Scope

In the context of a worker, both self and this reference the global scope for the worker. Thus, the previous example could also be written as:

```JavaScript
addEventListener('message', function(e) {
  var data = e.data;
  switch (data.cmd) {
    case 'start':
      postMessage('WORKER STARTED: ' + data.msg);
      break;
    case 'stop':
  ...
}, false);
```

Alternatively, you could set the ``onmessage`` event handler directly (though ``addEventListener`` is always encouraged by JavaScript ninjas).

```JavaScript
onmessage = function(e) {
  var data = e.data;
  ...
};
```

### Features Available to Workers

Due to their **multi-threaded** behavior, web workers **only** has access to a subset of JavaScript's features:

- The ``navigator`` object
- The ``location`` object (read-only)
- ``XMLHttpRequest``
- ``setTimeout()``/``clearTimeout()`` and ``setInterval()``/``clearInterval()``
- The [Application Cache](https://www.html5rocks.com/tutorials/appcache/beginner/)
- Importing external scripts using the ``importScripts()`` method
- [Spawning other web workers](https://www.html5rocks.com/en/tutorials/workers/basics/#toc-enviornment-subworkers)

Workers do **NOT** have access to:

- The ``DOM`` (it's not thread-safe)
- The ``window`` object
- The ``document`` object
- The ``parent`` object

### Loading External Scripts

You can load external script files or libraries into a worker with the ``importScripts()`` function. The method takes zero or more strings representing the filenames for the resources to import.

worker.js:

```JavaScript
importScripts('script1.js');
importScripts('script2.js');
```

or

```JavaScript
importScripts('script1.js', 'script2.js');
```

### Subworkers

> Workers have the ability to spawn child workers. This is great for further breaking up large tasks at runtime.
>
> - Subworkers must be hosted within the same origin as the parent page.
> - URIs within subworkers are resolved relative to their parent worker's location (as opposed to the main page).



=> [The Basics of Web Workers](#the-basics-of-web-workers)

## 6. Inline worker
 
### full example

Taking this one step further, we can get clever with how the worker's JS code is inlined in our page. This technique uses a ``<script> ``tag to define the worker:

```HTML
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
</head>
<body>

  <div id="log"></div>

  <script id="worker1" type="javascript/worker">
    // This script won't be parsed by JS engines
    // because its type is javascript/worker.
    self.onmessage = function(e) {
      self.postMessage('msg from worker');
    };
    // Rest of your worker code goes here.
  </script>

  <script>
    function log(msg) {
      // Use a fragment: browser will only render/reflow once.
      var fragment = document.createDocumentFragment();
      fragment.appendChild(document.createTextNode(msg));
      fragment.appendChild(document.createElement('br'));

      document.querySelector("#log").appendChild(fragment);
    }

    var blob = new Blob([document.querySelector('#worker1').textContent]);

    var worker = new Worker(window.URL.createObjectURL(blob));
    worker.onmessage = function(e) {
      log("Received: " + e.data);
    }
    worker.postMessage(); // Start the worker.
  </script>
</body>
</html>
```

In my opinion, this new approach is a bit cleaner and more legible. It defines a script tag with ``id="worker1"`` and ``type='javascript/worker' ``(so the browser doesn't parse the JS). That code is extracted as a string using ``document.querySelector('#worker1')``.textContent and passed to ``Blob()`` to create the file.

=> [The Basics of Web Workers](#the-basics-of-web-workers)

## 7. Handling Errors

```HTML
<output id="error" style="color: red;"></output>
<output id="result"></output>

<script>
  function onError(e) {
    document.getElementById('error').textContent = [
      'ERROR: Line ', e.lineno, ' in ', e.filename, ': ', e.message
    ].join('');
  }

  function onMsg(e) {
    document.getElementById('result').textContent = e.data;
  }

  var worker = new Worker('workerWithError.js');
  worker.addEventListener('message', onMsg, false);
  worker.addEventListener('error', onError, false);
  worker.postMessage(); // Start worker without a message.
</script>
```

=> [The Basics of Web Workers](#the-basics-of-web-workers)

## 8. A Word on Security

=> [The Basics of Web Workers](#the-basics-of-web-workers)

## 9. Use Cases

=> [The Basics of Web Workers](#the-basics-of-web-workers)

## 10. Examples

[Examples](https://html.spec.whatwg.org/multipage/workers.html#examples-6)

=> [The Basics of Web Workers](#the-basics-of-web-workers)

## 11. [Tutorial with a full example](https://www.sitepoint.com/javascript-web-workers/)

=> [The Basics of Web Workers](#the-basics-of-web-workers)