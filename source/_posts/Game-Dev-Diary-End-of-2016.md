title: 'Game Dev Diary: End of 2016'
date: 2016-12-22 23:13:20
author: Chris Saylor
category: Game Dev Diary
tags:
  - development
  - indiedev
  - gamedev
---

It has been a very busy year for us. We are slowly-but-steadily approaching
pre-alpha stage. Our biggest achievment for the year was completing the game loop.
Our core game loop works from start to finish with all stages and
a winning condition.

**HURRAY!**

<!--more-->

We still have quite a ways to go. While the game is playable, it isn't terribly
interesting since there is not a lot of card effect variaty. We have put a big focus
on completing a large list of card traits in order to start seeing some strategy
start to emerge from game play.

## Game Changes

One major change we've made in the core gameplay is how our scrolls (instant spells)
function. They no longer apply their effect immediately, but rather are now applied
to the target as a debuff. Their effects get applied at the beginning of
the combat phase. The reason we decided to delay the scroll effects were two fold:

1. During spell crafting, it can be a bit chaotic and you may miss an enemy player
   hitting you with instant spells.
2. There is literally no way to react to or prevent enemy scroll effects hitting you.
   Depending on the effect of the scroll, this could drastically change your strategy
   for the next combat phase and we felt it was a little unfair to the player not to
   have an opportunity to react (even if the opportunity is minimal).

Here's a sample of how it will look:

![Now smite applies a damage debuff instead of doing the damage immediately.](https://cloud.githubusercontent.com/assets/157755/21443407/f3fe4edc-c872-11e6-94ba-5d79de18bd96.gif)

## What does next year bring?

Traits. Lots of traits. We will continue to add (and evaluate) new traits to be
available on our cards. Once we have what we feel is enough diversity in strategy
with traits, we will divide our efforts on art and sound work.