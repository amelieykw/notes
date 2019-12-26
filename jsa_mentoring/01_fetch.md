# Summary

> [JS Network Request [Tutorial]](https://javascript.info/network)

1. [[Fetch](https://javascript.info/fetch)](#1-fetch)
2. FormData
3. Fetch: Download progress
4. Fetch: Abort
5. Fetch: Cross-Origin Requests
6. Fetch API
7. URL objects
8. XMLHttpRequest
9. Resumable file upload
10. Long polling
11. WebSocket
12. Server Sent Events

## 1. Fetch

> - 1.1 [**Response headers**](#11-response-headers)
> - 1.2 [**Request headers**](#12-request-headers)
> - 1.3 [**POST requests**](#13-post-requests)
> - 1.4 [**Sending an image**](#14-sending-an-image)
> - 1.5 [**Summary**](#15-summary)

JavaScript can send network requests to the server and load new information whenever is needed.

The basic syntax is:

```JavaScript
let promise = fetch(url, [options])
```

- **url** – the URL to access.
- **options** – optional parameters: *method*, *headers* etc.
  - (Without **options**, that is a simple GET request, downloading the contents of the url.)

The browser starts the request right away and returns a promise that the calling code should use to get the result.

Getting a response is usually a two-stage process:

1. The **promise**, returned by **fetch**, **resolves** with an object of the built-in **Response** class as soon as the server responds with headers.
   1. At this stage we can :
      1. check HTTP status, to see whether it is successful or not, check headers, but don’t have the body yet.
   2. The promise **rejects** :
      1. if the fetch was unable to make HTTP-request, e.g. network problems, or there’s no such site. 
      2. Abnormal HTTP-statuses, such as 404 or 500 do not cause an error.
   3. We can see HTTP-status in response properties:
      1. **status** – HTTP status code, e.g. 200.
      2. **ok** – boolean, true if the HTTP status code is 200-299.

      ```JavaScript
      let response = await fetch(url);

      if (response.ok) { // if HTTP-status is 200-299
        // get the response body (the method explained below)
        let json = await response.json();
      } else {
        alert("HTTP-Error: " + response.status);
      }
      ```

2. To get the response body, **Response** provides multiple **promise-based** methods to access the body in various formats:
   1. ``response.text()`` – read the response and return as **text**,
   2. ``response.json()`` – parse the response as **JSON**,
   3. ``response.formData()`` – return the response as **FormData** object (explained in the next chapter),
   4. ``response.blob()`` – return the response as **Blob** (binary data with type),
   5. ``response.arrayBuffer()`` – return the response as **ArrayBuffer** (low-level representaion of binary data),
   6. additionally, ``response.body`` is a **ReadableStream** object, it allows to read the body chunk-by-chunk.

      ```JavaScript
      // get a JSON-object with latest commits from GitHub

      let url = 'https://api.github.com/repos/javascript-tutorial/en.javascript.info/commits';
      let response = await fetch(url);

      let commits = await response.json(); // read response body and parse as JSON

      alert(commits[0].author.login);
      ```

      ```JavaScript
      // get a JSON-object with latest commits from GitHub (without await)
      fetch('https://api.github.com/repos/javascript-tutorial/en.javascript.info/commits')
      .then(response => response.json())
      .then(commits => alert(commits[0].author.login));
      ```

      ```JavaScript
      // get the response text

      let response = await fetch('https://api.github.com/repos/javascript-tutorial/en.javascript.info/commits');

      let text = await response.text(); // read response body as text

      alert(text.slice(0, 80) + '...');

      ```  

      > We can choose only one body-reading method.
      >
      > If we’ve already got the response with response.text(), then response.json() won’t work, as the body content has already been processed.
      >
      > ```JavaScript
      > let text = await response.text(); // response body consumed
      > let parsed = await response.json(); // fails (already consumed)
      > ```
      >

### 1.1 Response headers

The response headers are available in a **Map-like** headers object in ``response.headers``.

```JavaScript
let response = await fetch('https://api.github.com/repos/javascript-tutorial/en.javascript.info/commits');

// get one header
alert(response.headers.get('Content-Type')); // application/json; charset=utf-8

// iterate over all headers
for (let [key, value] of response.headers) {
  alert(`${key} = ${value}`);
}
```

### 1.2 Request headers

To set a request header in ``fetch``, we can use the ``headers`` option.

```JavaScript
let response = fetch(protectedUrl, {
  headers: {
    Authentication: 'secret'
  }
});
```

…But there’s a list of **forbidden HTTP headers** that we can’t set:

- ``Accept-Charset``, ``Accept-Encoding``
- ``Access-Control-Request-Headers``
- ``Access-Control-Request-Method``
- ``Connection``
- ``Content-Length``
- ``Cookie``, ``Cookie2``
- ``Date``
- ``DNT``
- ``Expect``
- ``Host``
- ``Keep-Alive``
- ``Origin``
- ``Referer``
- ``TE``
- ``Trailer``
- ``Transfer-Encoding``
- ``Upgrade``
- ``Via``
- ``Proxy-*``
- ``Sec-*``
These headers ensure **proper and safe HTTP,** so they are controlled exclusively by the browser.

### 1.3 POST requests

- **method** – HTTP-method, e.g. POST,
- **body** – the request body, one of:
  - a string (e.g. **JSON-encoded**),
  - ``FormData`` object, to submit the data as ``form/multipart``,
  - ``Blob/BufferSource`` to send binary data,
  - ``URLSearchParams``, to submit the data in ``x-www-form-urlencoded`` encoding, rarely used.

```JavaScript
let user = {
  name: 'John',
  surname: 'Smith'
};

let response = await fetch('/article/fetch/post/user', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json;charset=utf-8'
  },
  body: JSON.stringify(user)
});

let result = await response.json();
alert(result.message);
```

Please note, if the request ``body`` is a **string**, then ``Content-Type`` header is set to ``text/plain;charset=UTF-8`` by default.

But, as we’re going to send ``JSON``, we use headers option to send ``application/json`` instead, the correct ``Content-Type`` for **JSON-encoded** data.

### 1.4 Sending an image

### 1.5 Summary

A typical fetch request consists of two ``await`` calls:

```JavaScript
let response = await fetch(url, options); // resolves with response headers
let result = await response.json(); // read body as json
```

Or, without ``await``:

```JavaScript
fetch(url, options)
  .then(response => response.json())
  .then(result => /* process result */)
```

**Response properties:**

- ``response.status`` – HTTP code of the response,
- ``response.ok`` – true is the status is 200-299.
- ``response.headers`` – Map-like object with HTTP headers.

**Methods to get response body:**

- ``response.text()`` – return the response as text,
- ``response.json()`` – parse the response as JSON object,
- ``response.formData()`` – return the response as FormData object (form/multipart encoding, see the next chapter),
- ``response.blob()`` – return the response as Blob (binary data with type),
- ``response.arrayBuffer()`` – return the response as ArrayBuffer (low-level binary data),

**Fetch options so far:**

- ``method`` – HTTP-method,
- ``headers`` – an object with request headers (not any header is allowed),
- `body` – the data to send (request body) as string, FormData, BufferSource, - ``Blob`` or ``UrlSearchParams`` object.