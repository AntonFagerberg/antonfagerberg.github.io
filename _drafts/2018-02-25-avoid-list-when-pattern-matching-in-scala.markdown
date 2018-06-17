---
layout: post
title: "Avoid pattern matching with List in Scala"
categories: blog
---

I recently debugged a strange issue which seemed to appear out of
nowhere™. It all came down to a that we used `List` in a `match` while
the methods signature was changed from `List` to `Seq`.

Below is a simplification of what happened and how it can be avoided. 
It's one of those things which seem obvious in hindsight, but
in a complex system, these things can be tricky to spot.

In our code, we had a method which took a list of something and depending
on the number of items in it we wanted to do something.

Here's an simplified example:

```scala
def specialSum(numbers: List[Int]): Int = {
  numbers match {
    case List(a, b) => a + b
    case Nil => -1
    case _ => 0
  }
}

println(specialSum(List(1, 2))) // 3
println(specialSum(List(1)))    // 0
println(specialSum(List.empty)) // -1
```

When starting to use this method in our code base, we had other collections than `List` that we wanted to use (and using `.toList` everywhere is tedious). So `List` was changed to `Seq` to accomodate this:

```scala
// numbers was changed from List to Seq
def specialSum(numbers: Seq[Int]): Int = {
  numbers match {
    case List(a, b) => a + b
    case Nil => -1
    case _ => 0
  }
}

println(specialSum(List(1, 2))) // 3
println(specialSum(List(1)))    // 0
println(specialSum(List.empty)) // -1
```

With List it still works. Now, one may write a unit test using `Seq` in order to verify that the method's changed type signature still works:


```scala
def specialSum(numbers: Seq[Int]): Int = {
  numbers match {
    case List(a, b) => a + b
    case Nil => -1
    case _ => 0
  }
}

println(specialSum(Seq(1, 2))) // 3
println(specialSum(Seq(1)))    // 0
println(specialSum(Seq.empty)) // -1
```

This still works but this is just luck (or unlucky for us). This is because the `Seq`'s default implementation is `List`. So when we say `Seq(1, 2)` we actually (under the hood) get a `List(1, 2)`.

You can verify this with:

```scala
Seq(1,2).isInstanceOf[List[_]]
```

What happened in our code was that we wanted to pass in some other type which inhertis from `Seq`, for example `ArrayBuffer`.

```scala
def specialSum(numbers: Seq[Int]): Int = {
  numbers match {
    case List(a, b) => a + b
    case Nil => -1
    case _ => 0
  }
}

println(specialSum(ArrayBuffer(1, 2))) // 0 <- not what we want
println(specialSum(ArrayBuffer(1)))    // 0
println(specialSum(ArrayBuffer.empty)) // -1
```

What happens here is that our `ArrayBuffer` is passed in and is being "downgraded" to a `Seq`. 

The `match` has three cases:

 - Is the `Seq` of the more specific type `List` and does it have to elements. (This is no longer true.)
 - `Nil` is a short hand for empty `List`, but `ArrayBuffer.empty[Int] == Nil` will still hold true so it works.
 - `_` will match everything independent of type.

 Notice also that since we do match on `_` all compiler warnings and runtime errors (`MatchError`) will dissapear since we cover all possible outcomes.

 If we did:

 ```scala
  def specialSum(numbers: Seq[Int]): Int = {
  numbers match {
    case List(a, b) => a + b
    case Nil => -1
    case _: List[Int] => 0
  }
}

println(specialSum(ArrayBuffer(1, 2))) // 3
println(specialSum(ArrayBuffer(1)))    // 0
println(specialSum(ArrayBuffer.empty)) // -1
```

We'd get a `MatchError`.

In order to be safe, we can make sure that we always use `Seq` in the `match` since this will cover all collections which inherits from it ([more info here](https://docs.scala-lang.org/overviews/collections/overview.html)).

```scala
def specialSum(numbers: Seq[Int]): Int = {
  numbers match {
    case Seq(a, b) => a + b
    case Seq() => -1 // or Nil will work
    case _ => 0
  }
}

println(specialSum(ArrayBuffer(1, 2))) // 3 
println(specialSum(ArrayBuffer(1)))    // 0
println(specialSum(ArrayBuffer.empty)) // 0
```

As a bonus, if you for some reason prefer to use the `cons` notation:

```scala
def specialSum(numbers: Seq[Int]): Int = {
  numbers match {
    case a :: b :: Nil => a + b
    case Nil => -1
    case _ => 0
  }
}

println(specialSum(ArrayBuffer(1, 2))) // 0 <-- wrong
println(specialSum(ArrayBuffer(1)))    // 0
println(specialSum(ArrayBuffer.empty)) // -1
```

You can make it work by using `+:` (since `::` is `List` specific):

```scala
def specialSum(numbers: Seq[Int]): Int = {
  numbers match {
    case a +: b +: Nil => a + b
    case Nil => -1
    case _ => 0
  }
}

println(specialSum(ArrayBuffer(1, 2))) // 3
println(specialSum(ArrayBuffer(1)))    // 0
println(specialSum(ArrayBuffer.empty)) // -1
```

I wouldn't recommend it though, I personally don't find it readable (and I'm actually surprised it works with `+: Nil`, I thought `+: Seq()` was required in order to not get a `List`).

**Update:** It was pointed out to me that `+:` is slower than `::`. If you are recursively traversing a very long sequence, then `List` and `::` will give you much better performance.