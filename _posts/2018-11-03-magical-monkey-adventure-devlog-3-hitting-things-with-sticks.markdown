---
author: May Lawver
comments: true
date: 2018-11-03 16:00:34+00:00
layout: post
link: http://blog.maycod.es/2018/11/magical-monkey-adventure-devlog-3-hitting-things-with-sticks/
slug: magical-monkey-adventure-devlog-3-hitting-things-with-sticks
title: 'Magical Monkey Adventure Devlog #3: Hitting Things With Sticks'
wordpress_id: 48
categories:
- Devlogs
- Magical Monkey Adventure
- Programming
---

Right now, I only have half of the combat equation in place. Let's fix that. Three weeks late and (ideally) worth the wait, it's time to implement turn based combat! This was kind of tricky, but I've now got a functioning combat system. Let's go into how.

<!-- more -->

The first thing I did was set up the UI to get a feel for the layout. It morphed over time, but this four-square design stayed pretty constant.

![](/blog/assets/img/combatContinue.png)

(The "In Your Corner" and "In Their Corner" are relics from when it was going to be possible to fight multiple enemies at once, and it's probably going to be changed.)

Next came a scripting system to hook combat stuff into. Luckily, I had one handy for another project. All it took was some copying, pasting, and variable renaming to get it working. (This scripting system is also going to help once I move on to story sections and map movement, but let's not get ahead of ourselves.) After adding commands to deal damage and other such RPG fixtures, next came the little choice menu, which also hooked into the scripting system. (It's scripting all the way down.)

It's all fairly standard, I'd imagine. Advancing through the turn list, assigning each skill to a choice, etc. Each skill has a special version of its script that's only executed when a player character uses it; this version includes code to remove all the mana and worms that the skill costs.

![](/blog/assets/img/combat.png)

Something worth noting is how I decided to handle the enemy turn. This isn't a particularly elegant solution, and I might revise it later, but right now the game just gives you the option to continue. That triggers the script associated with the enemy's attack. There's no real AI at this point. The enemy chooses a random skill and targets a random fighter - it doesn't even check to see if the fighter is knocked out or not. This is going to need some work, but the whole point is to have a system that can be built upon.

Handling winning and losing were also a bit of a cheat. Internally, there are skills that have scripts to handle these things. If the enemy's defeated, the game gives you a choice with the victory skill attached; the inverse happens if your party is defeated. Since it's based on the scripting system, I can outsource this to future May.

This is all the combat system is right now. The code feels kind of messy and inelegant, but it works. Refactoring can come later. Next week: maps! (And a bunch of code cleanup I'm not going to write about.)
