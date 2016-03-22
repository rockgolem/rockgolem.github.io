title: Procedurally Generated Cards in Witches and Warlocks
date: 2016-03-22 18:12:42
banner: https://www.evernote.com/l/AQajdM8kKBNN1JzHZb8jVeQPmGKEY8qSjewB/image.png
thumbnail: https://www.evernote.com/l/AQajdM8kKBNN1JzHZb8jVeQPmGKEY8qSjewB/image.png
author: Stephen Young
category: Game Design
tags:
  - design
  - cards
  - proceedural
---

Last year I posted a quick [synopsis of Witches &amp; Warlocks](https://blog.rockgolem.com/Game-Design/Witches-Warlocks-A-Digital-Trading-Card-Game.html), a new digital trading card game that we're working on.  Since then, [Chris](https://twitter.com/cjsaylor) and I have been working on the core mechanics and multiplayer systems, and today I want to come up for air and talk about one of the most exciting features: procedurally generated cards.

Before I can do that, I need to take a step back and explain a bit about the different kinds of cards used in the game, as well as a few fundamental game concepts that influence the procedural generation.

<!-- more -->

## Types of Cards

* Spell Cards &mdash; these are the most common cards in the game; players combine spell cards to attack their opponents by matching a "port" on one side of the card with a port on another spell card during the Spellcrafting phase.  You can read more about [spell card port design here](https://blog.rockgolem.com/Game-Design/Initial-Card-Designs-and-Evolution-of-Game-Rules.html).
* Scroll Cards &mdash; these cards are canned spells that can be used during any phase, and their effects are usually instantaneous.
* Rune Cards &mdash; these cards grant permanent stat boosts, protection, and other effects to the players.  The player can have up to four rune cards in play at one time, represented by the cardinal directions (North, East, South, West).  A rune card might produce different effects depending on the position in which it is played (these differences are explained in the rules text of the cards).

When a card is generated, we assign it an element and an artifact, as described below.

## Elemental Domains

With the exception of a few non-elemental cards, every card in W&amp;W belongs to one of twelve elemental domains.  These domains dictate the properties of the cards when they are generated.  Here is a list of each element. Next to the element are a few keywords that we consider the general sense of what those domains might feel like.

* Fire &mdash; raw power, destruction, resurrection
* Water &mdash; transference, freezing, cleansing
* Metal &mdash; confinement, fabrication, corrosion
* Air &mdash; breath, electricity, flight
* Spirit &mdash; healing, willpower, protection
* Mental &mdash; ingenuity, engineering, psionics
* Illusion &mdash; misdirection, camouflage, confusion
* Demon &mdash; madness, nightmares, contracts
* Blood &mdash; life, death, poison
* Shadow &mdash; stealth, silence, predation
* Moon &mdash; radiance, energy, sacrifice
* Stone &mdash; defense, weight, bludgeoning

So, a Fire card might boost raw attack power, while a Spirit card might cause a healing effect.

## Schools of Sorcery
Cards in W&amp;W are also associated to one of three schools of sorcery.  Each school contains a grouping of elements and artifacts that might be used when generating a card.

### Classical
This school is rooted in warlock tradition.  It stands for order and balance, and is focused on maintaining equilibrium between offensive sorcery and defensive runes.

* **Domains**: Fire, Water, Metal, Air, Spirit
* **Artifacts**: Swords, Tomes, Dice, Potions, Rings, Chalices, Formulas, Chains

### Primal
Brute force and savage rituals define this school.  Erupting with flames&mdash;and guided by the light of the moon&mdash;Primal cards focus on attack and sacrifice.

* **Domains**: Blood, Shadow, Moon, Stone, Fire
* **Artifacts**: Daggers, Pouches, Hands, Roots, Horns, Lizards, Totems, Chants

### Magical
Cards from the Magical school rely on misdirection, manipulation, and control.  They tap into dreams and commune with demonic forces to overpower and debilitate their enemies.

* **Domains**: Spirit, Mental, Illusion, Demon, Blood
* **Artifacts**: Spheres, Lamps, Essences, Crystals, Wands, Masks, Dreams, Effigies

## How It All Fits Together
At the end of a few matches of W&amp;W, the player may find that they have been awarded a new card to add to their decks.  When we generate this card, we follow a procedure that looks something like this:

1. Based on a few factors like win streaks and player level, the rarity of the card is determined.
2. The card type (spell, scroll, or rune) is determined.
3. A school of sorcery is selected (Classical, Primal, or Magic).
4. An element and an artifact from that school are selected at random.
5. Now the fun part begins:  Based on the rarity, type, school, element, and artifact, the system adds traits to the card that generate special effects.  For example, maybe the system chooses the following: Uncommon rarity, Spell type, Primal school, Fire element, Dagger artifact.  Based on these factors it selects "armor piercing" (derived from the Dagger) and "burning" (derived from the Fire element) effects. Those traits are applied to the card.
6. Damage output, spell card ports, and energy cost are then generated as balancing factors for the card.
7. Finally, the card is given a name using an algorithm that takes into account all of the aspects of the card.  Our armor piercing, burning dagger card, for example, might get named "Obscure Dagger of Immolation"

I've left out a few things like difficulty, the quality level of the elements and artifacts and a few other odds and ends, but that's the gist of how the system works.

## The End Result
With this system in place, we can deliver [gazillions](http://borderlands.wikia.com/wiki/Borderlands_2) of cards with the potential to generate emergent combinations and gameplay scenarios for which we might not have planned.  Every time we think of a new game mechanic or card trait, we can feed these parameters into the card generator and many new cards will begin to propagate into the game.

## Balance
There is a good chance that game breaking cards will be generated by a system like this, and we're planning ahead to tackle these problems.  Firstly, obvious breaking combinations will be banned from the generation scripts.  Secondly, we have some tentative plans to use machine learning techniques to identify dominant combinations before they ever go live.

Inevitably something might slip through the cracks, a savvy user will discover an exploit, or a single strategy might start to dominate the leaderboards.  However, the game world of Witches &amp; Warlocks will be an ever evolving tapestry of mechanics that we will constantly be polishing.  Seasonal changes will be planned, and game breaking cards will be tweaked.
