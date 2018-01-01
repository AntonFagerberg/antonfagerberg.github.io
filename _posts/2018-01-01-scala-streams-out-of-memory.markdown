---
layout: post
title: "Scala streams - out of memory"
categories: blog
---

I was doing [Advent of Code](http://adventofcode.com) and was working on generating
numbers based on a pattern (day 15). Let's say I wanted to generate `0, 1, 2, ...`
to simplify this example.

The first thing that came to mind was building a stream. One way to do 
this in Scala is as follows:

```scala
Stream.iterate(0)(_ + 1)
```

or you can do it like this:

```scala
lazy val stream: Stream[Int] = 1 #:: 2 #:: stream.tail.map(_ + 1)
```

The plan was then to lazily generate 40 million numbers, filter out then ones I 
wanted and count them. Turns out this didn't work at all as memory started to run out 
(`OutOfMemoryError`) for two reasons which at first seemed counterintuitive.

First, if you look at the source code for `Stream` in Scala you find a lot warnings such
as:

```
*  - One must be cautious of memoization; you can very quickly eat up large
*  amounts of memory if you're not careful.  The reason for this is that the
*  memoization of the `Stream` creates a structure much like
*  [[scala.collection.immutable.List]].  So long as something is holding on to
*  the head, the head holds on to the tail, and so it continues recursively.
*  If, on the other hand, there is nothing holding on to the head (e.g. we used
*  `def` to define the `Stream`) then once it is no longer being used directly,
*  it disappears.
```

The `take` method has more to say:

```scala
override def take(n: Int): Stream[A] = (
  // Note that the n == 1 condition appears redundant but is not.
  // It prevents "tail" from being referenced (and its head being evaluated)
  // when obtaining the last element of the result. Such are the challenges
  // of working with a lazy-but-not-really sequence.
  if (n <= 0 || isEmpty) Stream.empty
  else if (n == 1) cons(head, Stream.empty)
  else cons(head, tail take n-1)
)
```

Unaware of this, you (as I) may think that code like this should work just fine:

```scala
object MemoryTest extends App {
  val stream = Stream.iterate(0)(_ + 1)
  println(stream.dropWhile(_ < 1000000).head)
}
```

Running the code above with `-Xmx10m` will throw an `java.lang.OutOfMemoryError: GC overhead limit exceeded` 
exception. However, if you change the `val` to `def` it will work just fine since
the stream's head reference can be garbage collected.

```scala
object MemoryTest extends App {
  def stream = Stream.iterate(0)(_ + 1)
  println(stream.dropWhile(_ < 1000000).head)
}
```

The next tricky thing is that many methods, such as the common `filter` method, 
and unlike `dropWhile`, isn't implemented in `Stream` but in `TraversableLike`. 

With `filter` you have to worry about how many elements that are going to be
memoized in between the elements which the predicate returns true for.

Running the following code with `-Xmx10m` will thrown `OutOfMemoryError`:

```scala
object MemoryTest extends App {
  def stream = Stream.iterate(0)(_ + 1)
  println(stream.filter(_ >= 1000000).head)
}
```

While this code will work just fine:

```scala
object MemoryTest extends App {
  def stream = Stream.iterate(0)(_ + 1)
  println(stream.filter(_ % 100 == 0).filter(_ >= 1000000).head)
}
```

With this in mind, many memory gotchas in Scala streams can be avoided. In the 
end though, I realised that I didn't need `Stream` but rather `Iterator` for my
[Advent of Code solution](https://github.com/AntonFagerberg/advent_of_code_2017/blob/master/src/main/scala/com/antonfagerberg/Day15.scala).

```scala
object MemoryTest extends App {
  val iterator = Iterator.iterate(0)(_ + 1)
  println(iterator.filter(_ >= 1000000).next())
}
```

If you're not going to take advantage of the memoization, go with `Iterator` instead!