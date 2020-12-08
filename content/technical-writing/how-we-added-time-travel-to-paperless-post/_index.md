---
weight: 1
bookFlatSection: true
title: "How We Added Time Travel to Paperless Post"
---

# How We Added Time Travel to Paperless Post

{{< hint info >}}
This was originally a post on the Paperless Post dev blog.
{{< /hint >}}

Our designers make our cards beautiful, but our users make them unique. To help
them, we’ve built a card editing tool that allows extensive customization. And
customize they do: over and over, we’ve seen our users stretch the limits of the
design system. In fact, we’ve seen them stretch it, bend it, twist it, crush it,
and sometimes even break it—as our customer support team can attest. They
experiment, they tweak, they twiddle, and they turn out cards, from the artistic
to the bizarre, that we as designers and programmers could never have
anticipated. This is a major strength of our site, even when our customers’
creativity fills up the bug tracker, and so editing features have always been a
priority. In the past few months, after withstanding a huge holiday rush, we had
breathing room to tackle a major new editing feature: a fully reversible undo
history.

## The History of History

We have not been sleeping for the last three years, of course. Undo history has
been in the app, off and on, in various forms, for a long time. Until recently,
text input could be undone with Ctrl-Z, although it wasn’t redoable. We switched
from per-character undo to batched input undo over the summer, with a little
help from the `_.debounce` feature of underscore.as.

Meanwhile, on the JavaScript side of the application, we had a state-based undo
system, which handled every other kind of user action. Two undo systems—odd, but
emblematic of one of the key design decisions behind the card creation app.

## The Hybrid

Early in the summer of 2011, we switched over from our original pure-Flash card
creation app to a new approach we call “the hybrid”: a JavaScript UI wrapping a
Flash viewport. Visually, JavaScript handles all the HTML controls outside the
viewport, while the Flash viewport renders the card and handles UI interaction
that happens within it, such as dragging a photo. More importantly, our
JavaScript code can be said to drive the app: it has modules which interact with
the server to maintain a canonical card state, and it relays that state to Flash
as needed. In turn, it interprets events from Flash and updates the server when
necessary. Flash and JavaScript sync constantly, since local communication is
cheap; but JavaScript hits the server only when necessary.

Our goal for this separation was to permit advanced Flash rendering techniques
such as fast photo filters and high-quality fonts, while doing most UI elements
in HTML. JavaScript, HAML, and Sass make UI updates a snap. What’s more, we can
easily replace the Flash viewport with a pure HTML5 solution where appropriate…
but that’s another blog post.

## State Snapshots

When implementing an undo history, having two codebases on two runtimes makes
life harder. Crucial state is split across runtimes. Even though JavaScript is
the canonical source of model data on the client side, plenty of other state is
segregated, including things you might not think of as “state”: object
references, event handlers, the starting values of in-progress slider
interactions.

When we first tried to implement an undo history, we assumed these issues could
be brushed aside. We need to restore object references and event handlers upon
undoing a delete? Recreate them! We need to get a delta (a single change amount)
from a slider drag (dozens and dozens of little events)? Assign the drag a
transaction id! JavaScript should be the sole source of all app data, not just
raw model information, and so we should be able to revert to any previous state
by loading a snapshot we’ve stored as a JavaScript string.

This may sound cumbersome, but it worked pretty well in practice. Many actions
could be undone just by piping model updates from JavaScript to Flash. But
although this system showed promise, and its outstanding bugs could have been
resolved, we knew we wanted a much more incremental and flexible approach.
Instead of serializing the entire application state on every user action, and
rewriting the entire application state on undo, why not store only the relevant
delta? This would use fewer resources and isolate the effects of each undo step.

## Design Patterns to the Rescue

After some internal discussion, we settled on the classic undo implementation:
the Command pattern. I first used the Command pattern in Java land, and although
I respected its power, I dreaded the tedium of defining customized command
classes. In the traditional Command implementation, there might be one for every
possible user action, each containing copies of the state necessary to reverse
and repeat the action. As I thought about it, however, I realized that
JavaScript and its more straitlaced cousin ActionScript 3 were well-suited to
the pattern—considerably more so than Java and C++, where the pattern
accumulated its original mindshare.

## Commands for Controls

There are a few immediate motivations for the pattern in UI programming. One is
to attach a single procedure to multiple UI elements—a menu item, a toolbar
button, and a keyboard shortcut, for example—with more flexibility than event
handlers, such as runtime remapping. In a language with first-class functions,
such as JavaScript, this handles itself; so do many other behavioral patterns,
such as Strategy… up to a point.

Another is to modify a button’s action bit by bit in response to other factors.
Add a Command to a form’s submit button, and alter the command state as the user
plays with the form. Sure, the button handler could just read the form, but if
you want to divorce command execution from the form code entirely, this is a
good architectural choice.

But the overriding reason to use the Command pattern is to implement undo and
redo.

## Alternatives to Commands: A Chilling Dystopian Hellscape

Without the Command pattern—without a way to visualize actions as storable
objects—only two sensible approaches remain. You can save coded representations
of the actions to be performed and revoked, such as strings to be interpreted
later; or you can save snapshots of the entire state of your app.

Both approaches can be effective. An array of simple action codes such as
`“element 123abc color = #ff0000”`, or more computer-readable data objects such as
`{targetID: "123abc", action: "move", x: 120, y: 0}`, will work for a long time,
especially if you write a simple interpreter for such commands. But once you
need to undelete a complex object, your undelete command must suddenly contain
all the commands necessary to rebuild the object from scratch; or you begin to
cache deleted objects for potential resurrection, and must perforce remember to
purge them when discarding a history branch.

Keeping a snapshot of the whole state is simple enough, as long as your model is
amenable to serialization in the first place. But unless you’re restoring a
whole VM state, or rebuilding a purely noninteractive document, it becomes a
real task to ensure functionality in all situations. You need a strong canonical
source of state data—which we had, in the JavaScript session state. The snapshot
approach had legs. But it lacked the one thing we really wanted: isolation. A
history event involving text should be replayable even if an unrelated photo has
been changed elsewhere; but with a snapshot approach, this was impossible.

## The Simplest Command

The classic approach, we decided, was best. A Command, in this pattern, is just
that: an object representing a command to be executed—or reversed—by the
application. Commands are not UI actions: a click is not a Command, but a click
can trigger a command. Commands are not controller or model methods:
`record.delete()` is not a Command, but a Command can invoke that method. At
minimum, a Command is an object which has a single execute method. For our
purposes, it must also have undo. This is especially simple in JavaScript, where
we don’t even need to make a class:

```
var command = {
  execute: function() {
    releaseTheHounds();
  },

  undo: function() {
    deployDogCatcher();
  }
};
```

Making Dumb Commands Smarter

“It’s like undo is smarter than do” —overheard on Campfire

Of course, in this example, we assume that `releaseTheHounds` and `deployDogCatcher`
are global functions that take no parameters. In any real application, we will
have commands which alter the program state. Their undo implementation must know
what state to revert to. In Java, it was common to write subclasses for each
type of command, like this:

```
public class ColorChangeCommand extends Command {

  private Potato potato;
  private int oldColor;
  private int newColor;

  public ColorChangeCommand(Potato potato, int newColor) {
    this.potato = potato;
    this.oldColor = potato.getColor();
    this.newColor = newColor;
  }

  public void execute() {
    potato.setColor(newColor);
  }

  public void undo() {
    potato.setColor(oldColor);
  }
}
```

```
public void handleColorClick(ClickEvent event) {
  Command command = new ColorChangeCommand(
    PP.getApplication().getCurrentPotato(),
    event.target.getColor());
  command.execute();
}
```

This subclass is simple, but as you start doing more complicated things,
subclasses proliferate, and things get out of hand in a hurry. There are tricks
to manage that complexity, but once you start using extra patterns or even
full-fledged frameworks, well, “now you have two problems”.

JavaScript and ActionScript 3 sidestep the whole issue. Functions in those
languages are closures, which means that they retain references to everything
that was in scope when they were defined. All you need is a simple hash to hold
a couple of closures, and you’re done. Here’s the above example, as we might
write it for Paperless Post.

```
function handleColorClick(event) {
  var potato = PP.Current.potato;
  var newColor = $(this).attr("data-color");
  var oldColor = potato.color;

  var command = {
    execute: function() {
      potato.color = newColor;
    },
    undo: function() {
      potato.color = oldColor;
    }
  };

  command.execute();
}
```

In this case, all the information we need is defined within the scope of
`handleColorClick`; and since that scope is available to the nested functions
execute and undo, it remains available to those functions whenever they happen
to run. Already you can see a gain in simplicity. As long as you have a good
grasp of function scope and closure mechanics, which should be natural for any
JavaScript programmer these days, the simplicity stays even when you’re storing
complex objects in those closures.

## Undo, Redo, Unredo, Reunredo, and Unreunredo

JavaScript and ActionScript make it easy to create a command object along with
all its state. The history of the app state—assuming it is entirely user-driven,
or that a user-driven component can easily be separated—can be expressed simply
as a linked chain of commands. The obvious and correct approach is to embody
history as an array, which is used as a stack: push an executed command, pop a
command in order to undo it. Push undone commands onto a redo stack; pop those
commands one by one to replay history, pushing each redone command onto the undo
stack.

To put it another way, as we travel forward through time, we leave behind us a
series of Commands: crystals of solidified intent. When we hit Undo, we take a
step back, pick up that crystal, give it a solid rap with our magic wand, and
time goes backward. Then we set it back down in front of us, on the redo stack.
If we go forward again, we tap the crystal and it repeats our undone action for
us, making time whole again. But if we do something else, instead, every crystal
ahead of us—our redo stack—disappears in the proverbial puff of logic.

## Cross-Runtime Issues

This stack-based approach is obvious and correct… until you start dealing with
two codebases on two runtimes. That’s when things get tricky. A lot of
interactions involve JavaScript simply sending model updates to the Flash
viewport, which duly renders them; but when an action originates within Flash,
such as a photo drag or text input, JavaScript simply doesn’t have enough
information. Text input must be stored as commands holding sophisticated state
such as text patches and formatting object fragments. Sending that kind of data
to JavaScript would add complexity we don’t need, since we’d need code on the
JavaScript side to incorporate fragmentary data into the model. Why implement
text patching in JavaScript when the text is only visible in Flash?

My second thought was to give each runtime its own history, with special
commands used to hand over control of the undo and redo buttons—literally
swapping event handlers, actually, depending on which runtime was “up” to undo
the next command. I was thinking of coroutines: the entire command history could
be visualized as cooperative multitasking. This quickly led to the approach I
actually used.

I didn’t write code for bad approaches, just notes. Never underestimate the
debugging power of a notepad!

## Main and Subordinate Command Stores

JavaScript was already the canonical source of model information on the client
side, the anointed speaker to the database. It made perfect sense to canonize
its command store as well, making it the sole keeper of application history. At
the same time, we wanted to handle Flash commands within Flash: a photo resize,
for instance, involves factors JavaScript should not be aware of, such as the
boundary contraints which keep an image in its frame during resizing. User
actions performed entirely within Flash should be undone by Flash, even though
JavaScript knows the score and calls the tune.

The solution was simple: keep two command stores. On the JavaScript side, the
twin undo and redo stacks; on the Flash side, a hash of commands. Commands
originating in Flash now do this:

1. User performs action.
1. Flash creates a command which can undo/redo the action.
1. Flash calls `historyUpdate` on JavaScript, passing the command as an argument.
1. JavaScript creates a command which calls `undo(uuid)` and `redo(uuid)` on Flash.
1. JavaScript assigns a new uuid to that command.
1. JavaScript adds the command to its undo stack.
1. JavaScript returns the uuid to Flash; this all took place during execution of `historyUpdate`.
1. Flash sets the uuid on its own command object.
1. Flash stores that command in its own command hash, keyed by uuid.

In effect, JavaScript maintains the entire command history, but some of the
commands delegate directly to commands on the Flash side. Flash stores the
commands as an unordered hash, relying upon JavaScript to maintain the sequence.

## Breaking with the Past

Sometimes we need to “break” history: destroy some of our stored commands and
start over. Any time the user performs a new action, for instance, our redo
stack must be cast into the void. Your application might have actions that
cannot be undone; when the user opens that door, her history shatters. Depending
on your application’s demands, executing a history break can be complex: you
need to purge those commands, make them all available for garbage collection,
and along with them, any object references and event listeners that might still
hold some obscure yet tenacious tie to the outer world. Orphaned event listeners
are like samurai ghosts, locked in this world past their time, forever waiting
to fulfill an obligation that does not come.

And sometimes, adding history will break your app. If you design with the naive
assumption that time travels in only one direction, you will paint yourself into
a corner. Adding undo history to our application was not just a matter of
wrapping UI handlers in command generators. Every one-way state transition had
to become reversible; every act of creation had to contain the blueprint for its
own destruction. But by the same token, no object could be deleted without
provision for its recovery. In the end, we touched nearly every aspect of the
application to implement an undo history, and turned up dozens of new and
interesting bugs in the process. If you’re writing a new app that might benefit
from undo/redo, I strongly advise building it in at the start.

## The Future of History

Undo/redo has gone live, but development is not over. Undo history is an ongoing
commitment: every new feature has to tie into the system, and every change to
the application can alter the resource usage of the command stacks. But it will
be worth it. Undo history is hard to get right, but it’s a huge usability
improvement our card senders will notice and appreciate.

## Epilogue: Where Are They Now?

This article was written in 2012; this epilogue was written five years later. I
am still proud of the system I created, and I stand by every word of this
article. However, the resulting system still had many edge cases, and bugs that
were difficult to reproduce, mainly because undo/redo were added to the system
long after its creation.

In the new design tool, time contraints prevented us from adding a full-featured
undo/redo system. This is unfortunate, because undo should is best when added at
the very foundation of a system. Even so, it was my idea to omit it, because our
immediate business goal of producing a working iPad application was more
important than the long-term UX goal of having a full-featured undo system.

I am excited about the possibility of using [event sourcing](https://msdn.microsoft.com/en-us/library/dn589792.aspx)
to implement undo and redo in complex single-page applications built using
event-driven immutable-data systems such as [re-frame](https://github.com/Day8/re-frame)
and [Redux](http://redux.js.org/).
