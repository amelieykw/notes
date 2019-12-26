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



## 5. **Rate limiting**


## 6. **Connection close**


## 7. **Connection state**


## 8. **Chat example**



## 9. **Summary**