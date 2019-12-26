# Long polling

> **Long polling is the simplest way of having persistent connection with server,**
>
> that doesn’t use any specific protocol like WebSocket or Server Side Events.

1. **Regular polling**
2. **Long polling**
3. **Demo: a char**
4. **Area of usage**

## 1. Regular Polling

---

The **simplest** way to get new information from the server is **periodic polling**. 
1. regular requests to the server: “*Hello, I’m here, do you have any information for me?*”, e.x., once/10 seconds)
2. In response, the server :
   1. first - takes a notice to itself that the **client is online**
   2. second – **sends a packet of messages** it got till that moment

#### Disadvantages
- Messages are passed with a delay up to 10 seconds (between requests).
- Even if there are no messages, the server is bombed with requests every 10 seconds, even if the user switched somewhere else or is asleep.
  - That’s quite a load to handle, speaking performance-wise.

## 2. Long polling

---

The flow (delivers messages **without delays**):

1. A request is sent to the server.
2. The server doesn’t close the connection until it has a message to send.
3. When a message appears – the server responds to the request with it.
4. The browser makes a new request immediately.

The situation when the browser sent a request and has a pending connection with the server, is standard for this method. Only when a message is delivered, the connection is reestablished. If the connection is lost, because of, say, a network error, the browser immediately sends a new request.If the connection is lost, because of, say, a network error, the browser immediately sends a new request.

![Long Polling](/jsa_mentoring/js_network_request/Long_Polling.png)

## 3. Demo: a chat

---

[Demo](https://javascript.info/long-polling#long-polling)

## 4. Area of usage

--- 

> **Long polling works great in situations when messages are rare.**

If messages come very often, then the chart of requesting-receiving messages, painted above, becomes saw-like.

Every message is a separate request, supplied with headers, authentication overhead, and so on.

So, in this case, another method is preferred, such as ``Websocket`` or ``Server Sent Events``.