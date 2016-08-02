---
layout: post
title:  "Comparison operators in Elixir"
date:   2014-12-28 02:09:25
tags: elixir
---

When I began learning [Elixir](http://elixir-lang.org/) and worked my way through [the getting started guide](http://elixir-lang.org/getting_started/1.html), I found two things which felt a bit out of place. One of them was the comparison operators which got me thinking more around this topic.

In Elixir, there are two equals operators: `==` and `===`. The difference between them are similar to other languages whereas the latter is more strict than the former.

{% highlight elixir %}
iex(1)> 1 == 1.0
true
iex(2)> 1 === 1.0
false
{% endhighlight %}

It can also be used in the same regard in nested structures:

{% highlight elixir %}
iex(1)> [1, 2, 3] == [1, 2, 3.0]
true
iex(2)> [1, 2, 3] === [1, 2, 3.0]
false
iex(3)> %{a: 2} == %{a: 2.0}
true
iex(4)> %{a: 2} === %{a: 2.0}
false
{% endhighlight %}

To accompany the stricter `===` operator, Elixir also has the negated version, `!==`, but as in most other languages, there is no strict version of the other comparison operators: `<`, `>`, `<=`, `<=`. While one could imagine that `<==` and `>==` easily could be created, there isn't really one as intuitive syntactic version for `>` and `<`.

As a comparison, in Scala and Java to name two languages, there is no strict comparison operators. In these languages, if we compare integers and floating point numbers, the integer is converted to a floating point number since this conversion does not lead to any data loss (which would happen if `1.1` would be converted to an integer). 

The interesting thing to think about how is more how the less strict `==` operator works in those languages that does have the strict version. In languages like PHP and JavaScript, the `==` operator is much more relaxed than in Elixir and so numeric values can, to take one example, be compared with strings in this same regard as well:

{% highlight javascript %}
"1.0" == 1 
true
{% endhighlight %}

In JavaScript, the implementation of the less strict `==` operator doesn't only apply to comparing numeric types and strings but [can also cause much wonder](http://stackoverflow.com/questions/359494/does-it-matter-which-equals-operator-vs-i-use-in-javascript-comparisons). These sometimes unexpected behaviours are however not possible in Elixir:

{% highlight elixir %}
iex(1)> "1" == 1
false
{% endhighlight %}

So in Elixir, the `===` operator only has one use which is to avoid comparing integers and floating point numbers which leads me to wonder if the `===` operator really is necessary in Elixir? My guess is that it is an inherited behaviour from Erlang which has the `=:=` operator (exactly equal to) and the corresponding `=/=` operator "exactly not equal to".

For me, the `===` operator seems to be unnecessary in Elixir - I've never missed the `===` operator in Scala and if it was removed from Elixir, the same behaviour could be recreated by combining the `is_integer/1` or `is_float/1` function with the `==` operator.