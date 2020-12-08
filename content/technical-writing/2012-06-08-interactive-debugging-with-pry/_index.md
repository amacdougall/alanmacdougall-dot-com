---
weight: 21
title: "Interactive Debugging with Pry"
---

# Interactive Debugging with Pry

{{< hint info >}}
This article was written in summer 2012, and the topic is not evergreen. I stand
by the core recommendations! Use Pry, use its debugging plugins. But the links
and exact details may have shifted over time.
{{< /hint >}}

I wrote a [post about pry earlier](/technical-writing/2012-03-27-using-vim-slime-with-pry-for-repl-perfection.md),
but at the time, I didn't realize just how much muscle Pry was packing. Install
two simple plugins and one builtin function, and you turn Pry into a stepping
debugger. It can pause at a breakpoint, step through code one line at a time,
and even shuttle up and down the call stack; and since you never lose your Pry
superpowers, you can rummage around your state to your heart's content.

## Time Stop

`binding.pry`. Two simple words with immense power. Speak them aloud, and your
program will pause in place, frozen in time, while the Pry REPL springs up
around it. Here's an example, from [the single most important project on Github](https://github.com/amacdougall/puppy-presenter),
an experimental puppy display grid.

```
# generate HTML from template
template = File.read config["files"]["template"]

binding.pry # ENTER THE MATRIX

engine = Haml::Engine.new template, :format => :html5;
output = engine.render Object.new, :images => images;
```

This scrap of script crams cute puppies into a HAML template, but first,
`binding.pry` freezes reality. Loops stop looping, events stop listening, and
the world halts in an eyeblink. Except for you. You have total freedom: you can
peek at your code and tinker with its innards at will. To resume, `Ctrl-D`; your
code will roar back to life and keep going until it hits another `binding.pry`.

If you run your local Rails instance with `rails server` or `./script/server`,
you can even drop into a Pry session right in your terminal, using the same
`binding.pry` technique. Rails debug spam stops: Pry starts.

## One Step at a Time

The `pry-nav` plugin makes it easy to execute your code one step at a time. When
your code hits the brick wall of `binding.py`, you can walk it forward, line by
line, with `next` -- or if you're not feeling so talkative, [alias it to 'n'](https://github.com/nixme/pry-nav#pry-nav)
in `.pryrc`.

We pause at `binding.pry`; now we can step forward. There will be a little arrow
showing us what line we're on. We can execute any code we want at the command
line; we'll get our output and then skim a little off the top.

`gem install pry-nav` or add it to your `Gemfile` to acquire it; `require
'pry-nav'` to equip it.

## Elevator Action

The `pry-stack_explorer` gem lets you move up and down the call stack. Can't
debug much of anything without that. These two methods make a nice easy target.
They don't do much of anything, so there's nothing to debug, but they're simple,
so debugging is easy. Life should be more like that.

```
require 'pry'
require 'pry-nav'
require 'pry-stack_explorer'

def outer(message, number)
  inner(message)
end

def inner(message)
  local = true
  binding.pry
end

outer("hello", 1000)
```

Plain old Pry lets you look at your locals when `binding.pry` sets its teeth.
Try to look at something outside the current scope, though, and Ruby stonewalls
you, but `pry-stack_explorer` doesn't care much for walls. Just use `up` and
`down` to traverse the stack.

Your program hasn't moved... just your point of view. `show-stack` shows you the
whole call stack, with a little arrow saying _you are here_.

## Further Reading

In [this post by Pry's author](http://banisterfiend.wordpress.com/2012/02/14/the-pry-ecosystem/),
you can read more about `pry-nav`, `pry-stack_explorer`, and other plugins that
might help you out.

