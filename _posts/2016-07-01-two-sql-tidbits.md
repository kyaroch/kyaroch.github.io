---
layout: post
title:  "Two Small Findings About SQL and Rails"
date:   2016-07-01 15:10:00 -0500
---
Part 2 of the geographic polarization post should be up soon; I got distracted
by another project for long enough that I'll have to spend some time figuring
out what it was that I was doing.

In the meantime, here are two minor (and mildly interesting) things I figured
out today while I was at work.

#### Conditions on a LEFT OUTER JOIN

I was working on a new scope for a Rails application I maintain, which uses a
legacy MySQL database. I needed to do a `LEFT OUTER JOIN` between two tables,
but include rows from the second table only if they met certain criteria. Of
course, using a standard `WHERE` clause in this situation would effectively
convert the `LEFT OUTER JOIN` to an `INNER JOIN`. In a similar case I
encountered previously, I was able to fix this by adding `OR table_2.id IS NULL`
to the query, but in this case that didn't work, for reasons I don't yet
completely understand.

The solution was to put the condition in the join itself:

{% highlight sql %}
SELECT
  foo.*
FROM foo
LEFT OUTER JOIN bar
  ON foo.id = bar.foo_id
  AND bar.some_attribute = 'some value';
{% endhighlight %}

This is probably obvious to someone more experienced with SQL, but it didn't
initially occur to me that I could do this.

#### Interpolating a date into a Rails SQL query

Of course, normally you can do this as follows:

{% highlight ruby %}
where("created_at < ?", date)
{% endhighlight %}

However, it turns out that this won't work in a `JOIN` clause. It'll raise
`RuntimeError: unknown class: DateTime`. The solution I ultimately found was to
do this:

{% highlight ruby %}
joins("bar on bar.foo_id = foo_id AND bar.created_at < '#{date.to_s(:db)}'")
{% endhighlight %}

Passing `:db` as a parameter to `#to_s` will format the DateTime object's string
representation in such a way that it can be used directly in an SQL query. Two
major caveats about this:

* Formatting the string in this way removes the time zone.
* Interpolating user-submitted input directly into an SQL query is [not good practice](http://rails-sqli.org/),
obviously. Passing a symbol as a parameter to `#to_s` will raise an exception
with most object types other than `DateTime`, notably including `String` itself.
Still, I'd be careful with this. (In the context where I used this, it shouldn't
matter, as the times being used in the query are set inside the model and
there's no way for a user to manipulate them.)
