---
  layout: post
  title: "Distributed collections in the standard library"
  categories: blog
  star: true
---

When working with sets of data, we usually work with strict (eager) collections. Sometimes we wish to work with lazy collections such as when we're dealing with infinite streams. If our data set is getting big enough, we may wish to use parallel collections in order to utilise our multi-core CPU to the fullest - but when we move towards huge amounts of data we must start thinking about distributed collections.

There are many frameworks that specialise in dealing with this problem but as our languages have been adding support for lazy and parallel collections, could the next step be distributed collections built into the language's standard library?

In this blog post I look at two of my favourite programming languages, Scala and Elixir, and how they can start, and in some regards already has begun, evolving towards distributed collections.

### Distributed collections
When I talk about distributed collections in this post, I'm referring to two things:

1. The abstraction, or type, we use to represent the collection of data (like `Array` or `List`).
2. A processing engine which we can use to do computations on the collection in a distributed way (on a cluster of machines joined in a network).

### Spark - The Ultimate Scala Collections
A concrete example of a distributed collection is the [RDD (Resilient Distributed Dataset)](http://spark.apache.org/docs/latest/programming-guide.html#resilient-distributed-datasets-rdds) used in [Apache Spark](http://spark.apache.org). The RDD is great because it looks and behaves very much as the standard Scala collections. RDDs supports many of the functions you already know and use such as `map`, `fold`, `reduce`, `filter`, `zip` and so on. Further more, the code you write looks very much the same as any Scala code using standard collections, but with the great improvement that the operations performed on the RDD can be distributed between many machines in a cluster.

In the fall of 2015, the creator of Scala, Martin Odersky, did a talk called [Spark—The Ultimate Scala Collections](https://spark-summit.org/eu-2015/events/spark-the-ultimate-scala-collections/). In his talk, Odersky argues that Spark is a DSL on top of Scala created in order to extend the language with new functionality: distributed computing.

As an example, Odersky shows this piece of code in one of his slides:

```scala
val xs = data.map(f)

xs.filter(p).sum
xs.take(10).toString
```

Spark RDDs are, and must be, lazy while (most) Scala collections are strict. In the most common Scala programs, you don't want lazy evaluation of collections. You want the `map` on line 1 to be evaluated once (strict) and not twice (lazy) - you can think of strict vs lazy in this example as whether the value of `xs` should be cached or not. 

But, for when you do want the lazy behaviour, Scala already has [views](http://docs.scala-lang.org/overviews/collections/views.html) but what Odersky mentions in his talk is a new alignment of views to make them behave more like the RDDs in Spark - perhaps even adding [pair-wise operations](http://spark.apache.org/docs/latest/programming-guide.html#working-with-key-value-pairs). When Odersky talks about views, he says that they enable the Scala code to produce a "recipe" rather than eagerly evaluating the collection operations. This "recipe" is similar to the RDD operations Spark uses to schedule how a cluster of networked machines should do computations on the distributed collections.

Just like Scala has support for [parallel collections](http://docs.scala-lang.org/overviews/parallel-collections/overview.html) which can be used whenever the user wants to utilise several CPU cores, having a distributed collection for when the user wants to utilise a cluster of computers would be a really interesting language feature.

Distributed collections do however require a way to actually schedule and execute the the computations on a cluster which is far from a trivial problem. On the other hand, Scala already have an official distributed actor system thanks to [Akka](http://akka.io) so we can at least play around with the though of this being possible.

### Elixir - GenStage
Ever since I started using Spark I thought to myself that given Elixir's distributed nature, wouldn't it be a good candidate for implementing a Spark-like distributed processing engine?

Earlier this month, José Valim [announced GenStage on the Elixir blog](http://elixir-lang.org/blog/2016/07/14/announcing-genstage/). In the blog post, Valim shows a word counting example:

```elixir
File.stream!("path/to/some/file")
|> Stream.flat_map(&String.split(&1, " "))
|> Enum.reduce(%{}, fn word, acc ->
    Map.update(acc, word, 1, & &1 + 1)
   end)
|> Enum.to_list()
```

The example illustrates how we can stream a large file and do a map and a reduce operation on it - but as Valim points out, doing this in Elixir will cause you to only use a single core on the machine on which the code is being executed. 

GenStage is Elixir's way of exchanging events with back-pressure between Elixirs processes. I won't go into detail about the GenStage architecture, the [blog post announcement](http://elixir-lang.org/blog/2016/07/14/announcing-genstage/) is a really good read if you want to know more about it.

However, included in GenStage is the new module [Flow](https://hexdocs.pm/gen_stage/Experimental.GenStage.Flow.html#content), and with it, the previous word counting example can be re-implemented as follows:

```elixir
alias Experimental.GenStage.Flow

File.stream!("path/to/some/file")
|> Flow.from_enumerable()
|> Flow.flat_map(&String.split(&1, " "))
|> Flow.partition()
|> Flow.reduce(fn -> %{} end, fn word, acc ->
  Map.update(acc, word, 1, & &1 + 1)
end)
|> Enum.to_list()
```

By replacing the `flat_map` and `reduce` function calls in the `Enum` or `Stream` module to the corresponding functions in the new `Flow` module, the processing of the collection can now take place in parallel on all cores available.

Given that Elixir runs on the Erlang (BEAM) VM which is known for low-latency, fault-tolerant and, especially interesting, distributed systems, expanding this approach to work with Elixir processes distributed over multiple machines in a cluster should be a natural next step.

In fact, this seem to be what Elixir are heading towards:

> _[...] we want to provide developers interested in manipulating collections with a path to take their code from eager to lazy, to concurrent and then distributed._

## In conclusion
While Scala is mostly glancing at Spark for inspiration, I don't believe any real effort is being made towards supporting distributed collections in the language itself. However, if the standard Scala collections end up looking more like Spark's RDD, then perhaps a framework like Spark itself may become the processing engine for running standard Scala collections on the cluster. It should be noted that Scala has previously been looking towards evolving the language in order to support features desirable in distributed computing such as [Scala SIP-21: Spores](http://docs.scala-lang.org/sips/pending/spores.html).

Elixir, on the other hand, seem to actually be heading in the distributed direction. Looking at the [Genstage.Flow](https://hexdocs.pm/gen_stage/Experimental.GenStage.Flow.html#content) documentation, we can see that the `partition/1` function is looking very similar to an explicit [shuffle operation](http://spark.apache.org/docs/latest/programming-guide.html#shuffle-operations) found in Spark. This makes me believe that the Flow module will end up being closer to the distributed collection processing found in Spark rather than the Scala Parallel collections.