---
author: May Lawver
comments: true
date: 2018-10-06 16:00:00+00:00
layout: post
slug: magical-monkey-adventure-devlog-2-programmer-art
title: 'Magical Monkey Adventure Devlog #2: Programmer Art'
wordpress_id: 34
categories:
- Devlogs
- Magical Monkey Adventure
- Programming
---

Last time, I set up the basics of Magical Monkey Adventure's artillery system. You'd think I'd move on to combat next, but I'm a huge fan of visual flourishes. This week: May creates a new image format and implements a particle system. <!-- more --> In the old fully graphical build, the character portraits were traced photos from Wikimedia Commons. They looked pretty goofy, which fit the aesthetic I was going for. Now that I'm using this ASCII art style, though, I can't fire up Paint and trace photos. My solution: invent an ASCII-based image format, find a way to convert other images to it, and bask in this new cohesive aesthetic. I'm calling this new image format "ACTI", for **A**SCII **C**ontainer for **T**extual **I**mages. (Sidenote: I know there are probably already ASCII art formats out there, since that was a big thing in the BBS/demoscene days, but rolling my own solution is way more fun.) The implementation seemed pretty easy. Take an image, resize it, scrape out all the pixel data, quantize it however I see fit, and boom. ASCII image. Sadly, though, it didn't go that way. Here's my test image. [(Grabbed from Wikipedia, naturally.)](https://commons.wikimedia.org/wiki/File:RGB_24bits_palette_sample_image.jpg)

![](/blog/assets/img/toucanPhoto.jpg)

And here's what my algorithm produced.

![](/blog/assets/img/questionableToucan.png)

This took me longer than I'd like to admit to figure out. I thought it was an issue with brightness at first, and it was only after I noticed JPEG artifacts on another test image that I realized what was up: I wasn't _resizing_ the image, I was _zooming in on the top corner_! This was very wrong and needed to be fixed, so I did the only obvious thing and implemented some Nearest Neighbor scaling. (I then realized as I was talking to a friend about this that I could've used JavaScript's built in Canvas scaling for this. Whatever. Implementing things yourself builds character.) (I also kept calling this bird a toucan when it's clearly a parrot, which isn't the smartest thing I've done.) So, now I can not only create blocky images like this one:

![](/blog/assets/img/blockyToucan.png)

I can also generate more textual ones like this, based off the venerable [10print](https://10print.org/):

![](/blog/assets/img/tenPrintToucan.png)

I'm going to keep playing around converting regular images to ACTI, but this is a pretty good start. I also set up a way to export ACTI images to actual .acti files, which involved a lot of messing around with Uint8Arrays and dumb errors, but I finally got the export/import system working. I also started work on a viewer/editor, but it's not going to be in any state worth showing off for a bit, especially if I actually wanted to give the game particles this week instead of just fussing around with file formats. My dream of having a large suite of custom file formats for this project is beginning to feel a little misguided. (What use is it optimizing these sorts of things [when nobody else seems to be?](http://idlewords.com/talks/website_obesity.htm))

So! Particles. Each particle is part of a system, to minimize setTimeout() calls to update the animations. I'm not sure if that's _that_ big of a deal, but it felt important. Whenever a projectile hits the ground, it creates a particle everywhere the explosion hit and flings them away from the point of impact. It looks pretty alright so far.

![](/blog/assets/img/projectileRain.gif)

An issue: the particles seem to flicker whenever the terrain redraws itself. It's not that bad as far as graphical issues go, so I might just leave it, but if I want my game about magic monkeys to be super polished, I'll be sure to try and fix this. I might also try to pretty up the explosions a bit, too, but right now that feels excessive.

Next week, I'm starting on combat. Special thanks to my good friend [acti](https://twitter.com/slackfluffy) for becoming a file format.
