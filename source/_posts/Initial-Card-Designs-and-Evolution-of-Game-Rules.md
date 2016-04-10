title: Initial Card Designs and Evolution of Game Rules
tags:
  - design
  - cards
  - rules
category: Game Design
author: Chris Saylor
thumbnail: /img/card-design-spell-v1.png
date: 2015-07-13 10:45:33
---


Over lunch, Steve and I had a _design sprint_ where the main topic was to discuss our different ideas about how the cards in _Witches and Warlocks_ were to be laid out and function.

<!-- more -->

## Functionality and Flavor

The card itself needs to convey important information to the player about what the card can do and how to play it, but we also wanted to leave some space to allow for flavor text and art, otherwise it would be boring!

Our design decisions will always favor functionality first, so with that in mind, priority of card information flows from top-to-bottom. Name, dice count, and effects are at the top, while art and flavor text would be at the bottom.

![](/img/card-design-spell-v1.png)

> This is a sample of a spell card, name, effects, and cost at the top.

## Card Ports

Up until this point in order to construct and play a spell, the player could match by element or artifact types up to a maximum of three spell cards together. One of the first concepts we discussed was the idea of cards having _ports_. This idea was to add a positional element to how the player can join spells together. Now the matching of spell cards is based on the port types positioned on the sides of the card. Since we can control card linkage at the card level, this allowed us to remove the arbitrary three card limit of the spell.

![](/img/card-ports.png)

> An example card with _ports_. The left side would accept a _lightning_ type spell, the right side would take a _holy_ type spell.

## Dynamic Card Content

With the game being digital, we have the luxury of being able to hide, expand, and otherwise modify the card on the fly. Certain parts of the card are only relevant at certain times. For example, the _energy_ cost is important when obtaining a card from the _Grimoire_ or _Library_, but are not useful when they are in your hand. This means our card design needs to be modular to be able to display different information depending on what the player is currently doing.

It also has to be modular in order to support different types of cards. Runes have different display requirements than spells, which have different requirements than energy cards. We'll cover all the types of cards available in _Witches and Warlocks_ in a future post.

---

We are still progressing on designs of the card and how it will fit into the game (and store), but are very happy with how it is shaping up. We're very excited about some of the design choices having a big impact on the game dynamics and fun!
