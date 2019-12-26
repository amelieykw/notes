# [Web Socket](https://javascript.info/websocket#summary)

> The WebSocket protocol provides a way to exchange data between browser and server via a persistent connection.
>
> The data can be passed in both directions as **“packets”**, without breaking the connection and additional HTTP-requests.
>
> WebSocket is especially great for services that **require continuous data exchange**

1. **A simple example**
2. **Opening a websocket**
3. **Extensions and subprotocols**
4. **Data transfer**
5. **Rate limiting**
6. **Connection close**
7. **Connection state**
8. **Chat example**
9. **Summary**

## 1. **A simple example**

To open a websocket connection, we need to **create new WebSocket** using the special protocol ``ws`` in the url:

```JavaScript
let socket = new WebSocket("ws://javascript.info");
```

(encrypted ``wss://`` protocol. It’s like **HTTPS** for websockets.)

> ### **Always prefer ``wss://``**
> ---
> 
> The ``wss://`` protocol not only encrypted, but also more reliable.
>
> That’s because ``ws://`` data is not encrypted, visible for any intermediary. Old proxy servers do not know about WebSocket, they may see “strange” headers and abort the connection.
>
> On the other hand, ``wss://`` is WebSocket over TLS, (same as HTTPS is HTTP over TLS), the transport security layer encrypts the data at sender and decrypts at the receiver. So data packets are passed encrypted through proxies. They can’t see what’s inside and let them through.

**Once the socket is created, we should listen to events on it.** There are totally 4 events:

1. ``open`` – connection established,
2. ``message`` – data received,
3. ``error`` – websocket error,
4. ``close`` – connection closed.
5. ``socket.send(data)`` - send something

```JavaScript
let socket = new WebSocket("wss://javascript.info/article/websocket/demo/hello");

socket.onopen = function(e) {
  alert("[open] Connection established");
  alert("Sending to server");
  socket.send("My name is John");
};

socket.onmessage = function(event) {
  alert(`[message] Data received from server: ${event.data}`);
};

socket.onclose = function(event) {
  if (event.wasClean) {
    alert(`[close] Connection closed cleanly, code=${event.code} reason=${event.reason}`);
  } else {
    // e.g. server process killed or network down
    // event.code is usually 1006 in this case
    alert('[close] Connection died');
  }
};

socket.onerror = function(error) {
  alert(`[error] ${error.message}`);
};
```

## 2. **Opening a websocket**

When ``new WebSocket(url)`` is created, it starts connecting immediately.

During the connection the browser (using headers) asks the server: “Do you support Websocket?” And if the server replies “yes”, then the talk continues in WebSocket protocol, which is not HTTP at all.

![Web Socket](/jsa_mentoring/js_network_request/web_socket.png)

Here’s an example of browser headers for request made by ``new WebSocket("wss://javascript.info/chat")``.

```HTTP
GET /chat
Host: javascript.info
Origin: https://javascript.info
Connection: Upgrade
Upgrade: websocket
Sec-WebSocket-Key: Iv8io/9s+lYFgZWcXczP8Q==
Sec-WebSocket-Version: 13
```

- **``Origin``** – the origin of the client page.
  - WebSocket objects are ***``cross-origin`` by nature***.
  - There are no special headers or other limitations.
  - But Origin header is important, as it allows the server to decide whether or not to talk WebSocket with this website.
- **``Connection: Upgrade``** – signals that the client would like to change the protocol.
- **``Upgrade: websocket``** – the requested **protocol** is ``“websocket”``.
- **``Sec-WebSocket-Key``** – a **random browser-generated key** for security.
- **``Sec-WebSocket-Version``** – WebSocket protocol version, 13 is the current one.

> ### **WebSocket handshake can’t be emulated**
>
> ---
>
> We can’t use ``XMLHttpRequest`` or ``fetch`` to make this kind of HTTP-request, because JavaScript is not allowed to set these headers.

If the **server** agrees to switch to WebSocket, it should send code ``101`` response:

```HTTP
101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: hsBlbuDTkk24srzEOTBUlZAlC2g=
```

Here ``Sec-WebSocket-Accept`` is ``Sec-WebSocket-Key``, recoded using a special algorithm. **The browser uses it to make sure that the response corresponds to the request.**

## 3. **Extensions and subprotocols**



## 4. **Data transfer**

WebSocket communication consists of **“frames”** – **data fragments**, that can be sent from either side, and can be of several kinds:

- **“``text frames``”** – contain ***text data*** that parties send to each other.
- **“``binary data frames``”** – contain ***binary data*** that parties send to each other.
  - **“``ping/pong frames``”** are used to ***check the connection***, sent from the server, the browser responds to these automatically.
- there’s also **“``connection close frame``”** and a few ***other service frames.***

> In the browser, we directly work only with ``text`` or ``binary`` frames.

> **WebSocket .send() method can send either text or binary data.**

A call ``socket.send(body)`` allows body in **string** or a **binary** format, including **Blob**, **ArrayBuffer**, etc. No settings required: just send it out in any format.

> When we **receive** the data, **text** always comes as string. And for **binary** data, we can choose between **Blob** and **ArrayBuffer** formats.

That’s set by ``socket.bufferType`` property, it’s "blob" by default, so binary data comes as Blob objects.

**Blob** is a high-level binary object, it directly integrates with ``<a>``, ``<img>`` and other tags, so that’s a sane default. But for binary processing, to access individual data bytes, we can change it to "``arraybuffer``":

```JavaScript
socket.bufferType = "arraybuffer";
socket.onmessage = (event) => {
  // event.data is either a string (if text) or arraybuffer (if binary)
};
```

## 5. **Rate limiting**

We can call ``socket.send(data)`` **again and again**. **But the data will be buffered (stored) in memory and sent out only as fast as network speed allows.**

The ``socket.bufferedAmount`` property stores how many bytes are buffered at this moment, waiting to be sent over the network.

We can examine it to see whether the socket is actually available for transmission.

```JavaScript
// every 100ms examine the socket and send more data
// only if all the existing data was sent out
setInterval(() => {
  if (socket.bufferedAmount == 0) {
    socket.send(moreData());
  }
}, 100);
```

## 6. **Connection close**

Normally, when a party wants to close the connection (both browser and server have equal rights), they send a “connection close frame” with **a numeric code and a textual reason.**

```JavaScript
socket.close([code], [reason]);
```

- **``code``** is a special WebSocket **closing code** (optional)
- **``reason``** is a string that describes **the reason of closing** (optional)

Then the other party in close event handler gets the code and the reason, e.g.:

```JavaScript
// closing party:
socket.close(1000, "Work complete");

// the other party
socket.onclose = event => {
  // event.code === 1000
  // event.reason === "Work complete"
  // event.wasClean === true (clean close)
};
```

Most common code values:

- 1000 – the default, normal closure (used if no code supplied),
- 1006 – no way to such code manually, indicates that the connection was lost (no close frame).

```JavaScript
// in case connection is broken
socket.onclose = event => {
  // event.code === 1006
  // event.reason === ""
  // event.wasClean === false (no closing frame)
};
```

There are other codes like:

- 1001 – the party is going away, e.g. server is shutting down, or a browser leaves the page,
- 1009 – the message is too big to process,
- 1011 – unexpected error on server,
…and [so on](https://tools.ietf.org/html/rfc6455#section-7.4.1).

WebSocket codes are somewhat like HTTP codes, but different. In particular, any codes less than 1000 are reserved, there’ll be an error if we try to set such a code.

## 7. **Connection state**

To get connection state, additionally there’s socket.readyState property with values:

- 0 – “**CONNECTING**”: the connection has not yet been **established**,
- 1 – “**OPEN**”: **communicating**,
- 2 – “**CLOSING**”: the connection is **closing**,
- 3 – “**CLOSED**”: the connection is **closed**.

## 8. **Chat example**

a chat example using ``browser WebSocket API`` and ``Node.js WebSocket module``  [(https://github.com/websockets/ws)](https://github.com/websockets/ws)

HTML
```HTML
<!-- message form -->
<form name="publish">
  <input type="text" name="message">
  <input type="submit" value="Send">
</form>

<!-- div with messages -->
<div id="messages"></div>
```

Client side 

> From JavaScript we want three things:
>
> 1. Open the connection.
> 2. On form submission – socket.send(message) for the message.
> 3. On incoming message – append it to div#messages.

```JavaScript
let socket = new WebSocket("wss://javascript.info/article/websocket/chat/ws");

// send message from the form
document.forms.publish.onsubmit = function() {
  let outgoingMessage = this.message.value;

  socket.send(outgoingMessage);
  return false;
};

// message received - show the message in div#messages
socket.onmessage = function(event) {
  let message = event.data;

  let messageElem = document.createElement('div');
  messageElem.textContent = message;
  document.getElementById('messages').prepend(messageElem);
}
```

Server side

> The server-side algorithm will be:
>
> 1. Create ``clients = new Set()`` – a set of sockets.
> 2. For each accepted websocket, add it to the set ``clients.add(socket)`` and setup ``message`` event listener to get its messages.
> 3. When a message received: iterate over clients and send it to everyone.
> 4. When a connection is closed: ``clients.delete(socket)``.

```JavaScript
const ws = new require('ws');
const wss = new ws.Server({noServer: true});

const clients = new Set();

http.createServer((req, res) => {
  // here we only handle websocket connections
  // in real project we'd have some other code here to handle non-websocket requests
  wss.handleUpgrade(req, req.socket, Buffer.alloc(0), onSocketConnect);
});

function onSocketConnect(ws) {
  clients.add(ws);

  ws.on('message', function(message) {
    message = message.slice(0, 50); // max message length will be 50

    for(let client of clients) {
      client.send(message);
    }
  });

  ws.on('close', function() {
    clients.delete(ws);
  });
}
```

> Just don’t forget to install Node.js and ``npm install ws`` before running.

## 9. **Summary**

WebSocket is a modern way to have persistent browser-server connections.

- WebSockets don’t have cross-origin limitations.
- They are well-supported in browsers.
- Can send/receive strings and binary data.
- The API is simple.

Methods:

- ``socket.send(data)``,
- ``socket.close([code], [reason])``.

Events:

- ``open``,
- ``message``,
- ``error``,
- ``close``.
  
WebSocket by itself does not include reconnection, authentication and many other high-level mechanisms. So there are client/server libraries for that, and it’s also possible to implement these capabilities manually.

Sometimes, to integrate WebSocket into existing project, people run WebSocket server in parallel with the main HTTP-server, and they share a single database. Requests to WebSocket use ``wss://ws.site.com``, a subdomain that leads to WebSocket server, while ``https://site.com`` goes to the main HTTP-server.