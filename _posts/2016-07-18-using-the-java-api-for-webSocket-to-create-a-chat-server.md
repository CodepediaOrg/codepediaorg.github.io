---
layout: post
title: Using The Java Api For Websocket To Create A Chat Server
description: "In this post, I’ll use Web sockets to create a tiny chat server using Tyrus, the reference implementation of the Java API for WebSocket (JSR 356)."
author: benas
permalink: /benas/using-the-java-api-for-webSocket-to-create-a-chat-server.html
categories: [java]
tags: [java, web sockets]

republished: true
original_title: Using The Java Api For Websocket To Create A Chat Server
original_url: http://benas.github.io/2016/02/21/using-the-java-api-for-webSocket-to-create-a-chat-server.html
---

In a [previous post](http://benas.github.io/2014/10/14/using-server-sent-events-to-create-a-monitoring-dashboard.html), I've used Server Sent Events to create a monitoring dashboard. SSE are a one way messaging format form server to clients
 in contrast to Web Sockets where communication is bidirectional. In this post, I'll use Web sockets to create a tiny chat server using [Tyrus](https://tyrus.java.net/), the reference
 implementation of the Java API for WebSocket (JSR 356). A great introduction to this API can be found on Oracle Network [here](http://www.oracle.com/technetwork/articles/java/jsr356-1937161.html).

 In order to keep the tutorial simple, the server and clients will be command line apps, no GUIs here, it is a serious blog :smile: So let's get started!
 <!--more-->

## A quick introduction to the Java API for WebSocket

If you don't have time to check all details of this API, here are the most important points to know. JSR 356 provides:

* both a programmatic and a declarative way to define client and server endpoints. In this post, I'll use the declarative way, through annotations.
* lifecyle events to which we can listen: open/close a session, send/receive a message, etc
* message encoder/decoder to marshal/unmarshall (binary or character) messages between clients and servers.
Encoders/Decorders are important since they allow us to use Java objects as messages instead of dealing with raw data that transit over the wire. We will see an example in a couple of minutes

That's all we need to know for now in order to implement the chat application.

## The data model

Since we will be creating a chat server, let's first model messages with a POJO:

```java
public class Message {

    private String content;
    private String sender;
    private Date received;

    // getters and setters
}
```

Messages will be encoded/decoded to JSON format between the server and clients using the following `Encoder/Decoder` classes:

```java
public class MessageEncoder implements Encoder.Text<Message> {

    @Override
    public String encode(final Message message) throws EncodeException {
        return Json.createObjectBuilder()
                       .add("message", message.getContent())
                       .add("sender", message.getSender())
                       .add("received", "")
                       .build().toString();
    }

}
```

```java
public class MessageDecoder implements Decoder.Text<Message> {

    @Override
    public Message decode(final String textMessage) throws DecodeException {
        Message message = new Message();
        JsonObject jsonObject = Json.createReader(new StringReader(textMessage)).readObject();
        message.setContent(jsonObject.getString("message"));
        message.setSender(jsonObject.getString("sender"));
        message.setReceived(new Date());
        return message;
    }

}
```

The `MessageEncoder` uses the [Java API for JSON Processing](https://jsonp.java.net/) to create Json objects, no additional libraries :wink:

That's all for the data model, we have created our domain model object `Message`, and two utility classes to serialize/deserialize messages to/from JSON format.

## The server side

The following is the code of the server endpoint. I'll explain it in details right after the listing:

```java
@ServerEndpoint(value = "/chat", encoders = MessageEncoder.class, decoders = MessageDecoder.class)
public class ServerEndpoint {

    static Set<Session> peers = Collections.synchronizedSet(new HashSet<Session>());

    @OnOpen
    public void onOpen(Session session) {
        System.out.println(format("%s joined the chat room.", session.getId()));
        peers.add(session);
    }

    @OnMessage
    public void onMessage(Message message, Session session) throws IOException, EncodeException {
        //broadcast the message
        for (Session peer : peers) {
            if (!session.getId().equals(peer.getId())) { // do not resend the message to its sender
                peer.getBasicRemote().sendObject(message);
            }
        }
    }

    @OnClose
    public void onClose(Session session) throws IOException, EncodeException {
        System.out.println(format("%s left the chat room.", session.getId()));
        peers.remove(session);
        //notify peers about leaving the chat room
        for (Session peer : peers) {
            Message message = new Message();
            message.setSender("Server");
            message.setContent(format("%s left the chat room", (String) session.getUserProperties().get("user")));
            message.setReceived(new Date());
            peer.getBasicRemote().sendObject(message);
        }
    }

}
```

The code is self explanatory, but here are important points to note:

* The `ServerEndpoint` annotation marks the class as a server endpoint. We need to specify the web socket endpoint as well as message encoder/decoder.
* We need to hold a set of connected users in order to broadcast messages to chat rooms
* All lifecycle event methods have a `Session` parameter that allows to get a handle of the user that initiated the action (join/leave/message)
* `MessageEncoder` and `MessageDecoder` are used to marshal/unmarshal messages from JSON to our `Message` class. This is what allows us to have a parameter of type
`Message` in the `onMessage` method. Without these encoder/decoder classes, we need to handle raw text messages..

That's all for the server side, nothing fancy :smile:

## The client side

On the client side, we need to listen to the `onMessage` event and print the received message to the console:

```java
@ClientEndpoint(encoders = MessageEncoder.class, decoders = MessageDecoder.class)
public class ClientEndpoint {

    private SimpleDateFormat simpleDateFormat = new SimpleDateFormat();

    @OnMessage
    public void onMessage(Message message) {
        System.out.println(String.format("[%s:%s] %s",
         simpleDateFormat.format(message.getReceived()), message.getSender(), message.getContent()));
    }

}
```

## Putting it all together

Now that we have defined the data model, client and server endpoints, we can proceed with a final main class to run the application.

The main class to run the server is the following:

```java
public class Server {

    public static void main(String[] args) {
        org.glassfish.tyrus.server.Server server =
            new org.glassfish.tyrus.server.Server("localhost", 8025, "/ws", ServerEndpoint.class);
        try {
            server.start();
            System.out.println("Press any key to stop the server..");
            new Scanner(System.in).nextLine();
        } catch (DeploymentException e) {
            throw new RuntimeException(e);
        } finally {
            server.stop();
        }
    }

}
```

All we have to do is create a `org.glassfish.tyrus.server.Server` instance and start it.
We should specify the `ServerEndpoint` class, the port and the websocket endpoint as parameters.

To create a client, I'll use the following class:

```java
public class Client {

    public static final String SERVER = "ws://localhost:8025/ws/chat";

    public static void main(String[] args) throws Exception {
        ClientManager client = ClientManager.createClient();
        String message;

        // connect to server
        Scanner scanner = new Scanner(System.in);
        System.out.println("Welcome to Tiny Chat!");
        System.out.println("What's your name?");
        String user = scanner.nextLine();
        Session session = client.connectToServer(ClientEndpoint.class, new URI(SERVER));
        System.out.println("You are logged in as: " + user);

        // repeatedly read a message and send it to the server (until quit)
        do {
            message = scanner.nextLine();
            session.getBasicRemote().sendText(formatMessage(message, user));
        } while (!message.equalsIgnoreCase("quit"));
    }

}
```

The `ClientManager` provided by JSR 356 is the entry point to create clients.
Once we get a client, it is possible to connect to the server and obtain a `Session` object. This is the main point of interaction between server and clients.
It allows to send messages, register handlers and get/set sessions parameters.

The complete source code of the tutorial can be found [here](https://github.com/benas/web-socket-lab).
In this lab, I've created [an example](https://github.com/benas/web-socket-lab#example) with one server and two clients.

## Summary

In this post, we have seen how easy is to create a chat application using the Java API for WebSocket (JSR 356).
 This API has been added in Java EE 7 and greatly simplify the programming model of real-time interactive applications.

 Note that I've used only reference implementations of Java EE APIs, without using a container, nor third party APIs or libraries.
 If only I meet one of those guys that say Java EE is heavy :rage:

 I've already used a Javascript stack for this kind of requirements, basically to implement a more elaborate [real-time game server](https://github.com/benas/gamehub.io) with NodeJs and SocketIO.
 I must admit that the Java API is also really great, easy and straightforward.

 I hope you enjoyed this article. Time to get your hands dirty..
