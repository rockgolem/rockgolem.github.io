title: 'Game Dev Diary: Concerning Timers'
date: 2016-05-23 13:49:31
author: Chris Saylor
category: Game Dev Diary
banner: /img/timers_header.jpg
thumbnail: /img/timers_header.jpg
tags:
  - development
  - timers
  - infrastructure
---

In the history of games, time is a major factor in the rules. Whether it's the time a ball has to bounce in order to grab some metal jacks or the time to take your turn in chess, time is an integral part of any game, and this game is no exception.

<!--more-->

We have recently built in the use of timers in three distinct use cases: preventing players from stalling the game, waiting for players to rejoin after all disconnect, and controlling animations. I'll go into some technical detail at the end.

## Simultaneous play...almost

One major feature of our game is that it is quasi-turn based. It is a mix of both realtime play (building spells, buying cards from the vault, casting scrolls, etc) and turn-based play (selecting targets and resolving combat). Anytime there is some sort of turn for a player to make a decision, everyone else has to wait. What happens when a player never makes their decision? A player can't halt the game indefinitely or it just wouldn't be fun, so we designed some phase timers to expire a player's turn if they are taking too long. If everyone is engaged in the game, you may never see a timer expire as it can be quite fast-paced. These timers represent the longest period any of these phases *should* take before it becomes not fun.

## To Rejoin or not to rejoin

We keep state for each player to know if they are part of an active game. If the client quits or is disconnected, the first thing we do upon loading up the client is see if the player has an active game. If so, we put them right back into the action. We use a timer once all players have disconnected from an active game that hasn't concluded yet, and in the event of no-one rejoining within a time limit, we end the game. We're still in the process of deciding how this effects player's stats or win/loss record so stay tuned for that.

## Animation timing

In our original design, we allowed the client to get events from the server as quickly as they came in, and then we would buffer them in the client so that they appeared in sequence and properly delayed based on what was animating. We have gone away from this design in all but a few places that have sequential dependencies. Certain events can't be delayed from the server (ie when a timer starts), which means that a players time in spell or combat phase was being consumed by animation time. To solve this, we started setting a timer at key moments on the server side to account for average animation time, so that future events would not occur until after the timer expired. This allows us to animate actions and have server timers be in sync with the client.

## Distributed timers

__Here be dragons__.

Now we'll go into the technical details of how we pulled this off. If you read [my previous post about our system](/Game-Dev-Diary/Infrastructure-Journey.html), you'll know we use a microservice approach, one of which is a timer service. Since our architecture is built for running multiple instances of these services for scale, we had to get a bit creative in our approach to timers. We use nodejs, so there is already a `setTimeout` function available, but there are two main reasons we can't use it:

1. What happens if that particular instance of the timer service goes down or is restarted? (the game would stop progressing)
2. What if an event comes in that should cancel a timer? With multiple timer services running, we can't guarantee the service that has the timer will receive the cancellation event.

Instead, we decided to use [Redis](http://redis.io/) as a stateful backend for all our timers and a library called [dtimer](https://www.npmjs.com/package/dtimer) to coordinate. This setup allows for one timer service to start a timer, and any timer service can cancel or listen to its expiration.
