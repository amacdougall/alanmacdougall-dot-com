---
weight: 3
bookFlatSection: true
title: "Side Projects"
---

# Side Projects

I write code in my spare time for the fun of it, and I have plenty of other
hobbies, so most of these projects are incomplete. Check out the source code,
though; I have put a lot of time into these babies over the years.

## Mazes

[Github link](https://github.com/amacdougall/mazes)

I spent a while working through [Mazes for Programmers](https://pragprog.com/book/jbmaze/mazes-for-programmers),
by Jamis Buck. The book is written in Ruby, but I reimplemented many of the
algorithms in Clojure. In Ruby, you get to implement the algorithms more or less
the same way you might do them in C++, but Clojure requires a very different
style. This has been a pleasant challenge.

In particular, check out the maze generation and pathfinding algorithms in [this folder](https://github.com/amacdougall/mazes/tree/master/src/cljc/mazes).
There is also a React-based (via [re-frame](https://github.com/Day8/re-frame))
front end application which demonstrates some of the algorithms.

## Alpha Counter

[Github link](https://github.com/amacdougall/alpha-counter)

A life counter for the [Yomi card game](http://www.sirlin.net/yomi/). Try it out
at [yomicounter.com](http://yomicounter.com/): choose two characters, hit "Start
Game", then hit a few of the damage buttons and see what happens. Tap a life bar
to change the target. The timing behind the combo damage accumulation and
application is all done with [core.async](https://github.com/clojure/core.async),
Clojure's equivalent of Go's channel abstraction.

I wrote this because I didn't like adding up damage mentally and tracking it on
paper while playing the game. A great example of solving real-life problems with
programming.

## Raise Your Game

[Github link](https://github.com/amacdougall/raiseyourgame)

Although this project was intended to become a Youtube annotation site, I got
deeply sidetracked into Clojure database and testing code. Check out the
[server-side code](https://github.com/amacdougall/raiseyourgame/tree/master/src/raiseyourgame)
and its [tests](https://github.com/amacdougall/raiseyourgame/tree/master/test/raiseyourgame/test)
for a good example of hand-rolled Clojure account management.

If I had to do it over again, to be honest, I would just use Rails with some
plugins. The client side was going to be the really interesting part of this
project! However, I will say that there is something really satisfying about
slowly building up a framework of well-tested code.

## Wrongtangular!

[Github link](https://github.com/amacdougall/wrongtangular)

At Paperless Post, at one point, we needed to populate the `rectangular?`
attribute for thousands of paper graphics. We could not simply compare the width
and height of the graphics, because transparent and nearly-transparent pixels
greatly complicated the issue. Ultimately, we realized that a human would need
to evaluate each image.

To evaluate each image at a glance, instead of laboriously clicking through our
internal CMS, I created `Wrongtangular!`, a ClojureScript application which would
display image after image to its user. With fingers on the home row, the user
hits any right-side key to approve the image, and any left-side key to reject
it. Kind of like Tinder, but for whether graphics are rectangular.

Later, I realized that it might be useful to have such an application for any
arbitrary dataset and any boolean attribute. Hence `Wrongtangulizer`, a gem
meant to be used in the Paperless Post Rails REPL to generate instances of
`Wrongtangular!`.

## underscore.as

[Github link](https://github.com/amacdougall/underscore.as)

A simple port of most of underscore.js 1.3. This was extremely handy back when
ActionScript was relevant, and I am still proud of a few of the hacks I used to
get the concepts working in AS3's less flexible runtime.

## Everything Else on Github

There are a lot of barely-started projects, sample code, and so on, but none of
it is really worth reading.
