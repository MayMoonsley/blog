---
author: May Lawver
date: 2020-03-09
layout: post
title: Random Poetry With RiTa
---

Last semester I wrote a Node program to randomly generate sonnets.

## What's a Sonnet?

A [sonnet](https://en.wikipedia.org/wiki/Sonnet) is a kind of poem written in iambic pentameter with one of a couple different rhyme schemes. Iambic pentameter is a fancy way of saying "ten syllables with alternating stress." The pattern is unstressed, stressed, repeat. An example of this is the line "But, soft! what light through yonder window breaks?" from _Romeo and Juliet_.

I'll be using `0` to represent unstressed syllables and `1` to represent stressed syllables. For example, *puppetry* has stress pattern `1/0/0`.

## How Can We Generate One?

In order to randomly generate sonnets, I used two libraries: 

* [RiTa](https://rednoise.org/rita/index.php) has a bunch of really useful language utilities. The most useful one for this is the ability to get a word's stresses.

* [word-list](https://www.npmjs.com/package/word-list) provides almost 275,000 words to work with&mdash;more than enough grist for the poetry mill.

I was introduced to RiTa by [Darius Kazemi's blog post about procedural poetry](https://tinysubversions.com/notes/sorting-bot/). As soon as I heard about it, I wanted to try something like this. He was using RiTa to take a predefined template and fill in the blanks with words that fit the rhyme scheme and meter; I'm using it to generate sonnets from scratch.

The algorithm I went with at first is pretty simple:

1. Start with the stress pattern of a full line: `0/1/0/1/0/1/0/1/0/1`

2. Generate a list of all the words that fit at the beginning of the line: all `0/1` words, all `0/1/0` words, all `0/1/0/1` words, etc. (Note that there are no `0` words, since all one syllable words are `1` according to RiTa. This isn't really accurate&mdash;the stress wouldn't fall on every word in an entirely monosyllabic sentence&mdash;but it's accurate enough that it's not a huge problem.)

3. Select a random word from this list and add it to the line.

4. Remove the word's stresses from the start of the current stress pattern.

5. Repeat with the remaining stresses until the line is filled.

6. Repeat fourteen times. Now we have an unrhyming sonnet!

Of course, since our sonnet doesn't rhyme, it's not really a sonnet. It has the same rhythm as one, though, so it's a pretty good start. Once I got the algorithm working, this was its first output:

> Confuter luminaries by cumshaw
> 
> Maligners slither colloids delighters chabouks
>
> Deducements soops ceiled galabiyas pheezed
>
> Reflecting overdeviates satelles
>
> Elastications wizes quersprung pejorate
>
> Undried repriming windless toxin drole
>
> Unbraste acquitted manitou contends
>
> Preexisted flyblow metrication chawed
>
> Desertizations gutta bodysurf
>
> Commuters supper worn empassioned sines
>
> Reformulation matte refortify
>
> Attractant murrain depletion shrivers twist
>
> Machining forenights delayer obtrude
>
> Tequilas several fell unbrushed resail

Most of this is complete nonsense, which is to be expected. Still, one line stood out to me: _Undried repriming windless toxin drole._ It evokes such strong imagery; when I read it I imagine a stagnant pool of water surrounded by sickly-sweet vapor. However, there are a lot of really obscure words throughout the poem. You basically have to have the dictionary open in another tab if you want to understand it, which is less than ideal.

## How Can We Simplify Our Lexicon?

I thought of a couple things to do to solve this problem, but what I decided on was pulling words from some given source text instead of from the entire dictionary. This keeps the poems relatively constrained in terms of vocabulary. [Project Gutenberg](https://www.gutenberg.org/) makes it easy to find large source texts; with some minor cleaning, _Pride and Prejudice_ was ready to process. Getting all the words out of the text was fairly simple, though I had to make sure not to include stuff like numbers.

This is the first sonnet generated from the _Pride and Prejudice_ lexicon:

> Surmises lost describe admire resolve
>
> Proportioned whist amazed confinement fare
>
> Delicious laugh propose appear apace
>
> Regained exact transition speed incur
>
> Instead unfelt asserting high comprise
>
> Obliging authorising talent noon
>
> Resisting throwing here disliked correct
>
> W sorry french dispute command therein
>
> Attorney tanned respecting did redress
>
> Repeated notions no convinced advice
>
> Formation rail disarm required made
>
> Indulgent parents deciding sized scenes bowl
>
> Performs malicious bell adept subjects
>
> Hotel aloud subsist imply about
>
> Acknowledge carried glories did surmount

I don't know how the W snuck in there. (In case you'd like to read this poem aloud, note that RiTa thinks it should be pronounced "wuh", not "double-you.") This is still about as comprehensible as the first poem, though it feels like the second one has more interesting phrases in it. (I'm particularly fond of "asserting high compromise" and "carried glories did surmount.")

Still, this isn't really coherent. We can read meaning into some lines through a sort of linguistic [pareidolia](https://en.wikipedia.org/wiki/Pareidolia), but a lot of the time that's pretty difficult. This algorithm could theoretically produce something stunning (remember that line about toxins?) but it also produces a lot of junk. [This excellent article](http://www.possibilityspace.org/tutorial-generative-possibility-space/index.html) talks more about this idea; basically, forcing the poetry generator to be more coherent reduces the number of poems it could possibly produce. This is the point of it, of course. With that in mind...

## How Can We Make This Poetry More Coherent?

This is a difficult question to answer since there are so many ways to answer it. Markov chains? Neural networks? Just leaving the generator as is and calling the incoherence a feature? These are all valid approaches, but the one I chose here was simple:

1. Find all the fragments of text in some source that fit the meter.

2. Create a sonnet by pulling from these lines at random.

Initially I was worried that there wouldn't be enough suitable fragments to make a sonnet. However, this wasn't the case. My program found 150 such fragments in _Pride and Prejudice_ and 589 in _War and Peace_. (The algorithm initially found more, but I wasn't sure it was handling letters with accents correctly, so I removed all the lines containing them.) Getting lines from multiple sources allows us to do something pretty nifty. Sonnets typically have what's called a [volta](https://en.wikipedia.org/wiki/Volta_(literature)) or turn. The idea is that the sonnet changes perspective or tone part of the way through. Again, Wikipedia explains it better than I can. My hypothesis is that we can emulate this by switching from one source to another in the middle of a sonnet. The sonnet as a whole still won't be very coherent, but at least the lines should mostly make sense on their own.

Here's the result of this experiment:

> The windows of the houses and the streets
>
> Rouged faces dressed in glaring colors who
>
> Dispatching of adjutants was confined
>
> Expression of the princess pretty face
>
> The thoughtful profile of her drooping face
>
> Addressing princess mary for the first
>
> Announce the peace negotiations and
>
> Forget yourself again before her as
>
> In his patroness lady catherine
>
> The real the worst objections to the match
>
> Were fastened on the parcels placed within
>
> Unguarded and imprudent manner nay
>
> The chaise arrived the trunks were fastened on
>
> Ashamed especially when contrasted with

The individual lines here make more sense than previous poems for obvious reasons. In my opinion, this makes the work as a whole worse, almost as if it's fallen into a poetic [uncanny valley](https://en.wikipedia.org/wiki/Uncanny_valley). It's harder to see meaning in the connections between random lines than it is to see meaning in a single nonsensical line, at least for me. There's almost a link between the first two lines if you imagine them setting the scene, but the rest of the sonnet doesn't really fit together. The turn also isn't super obvious. Still, weird techniques like this are useful to play around with even if they don't end up working well&mdash;you never know when they could come in handy.

## What Next?

My first goal with these experiments was to have fun, and I'm proud to say that I accomplished that. They were also pretty good practice for [NaNoGenMo](https://nanogenmo.github.io/). (NaNoGenMo is still eight months out as I write this, but it's good to be prepared.) With that in mind, an obvious next step is to make the sonnets rhyme. I have a lot of other ideas for where to go from here, too. Expect more generative poetry in the future. If nothing else, these techniques produce excellent idea fodder, and that's a good enough reason to continue with them.