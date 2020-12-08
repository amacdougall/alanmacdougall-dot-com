---
weight: 1
bookFlatSection: true
title: "Professional Work"
---

# Professional Work

## At GetFeedback

This story is still being written.

## At Paperless Post

From May 2011 to December 2017, I worked on the design tool, the single-page
webapp used to customize cards in [Paperless Post](https://www.paperlesspost.com).
I am proud to have worked with a great team on a complex product that directly
adds value to the lives of our customers.

The second incarnation of that design tool is powered by an embeddable
canvas-based JavaScript module nicknamed "the viewport". This module is at the
core of the Paperless Post card system, and powers the React-based desktop web
site, the Redux-based mobile web site, the iOS-based native apps, and the
nodeJS-based server-side renderer.

### Undo/Redo

I wrote a pretty solid undo/redo system for the old Flash/JavaScript hybrid
version of the design tool. I am still proud of it! Some of my finer technical
work. Required a lot of grit to go through and apply it to every aspect of the
application. [This article](/technical-writing/how-we-added-time-travel-to-paperless-post/),
originally from the company tech blog, describes it in detail.

### And Everything Else

I did a lot of heavy lifting at Paperless Post.

* In my first months, I proposed and led a refactor of the architecture of the
  Flash code behind the card design view.
* In my first year, I proposed and led a refactor of the business logic which
  tied the Flash and JavaScript design components together. The refactored
  business logic still runs in the application today, in an evolved form.
* As we developed the HTML5 canvas-based viewport module, a co-worker and I got
  elbow-deep in the fine details of text editing, since we had to implement all
  text interactions from scratch. Ask me about selection pivot points some time.
* Text editing requires a lot of mouse sequences. For instance, to select one
  word at a time, you double click and drag before releasing the second mouse
  click. JavaScript only gives you `mouseDown`, `mouseMove`, and `mouseUp`.
  I wrote an extensive state machine to cover the full range of mouse input.
* When we brought the viewport to the desktop web, replacing the old Flash/JS
  hybrid application, I proposed the use of React, and designed the
  architecture.
* I wrote a generalized client-side data model class which implemented an
  Observer pattern. The model class supported transactions: we could perform a
  lot of model property changes before emitting a single event describing the
  batch of updates.
* Right before we discontinued the Paper product, I implemented a recipient
  addressing feature. Knowing that the feature might not be used in the long
  run, I designed the data model so that it would have no impact on the rest of
  the database. Although the feature was not especially difficult, I consider
  this item an example of my technical judgment.
* In my last project at Paperless Post, I worked with an _ad hoc_ team to get
  card creation working on mobile browsers. The mobile-web app is pure
  JavaScript, hosted on S3, and it communicates with the server's JSON API. I
  was pleased to have the opportunity to use Redux on the job, and I was
  satisfied to leave the company on a win.

### Statistics

According to git, my 5,376 commits and 494 pull requests add up to 171,332 lines
added, 141,331 lines deleted, across 1,127 unique files in three repositories.

Edit: At the time I left the company, the PR count was over 500. A nice
milestone for a major part of my professional journey.
