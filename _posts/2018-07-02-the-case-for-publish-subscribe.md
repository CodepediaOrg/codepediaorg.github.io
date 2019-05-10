---
layout: post
title: The case for publish/subscribe in your software architecture considerations
description: "The case for publish/subscribe in your architecture considerations: loose coupling and asynchronous
consumption of messages"
author: ama
permalink: /ama/the-case-for-publish-subscribe-in-software-architecture
published: true
categories: [architecture]
tags: [architecture, design-patterns, cloud]
---

So you are developing a core platform for your enterprise. Your platform becomes successful, you have more clients consuming 
 your services. Some (most) will need some core data synchronized in their systems. You might start experiencing the following pains

## Pains

1. You might use a push mechanism to notify clients about an core element update (e.g. user or organisation data update).
 But what happens when a new client ist added? Well your core platform needs to be aware of it. This might imply code or configuration changes,
 new deployments etc. - ** it does not scale **
2. If a client of the platform is not online the time the update notification is pushed, it will miss the update, which might lead to data inconsistency.

## Pub/Subscribe to the rescue

Both issues are elegantly solved if you use a publish/subscribe mechanism. Your core platform, the publisher, will publish the update event to a message
broker, that will be consumed by one or several clients, subscribers. See the diagram below for the 1000 words, worth of explanation:

<!--more-->
<figure>
   	<img src="http://www.codingpedia.org/images/posts/publish-subscribe-architecture/publish-subscribe-architecture.png" alt="" />
   	<figcaption>Model architecture for a publish/subscribe scenario in enterprise</figcaption>
</figure>

Well, a few words won't hurt anyone: your core platform might get relevant notification events from business or private clients.
now instead of pushing the notification to the consuming clients it publishes it to a topic (might be a cloud service), where is persisted
for a configured duration or consumed by all subscribers.

The simplest scenario is to use a broadcast pattern for the topic where every subscription gets a copy of each message sent to the topic. Other topologies 
could be set up for more advanced scenarios via message filtering and routing. This usually involves additional topics or queues. Most potent brokers offer
such abilities.


### Benefits

The benefits are the solving of the previously mentioned points:
- **decoupling**: the publisher is not aware anymore of consuming clients
- **asynchronous consumption (offline support)**: subscribers might consume the messages at a later time

This is no silver bullet for all your architecture needs, you might need tightly coupled systems to guarantee message delivery for example. 
Just don't forget about it, when you are designing your next (enterprise) project.

# References:

* https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern
