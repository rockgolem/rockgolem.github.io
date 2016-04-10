title: "Game Dev Diary: Scrolls, Runes, and Dice"
date: 2016-04-10 10:08:00
author: Stephen Young
category: Game Dev Diary
banner: /img/smite-scroll.jpg
thumbnail: /img/smite-scroll.jpg
tags:
  - scrolls
  - runes
  - development
  - dice
---

We've been banging away at our keyboards for the past few weeks to improve our game design and implement a few new features.  We focused on reducing complexity without compromising depth, and moving closer to completing the core game mechanics.

Here are some highlights:

* Streamlined the Rune card mechanics
* Simplified the Scroll card mechanics
* Improved the dice design for better depth of play

<!-- more -->

## Rune Cards
I conceived the initial Rune card mechanic in the initial design phase (back when I was working solo on the project).  The idea worked like this:  each player's avatar was surrounded by four "rune slots."  Each slot was represented by one of the cardinal directions (i.e. `North`, `East`, `South`, `West`).  As a player acquired Rune cards, they placed these cards in one of the four rune slots.  Placed Rune cards would apply permanent effects to the players, the arena, or their opponents.  Sometimes Rune cards would apply different effects depending on the slot in which they were played.  For example, a Rune card might offer the player `+1 Armor` if played in the `North` slot, but `+1 Evasion` if played in the `East` slot.  Once all four positions were full, the player needed to trash a played Rune card in order to play another one.

This system was interesting, and provided a certain amount of choice when acquiring and placing Rune cards.

However, it had a major drawback.

### The Noob Learning Curve
If our gameplay mechanics and rules are not intuitive, new players are going to have a more difficult time understanding how to play.

* Hardly anyone reads on-screen "explanation" text;
* the pace of the game doesn't lend itself easily to reading text anyway;
* and, [Chris](https://twitter.com/cjsaylor) and I both agree that forced tutorials suck.

Card games like these will inherently require the player to learn some rules, but we hope to minimize the learning curve as much as possible.

Looking at the Rune card mechanic from the perspective of a new player reveals a layer of complexity that is not immediately intuitive.  What do these cardinal directions mean?  For Rune cards that grant the same effect in all positions, the cardinal directions are meaningless.  Rune cards that apply different effects based on positions need to display a lot of complex text to convey this mechanic to the player (again, people don't usually read).

### The New Rune Mechanic
To solve this problem, we cut out the positions entirely.  Now, the player has four un-labeled rune slots; there are no differences between each slot.  Rune cards do the same thing no matter the slot in which they are played.  A new player selects "play rune" and the rune card just works without further thought.

One unfortunate drawback of this new system is the lack of depth for more experienced players.  To combat this problem we conjured up the idea of "Set Traits" and "synergies."  "Set traits" will be traits that increase their power or change their effects when played together with another Rune card that has a trait from the same Set.  Synergies, as the name implies, will be new effects that are applied when a combination of specific traits from different Rune cards are detected.

New players might stumble into these Rune card combos, but experienced players will probably be building their decks with these combos in mind.

This past week I put the finishing touches on the initial code, and implemented the Armor trait functionality.  One down, many more to go.

## Scroll Cards
We also made some changes to the Scroll card rules.  Scroll cards are "instant" spells that can be played at any time.  Think "Direct Damage Fireball" that just damages your opponent outright for a small amount.  Originally, when a Scroll card was played, we intended to start a short 10 second timer to allow other players to throw down their own Scroll cards.  Once all players finished playing Scrolls, the cards would be resolved Last In First Out.  For example:

1. Tim plays a "Smite Scroll" and targets John.  The "Smite Scroll" will damage John by 2 points if it resolves, because it has a `+2 Direct Damage` trait.
2. Before the 10 second timer is up, John plays a "Nullify Scroll" and targets the "Smite Scroll."  The "Nullify Scroll" removes all traits from any targeted scroll.
3.  Finally the timer ends.  The "Nullify Scroll" was the last card played, so it is resolved first.  It removes the `+2 Direct Damage` trait from the "Smite Scroll."
4.  The "Smite Scroll" then resolves and does nothing, because it has no traits.

This is a standard implementation of instant cards, but it has two problems in the context of our game.

### Pacing
Most of the gameplay in Witches &amp; Warlocks is pretty fast paced.  Players make simultaneous moves during the Upkeep and Spellcraft phases, and only slow down to take turns during the Combat phase.  Forcing the players to stop what they are doing for a 10 second timer slows down the game significantly.  Even if we allow players to click a button to skip the timer (e.g. they don't have any Scroll options to play in response to someone else's Scroll), they still had to stop what they were doing or thinking about.  We would rather have players focused on their strategy.

### Poor Odds
Players discard their entire hand and draw a new hand *every round*.  In the above example, the odds that John actually has the "Nullify Scroll" card in his hand at the same time that Tim plays the "Smite Scroll" is low in the beginning of the game, and pretty rare in the late game as their decks get larger.  It just doesn't make sense to give players the chance to respond with Scroll cards from other players.

### The New Scroll Mechanic
As we did with the Rune card position mechanic, we cut the Scroll card timer mechanic entirely.  Scroll cards can still be played at any time, but they resolve as soon as they are played.  There are no timers, so the pace of the game stays fast.  Instead of responding to Scroll cards with other Scroll cards, we'll provide some Rune card options that the players can employ to protect themselves.

Yesterday I put the finishing touches on the initial Scroll card code, and I implemented the "Direct Damage" trait.

## Dice
A few months ago Chris expressed frustration with the current dice design.  Here's how they originally worked:  Every Spell card had a `+X Dice` trait, where X was the number of dice that you would add to your attack if you added this card to your spell.  These were six-sided dice with the following values (crits ignore armor): `Zero`, `Zero`, `One`, `One Crit`, `Two`, and `Two Crit`. Ignoring the crits for now, if you averaged the values of the sides you got: `(0+0+1+1+2+2)/6=1`.  So in effect, each die that a Spell card added to your final spell added an average of 1 damage.  This was a nice round number that I really liked.

The problem was that basic spell cards&mdash;like the core "Eldritch Blast" card that everyone starts the game with&mdash;only add one die to the spell.

### Rolling Zero
So in the beginning, players roll a decent number of Zeros, causing them to do no damage.  Even though the players would sometimes roll a Two&mdash;causing their average damage output to be 1 damage per die over the course of the match&mdash;the psychological effect of sometimes rolling all goose eggs and doing no damage was strong and very frustrating.

### The New Dice Mechanic
After much deliberation, we came up with an idea that not only solves the problem, it adds another layer of depth to the dice game.

We came up with the idea of multiple types of dice:  Fast dice, Medium Dice, and Strong Dice.  A given card might have `+2 Fast Dice` instead of the old `+2 Dice`.

These dice have different values on their sides.  Here's a breakdown of each type and some of their attributes:

#### Fast Dice
* Low damage, low crit dice that will never miss.
* Sides [1, 1, 2, 2, 3, 3c]
* 1-3 damage, average 2 damage per die
* Even chance to roll 1, 2 or 3
* 16% chance to crit
* 0% chance to miss

#### Medium Dice
* Good damage, good chance to crit, small chance to miss.
* Sides [0, 2, 2c, 4, 4c, 6]
* 0-6 damage, average 3 damage per die
* 66% chance to roll 2 or 4
* 33% chance to crit
* 16% chance to miss
* 16% chance to roll 6

#### Strong Dice
* Great damage, good chance to crit, good chance to miss.
* Sides [0, 0, 5, 5c, 10, 10c]
* 0-10 damage, average 5 damage per die
* Even chance to roll 0, 5 or 10
* 33% chance to crit
* 33% chance to miss

The aforementioned "Eldritch Blast" cards now start with `+1 Fast Dice` so they never miss.  With this new system, players can build their decks with safe dice, or acquire cards that offer the chance to do strong damage but might miss entirely.

This new Dice mechanic has not yet been implemented, but we're going to start writing the code for it pretty soon.  Probably this coming week.

## Getting Closer to Feature Complete
We only have a few more core mechanics to implement before we can start iterating on content.  After the above dice mechanic is done, we are going to work on properly showing the player what spell cards are played by their opponents, and the results of their dice rolls (right now we just show the damage done).  We also have a few small things to add to the Vault system for spending energy on cards during the game.  After that we're going to reconvene and talk about our next steps before adding lots of content to our procedural card generator.  We'll keep you posted on the blog as soon as we know more.