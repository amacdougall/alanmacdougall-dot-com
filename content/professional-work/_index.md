---
weight: 1
bookFlatSection: true
title: "Professional Work"
---

# Professional Work

## At Narvar

As manager of the team responsible for Narvar Returns & Exchanges for Shopify, I
lead a team of five developers, and we're still hiring. I work with a product
manager and a designer to plan the future of the application. Some highlights:

* I reached across teams to rework our ticket prioritization and scheduling
  system. Previously, the team manager took direct responsibility for pulling
  priority issues out of a chaotic and disorganized backlog. I brought
  customer-facing teams into the process and gave them a leading role in
  deciding ticket priorities. This involved negotiations among six different
  managers of teams with different constituencies and priorities, but ultimately
  increased time to resolution by 50% and reduced status update requests by 90%.
* The mid-market Shopify Returns product has always been a ground for
  experimentation before features are implemented in the enterprise product.
  Repeatedly, we took high-priority CEO-driven projects with externally imposed
  deadlines, and landed those projects precisely on time.
* I act as the front line to assess and diagnose incoming tickets.
* An engineering manager should know the codebase. I make sure to spend time
  delivering bugfixes and minor features every week.
* I work across time zones and continents to manage a distributed team.

## At SurveyMonkey

As manager of the team responsible for third-party integrations in GetFeedback,
I led a team of five developers. I worked with a product manager and a designer
to plan the future of the application. During this time:

* We created a new Salesforce integration microservice, with an experimental
  frontend. It ran within a Kafka-based microservice architecture that hosted an
  increasing number of services, as part of an initiative to unify GetFeedback
  and another recent acquisition, Usabilla.
* We built a contact database for survey response attribution, with an
  innovative client-side spreadsheet import system. This system was designed as
  the centerpiece of a customer-experience management platform which would
  encompass GetFeedback and third-party CRMs.
* When Zendesk and SurveyMonkey announced a planned merger, we played a crucial
  part in a crash effort to build a Zendesk integration. The merger fell
  through, but the Zendesk integration is now available to all customers.
* Through it all, we continued to maintain and improve our existing Salesforce
  integration.

There were several changes of direction in this period, and so my management
style emphasized stability, medium-term goals, and a clear business rationale
for each effort. My team had excellent retention despite general attrition, and
feedback from my managers and team members emphasized my calm and positive
attitude. (Full disclosure: I was sweating bullets half the time.)

## At GetFeedback

As a full-stack developer on a small team, I had responsibility for every aspect
of the GetFeedback application. Using Rails and JavaScript, I did countless
features and bugfixes. This system was based on Ember.js, which was a fun trip
outside the React box.

* I made many improvements and refinements to the Salesforce integration at the
  heart of our business.
* I implemented a print-friendly view of one of our main analytics reports, a
  surprisingly complex task that got deep into browser-level rendering, and even
  saw me analyzing the Chromium source code to understand the graphics pipeline.
* I wrote a quick and dirty tool to suggest possible fields for univariate
  segmentation, filtering out fields whose datasets were too noisy, too sparse,
  too this, too that.
* I implemented import and export of spreadsheets for multilanguage
  translations. This was a fun job that got me deep into the various specs for
  language naming, string encoding, and spreadsheet parsing. I ended up with a
  lot of respect for UTF-8.
* With a co-worker, I created the Actions feature, which permits customers to
  easily generate complex workflows for established use cases. There is now an
  entire team working on a similar concept.
* I played a large part in integrating Twilio with our existing distribution
  methods, permitting SMS survey distribution. Twilio is great! I understand now
  why they had billboards all over SF just saying "ask your developer."
* I did a lot of backend work on our billing and subscription systems. Tax
  calculation is hard, and even filling in the gaps in a third-party API was a
  real task.
* With co-workers, I connected one of our main analytics views to a
  live-updating object database instead of Postgres, greatly improving its
  responsiveness and filtering abilities.
* And amid all this, I fixed countless bugs and did many small features.

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
