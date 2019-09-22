---
author: May Lawver
comments: true
date: 2018-09-29 16:00:02+00:00
layout: post
slug: magical-monkey-adventure-devlog-1-the-basics
title: 'Magical Monkey Adventure Devlog #1: The Basics'
wordpress_id: 21
categories:
- Devlogs
- Magical Monkey Adventure
- Programming
---

I've been reading a fair amount of [Shamus Young content](https://www.shamusyoung.com/twentysidedtale/) lately. (His stuff is up there with [The Digital Antiquarian](https://www.filfre.net/) for me. Amazing comfort food reading.) I particularly enjoy his programming project stuff, and since I've been working on a game, I got to thinking.

I'm a programmer. I write. I could do that. So I am. Welcome to the Magical Monkey Adventure devlog.

<!-- more -->


## What's Magical Monkey Adventure?


Magical Monkey Adventure is an idea I've had for a while. The core concept is [Scorched Earth](https://en.wikipedia.org/wiki/Scorched_Earth_(video_game)) plus a more traditional turn-based RPG. I can't quite remember the genesis of it. (Probably something to do with [Gorilla.BAS](https://en.wikipedia.org/wiki/Gorillas_(video_game)) - I remember considering Gorilla Bash as a title.) The general idea is that you alternate between:




  1. Launching projectiles from a cannon to gather mana from magic ore, and


  2. Spending said mana to attack your enemies.


This is kind of an oversimplification, but it conveys is the basic idea.

I worked on a graphical version of Magical Monkey Adventure for a while, but I left the source code for that at home when I went to college and, more to the point, it was kind of ugly. To alleviate the scourge of programmer art, I'm going to be using [ROT.js](http://ondras.github.io/rot.js/)'s ASCII display feature, so I can save time by not having to work on art. (I don't even have to write my own ASCII display code!)

The stage is set - let's write some code.


## Terrain Generation


So, first things first: terrain generation. This was pretty simple. It generates some rolling hills by adding a bunch of sine waves of random heights and frequencies and places ores using a Voronoi map. (I'll probably come back to this at some point, since this system won't ever generate any cliffs or overhangs and probably won't give us any cool mountains, but it's good enough for now.)

![](/blog/assets/img/landgenUnfilled.png)

I was pretty happy with it, but it kept looking a little dim. I tweaked the colors a bit, but nothing really worked. Then I realized: background colors!

![](/blog/assets/img/landgenFilled.png)

There we go! Much better. At some point I'd like to make sure this is colorblind-friendly, but, again, good enough for now.


## The Cannon and Projectiles


Next: the cannon. There's a proud tradition of player characters being represented with @s, so that's what I'm going to do here. I added a bit of UI to show the angle and strength you're firing with and, for fun, some more things that'll come in handy later - ammo, mana, and worm totals. (We'll get to the significance of worms later.)

![](/blog/assets/img/cannonAndUI.png)

With controls to aim and increase/decrease power added soon after, next came the projectiles. These were also pretty simple. They update themselves every ~20 milliseconds until they leave the screen or hit the ground, at which point they explode. (I was considering using getAnimationFrame() instead of setTimeout(), but that felt a bit like overkill when projectiles aren't going to be on that screen all that often. Besides, callbacks are more fun.) I ran into a bug where projectiles would fall through the UI at the bottom, but I thought it was cute, so I kept it.

We're right about where we want to be. Blowing up terrain gives you mana in the right amount, the cannon fires fine, and you even run out of ammo appropriately...but it still feels weird aiming with only these numbers. Sure, you can get used to it, but it's still not super intuitive. What we need is some kind of aiming reticle.


## Some Kind of Aiming Reticle


Getting the angle the player is aiming in would probably suffice, but that doesn't help with figuring out how much power is needed, nor does it show the effects of gravity. What we need is something that'll show the arc of the projectile. (Sidenote: I should really figure out what the projectiles are, at least so my brother will stop making poop jokes.) Something like what you have in most golf games.

This was a little tricky to implement, since the projectiles do all their own calculations. I'd have to duplicate that code in order to figure out where to draw the little aiming dots...or I could just abstract away the calculations. So now each projectile has a little helper object telling it where it should be each time it updates itself. I just had to give the cannon one of those for drawing the aiming reticle, and...

![](/blog/assets/img/aimingReticle.png)

There we go! It's a little bit ugly, but that can't really be helped. The projectile's path is kind of ugly, so the aiming aid is too. Still, this is pretty solid, and now if I want to add some complications like wind, all I have to do is update the calculator and both the projectiles and the aiming guide will be updated.

So that's the current state of Magical Monkey Adventure. See you next week!
