---
layout: post
title:  "Relatively Uninformed Reflections on Two Scripting Languages"
date:   2016-07-03 12:00:00 -0500
---

Things I miss about Python when using Ruby:

* No cases where there are numerous different ways to do the exact same thing. (In some cases I think this is fine and even good, but in others it serves no purpose and is confusing.)
* Named arguments. You can basically approximate this by using an options hash, but it’s clunky.
* The science/math/data analysis/visualization libraries (which are of course the main reason to use Python).
* First-class functions. True, Ruby has procs/lambdas, but in Python all functions work this way.
* Semantic indentation. If you’re going to indent anyway, using `do` and `end` is just clutter.
* Semantic versioning. I realize Ruby uses this *now*, but it’s not at all obvious that the major backward-incompatible update was from 1.8 to 1.9.

Things I miss about Ruby when using Python:

* Capitalization conventions that are universally followed and make sense.
* Related to the above: modules/classes are immediately distinguishable from variables, and variables with different scopes are immediately distinguishable from each other.
* Constants.
* Symbols rather than words for logical operators (`||`, `&&`, `!`). They look different from the words surrounding them, so you can visually parse the line at a glance.
* Enumerators! As I understand it, Python for loops are just syntactic sugar for the `__iter__` method anyway, so why not make it explicit?
* Consistent object orientation; most everything is a method instead of a keyword/operator/built-in function. This is more intuitive to me, and lets you do lots of stuff by chaining methods.
* (Specifically, why the heck are `map()`, `reduce()`, etc. built-in functions in Python instead of methods on iterable objects?)
* `if` and `case` statements are expressions that return a value.
* Lots of things that make it easier to be succinct – occasionally at the expense of more complexity, but I don’t mind this in cases where it’s obvious what is happening.

To elaborate on the last point: I think that explicitness is good, and
simplicity is good, but brevity is also good. The less code there is, the more
quickly you can read it and understand what it does. Some of Ruby's deviations
from the Python "There's Only One Way To Do It" ethos are beneficial in this
regard. Some people have a problem with one-line conditionals and `unless`
because they're redundant, but if you see `raise ArgumentError if x < 0` or
`unless foo == bar`, it's immediately clear what that means. Ruby also has the
ternary operator, which Python did eventually approximate with the
`x = y if a else z` syntax, but that's just weird.

I also like the fact that Ruby assignments return the value assigned – which
can cause `=` vs `==` mixups, but the interpreter will yell at you unless you
use the "safe assignment in condition" idiom, and it's convenient to be able to
do stuff like this:

{% highlight ruby %}
if (match = regex.match(str))
  do_stuff_with(match)
end

while (line = file.gets)
  process(line)
end
{% endhighlight %}

Sometimes, though, the flexibility comes at the expense of clarity. There are
at least four ways to call a proc, two of which I've never actually seen anyone
use:

{% highlight ruby %}
foo = Proc.new { |x| x }
# As far as I can tell, these are all equivalent
foo.call(bar)
foo.(bar)
foo::(bar)
foo[bar] # what
{% endhighlight %}

. . . and Ruby has lambdas as a special type of proc. I always used to forget
which was which, and I still can't think of any good reason to use a regular
proc rather than a lambda, although I use them in Rails because it's idiomatic.

Likewise, although the usual logical operators are `&&`, `||`, `!`, you *can* in
fact use `and`, `or`, and `not` – but they have lower operator precedence. An
alternate syntax that works, but works in a very subtly different way, is not a
good thing. (These operators were apparently intended to be used for control
flow, but in practice I suspect their only purpose is to troll Python
programmers.) And those weird Perl-esque global variables – `$'`? What the heck
is `$'`?

In 1993, it may have made sense to accommodate programmers who were used to C,
Perl, Smalltalk, etc., etc., but now that there's a corpus of actual production
code to draw on, I think Ruby needs a backward-incompatible update to remove all
the cryptic features that no one actually uses.
