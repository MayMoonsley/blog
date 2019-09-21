---
author: mlawver
comments: true
date: 2018-11-10 16:00:26+00:00
layout: post
link: http://blog.maycod.es/2018/11/magical-monkey-adventure-devlog-4-roadmap/
slug: magical-monkey-adventure-devlog-4-roadmap
title: 'Magical Monkey Adventure Devlog #4: Maps'
wordpress_id: 62
categories:
- Devlogs
- Magical Monkey Adventure
- Programming
---

This post is a twofer. First: maps. Second: some code cleanup I did.

<!-- more -->


## Maps


![](/blog/assets/img/map.png)

Maps are working now! Scripts can be executed whenever you walk onto a space (which is where the lovely flavor text is coming from) and whenever you try to enter one. These scripts haven't been set up yet, but that it can be done at all is what matters for now. Since everything's connected to the scripting system, it'll be relatively easy to put all the pieces together -- a node on the map can send you to a battle or refill your health or give you more worms or whatever else the scripting system will be able to do.


## Refactoring


Last week, I mentioned that I'd do some code cleanup, and I did indeed clean up my code. Specifically, I messed with the scripting system, which was basically the ugliest thing in my code. Inside a `for` loop that iterated through the script, there was a giant `switch` statement, with a `case` for each command. In addition to looking ugly, it felt like the wrong way of doing things. Wasn't there a better way?

Yes, there was! Instead of running through the `switch` statement, the functions are now stored in an object. The `for` loop is still there, but it just calls whatever function the command points to, passing in the arguments directly. While I haven't run the numbers to see if it's faster, it's a heck of a lot cleaner and easier to parse, so I feel confident in calling it the better way.

There was one issue with this, though. When the code behind a command was called from inside the `for` loop, it had access to the current index. This was how I did branching code: through `goto`s and `label`s. `goto` just set the index to the first instance of the matching `label` in the script array. This wouldn't work if the code wasn't in the loop, though, barring making the array index global. (Yuck.)

So I did what I should've done a while ago: made a proper `if` command, based on how conditionals work in [Shenzhen I/O](http://www.zachtronics.com/shenzhen-io/). (It might be how things work in actual real-life assembly, too, but I wouldn't know.) The `if` command just sets a variable in the scripting system to be either `true` or `false`. Lines preceded by plus signs only run if the condition is true; the opposite is true for lines preceded by minus signs.

Here's some example code using the old method:

```
jumpif a a==b
log A is not B.
jump end
label a
log A is B.
label end
```

And here's some code with the new method that does the same thing:

```
if a==b
+log A is B.
-log A is not B.
```

I really should've done things this way from the start, but a late improvement is better than continuing to do things the wrong way.

So! That's what I accomplished this week. Next time: a map of a different sort. (And maybe some worms.)
