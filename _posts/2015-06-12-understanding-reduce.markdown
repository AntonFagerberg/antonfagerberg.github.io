---
  layout: post
  title: "Understanding reduce / fold"
  categories: blog
---

_Disclaimer: There are some differences in the definition of fold and reduce in different languages - I won't explore them here. Neither is the right / left variations of fold or reduce explained here._

Fold or reduce, from now on I will only use fold, is one of those functions which can be a bit hard to grasp when you're new to functional programming but once you get it, it feels very natural and you end up using it all the time. A little while back I listened to a discussion about how to explain fold to novice programmers. Usually you end up drawing each iteration on a whiteboard and try to explain how the accumulator changes in each step.

But yesterday I was reading the paper [Why functional programming matters](https://github.com/papers-we-love/papers-we-love/raw/master/functional_programming/why-functional-programming-matters.pdf) and found that it explained fold in a way which I'd never seen before - perhaps that's just me but it goes something like this:

Suppose you want to use fold on a list. First we must acknowledge that a list simply consists of many [cons cells](https://en.wikipedia.org/wiki/Cons). In Scala, we can create the `cons cells` used in lists with the function `::`. The function will take a value on the left hand side of the operator and another `cons cell` or the "empty" `Nil` value on the right hand side, and return a new `cons cell`. 

This is how it looks like:

```scala
// Nil is equal to the empty list.
Nil == List()

// A cons cell consisting of 1 and Nil equals a
// list with one element which has the value 1.
1 :: Nil == List(1)

// A list consisting of any number of elements can
// be created by combining multiple cons cells.
1 :: (2 :: (3 :: Nil)) == 1 :: 2 :: 3 :: Nil == List(1, 2, 3)
```

Now we want explain fold by creating the function `sum` which sums up the elements of a list. In Scala, we can do it like this:

```scala
// Add is a function that takes two integers, a and b
// and returns the resulting a + b.
val add = (a: Int, b: Int) => a + b

// Sum is a function which takes a list of integers
// and returns the sum of all the integers using fold.
val sum = (l: List[Int]) => l.fold(0)(add)


// This is how it we can use it.
sum(List(1, 2, 3)) == 1 + 2 + 3 == 6
```

What the paper did, which I found interesting, is how it explains how the `fold` works. As seen in the example above, `fold` takes two arguments, an accumulator: `0`, and a function: `add`. By using `fold` on a list, we essentially replace the function `::`, which represents the `cons cells`, with our specified function `add` (or `a + b`), and the accumulator `0` replaces the Nil value.

Or explained in code:

```scala
sum(List(1, 2, 3)) ==
sum(1 :: (2 :: (3 :: Nil))) ==
1 + (2 + (3 + 0))
```

A neat way to explain fold or reduce if you ask me. The paper continues and applies this concept to a tree, but you have to read that yourself if you want to know more - I highly recommend it!