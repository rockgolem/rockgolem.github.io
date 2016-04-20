title: "Game Dev Diary: Infrastructure Journey"
date: 2016-04-20 10:08:00
author: Chris Saylor
category: Game Dev Diary
banner: /img/infrastructure-overview.png
thumbnail: /img/infrastructure-overview.png
tags:
  - ops
  - infrastructure
  - development
  - containers
---

We've been talking about design and game mechanics, and you've seen the progress we've been making with the client. In this post, we're going to take a peek under the hood and explore how the game servers are put together and how we plan to scale them up.

<!--more-->

## The Stack

First things first, our server code runs on [Node.js](https://nodejs.org). As you can see from the above diagram, we are running the application in a microservice architecture. We have a relatively large number of small game services that each perform specific tasks. For example, when you request to join a multiplayer game, the `matchmaker` service is responsible for finding a game for you. 

Our microservices run in [Docker Containers](https://www.docker.com/) making them easy to deploy and spin up additional service instances on demand. All of the services communicate in an asynchronous fashion via a message queue application called [RabbitMQ](https://www.rabbitmq.com/).

Finally, each service stores data that it cares about in [MongoDB](https://www.mongodb.org/) and [Redis](http://redis.io/).

During our upcoming alpha phase, we'll be deploying on small instances on a VPS like [DigitalOcean](https://www.digitalocean.com/), however during the beta (and ultimately after release), we'll be deploying to [AWS](http://aws.amazon.com/) or [GCE](https://cloud.google.com/compute/). It's very simple for us to deploy in virtually any cloud computing service due to our container infrastructure.

Read more about the [details of our stack](http://stackshare.io/cjsaylor/rockgolem).

## We don't ask our servers, we tell them.

The principle mechanism that makes our architecture so scalable is our asynchronous message queue and websocket connections. Instead of making a request to the server and waiting on a response before proceeding, we tell the server what the player did over a socket connection and react to messages from the server telling us what it did. This allows us to do a lot of tricks on the client side to hide any latency a player might experience. In addition, all of the services have an option to react to each event in their own way.

This also makes it so each service can have a "local" (read: local in region) database which allows us to scale horizontally almost infinetly (or until we reach RabbitMQs limits). Each service is listening to all events, which means the individual service can store only the information it needs. You can see all of the events we process in the following graph:

![All events emitted and listened to by all game services.](/img/event-graph.png)

[Stephen](https://twitter.com/young_steveo) made a very nifty [Grunt](http://gruntjs.com/) task that scans the source code and generates a [`.dot` file](https://en.wikipedia.org/wiki/Graphviz) that rendered the graph above.

---

That's it for now. Next time, we'll talk about how we secure the communication between the client and the server.


