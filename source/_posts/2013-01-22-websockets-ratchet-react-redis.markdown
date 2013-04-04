---
layout: post
title: "A Websockets, Ratchet, Silex and Redis pubsub implementation"
date: 2013-01-22 12:00
comments: true
categories: [php, ratchet, redis, silex, websockets]
published: false
---
I was approached by a betting/gambling development company who potentially needed a middleware building that would pull from an existing gambling web service and basically transmit to connected iPhone clients the changes from the web service.

At first, the obvious answer might be to create another REST web service that the iPhone clients could just ping for changes.  However, one of the devs explained that this wouldn't be fast enough, or scale - they'd need changes to be transmitted as soon as possible, as the app would be a real-time betting app and there'd be thousands of connections to the server.

I'd recently been using Redis which has a publish/subscribe feature and also knew about Websockets and node.js - but never had the chance to implement them.  I figured these could all fit together somehow and suggested a building a proof of concept.

Once I started to look around I discovered a PHP event driven programming solution, *Ratchet*.  I decided to use this in conjunction with *Silex*, and *Predis-async* to build the middleware, the idea being that anything can use Redis publish to communicate with the middleware Ratchet app, sending messages (in the case of the potential betting solution, changesets to the betting odds) to the front end.  I would use *Websockets* and *autobahnJS* on the front end (iOS apps can happily use Websockets to communicate with a server).

There was no documentation on using Ratchet or React's event loop with Predis-async so I've decided to make the solution available online, [on github jmoz/silex-test](https://github.com/jmoz/silex-test).  Here's a sample of the daemon where you can see the Predis-async integration:

{% gist 4594162 %}

The proof of concept app is running at [silex-test.jmoz.co.uk/pubsub](http://silex-test.jmoz.co.uk/pubsub).

Below is the text from the README.

# Websockets, Ratchet, Redis pubsub proof of concept

This is a proof of concept *HTML5 websocket app* showing how **ratchet and websockets** can be used along with **Redis' pubsub** publish and subscribe.

You can use the form below to subscribe/unsubscribe to a channel.  When you subscribe you also subscribe to the corresponding Redis channel.  So if using the cli you connect to Redis and do a `PUBLISH channel:jmoz foo`, the message will be published from Redis to the Websocket server then to the browser.

You can publish via websockets to the WS server which will then broadcast to connected clients.  This does not touch Redis.

You can make an AJAX request to this app which will then use Redis and `PUBLISH` which will be picked up by the WS server and broadcast to clients.  This is a way to demo the raw Redis pubsub without using the cli.

The following tech is used:

- [Ratchet](http://socketo.me/) - the core WAMP app that uses an event loop, handles websockets and subscribes to Redis.
- [Predis-async](https://github.com/nrk/predis-async) - the client library for Redis that implements the [react](http://reactphp.org/) event loop.
- [Silex](http://silex.sensiolabs.org/) - used as the framework and provides the AJAX endpoint to publish via Redis.
- [AutobahnJS](http://autobahn.ws/js) - the JS library to implement websockets.