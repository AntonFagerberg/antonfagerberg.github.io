---
  layout: post
  title: "Name types, not just variables"
  categories: blog
  star: true
---

A primitive data type can be defined as the most basic building block of a programming language. Typical primitive data types are integers, floats and booleans. Whether you differentiate between these types and objects in languages such as Java, or if they're all objects like in Scala, or if you language doesn't have objects at all doesn't matter in the following text. What does matter is that the language is statically typed.

## Naming variables

In order to make our code usable and understandable, we typically name methods, functions and variables to give us clues about what we're dealing with.

```scala
val age = 30

def getAge(person: Person): Int = { ??? }

val isOfSameAge: (Int, Int) => Boolean = ???
```

Replace `age`, `getAge` and `isOfSameAge` with `x`, `y` and `z` and our code gets extremely difficult to work with. Naming variables is very important but it's not always enough.

Suppose we're writing some analytics tool which will process "big data". Usually we want to extract certain values from larger objects in order to reduce the amount of data shuffled over the network. Imagine that we have a a lot of huge `Person` objects and a big set of `ShoeStore` objects and now we want to process this data in some way. For our current business case, we need the age, zip code and name of all the persons, and the zip code and warehouse id from all our shoe stores (and the zip code is just an integer in our country). 

So we create these two methods to extract the minimal data that we're interested in:

```scala
def getAgeZipCodeAndName(person: Person): (Int, Int, String) = ???
def getZipCodeAndWarehouseId(shoeStore: ShoeStore): (Int, Int) = ???
```

That kinda works, but how much can we rely on the naming of the method. If we look at `getZipCodeAndWarehouseId`, we can see that it returns two integers - but what are they? Well, hopefully it's the zip code and warehouse id, but what about the ordering? Is it the order defined in the method name, zip code first, or is it more logical to always return the warehouse id first? We simply do not know just by looking at the method definition.

## Type aliases and case classes

Two common ways to improve upon this is by using type aliases or case classes (case classes can be replaced by classes, records or whatever your language supports).

In Scala, we can define a type alias as follows:

```scala
type WarehouseId = Int
type ZipCode = Int

def getZipCodeAndWarehouseId(shoeStore: ShoeStore): (WarehouseId, ZipCode) = ???
```

Note that a type aliases are, as the name suggests, only aliases - they're not new types.

Or we can use a case class:

```scala
case class AgeZipCodeAndName(age: Int, zipCode: Int, name: String)

def getAgeZipCodeAndName(person: Person): AgeZipCodeAndName = ???
```

Great, now we know what the methods are returning. Let's take our data sets, extract the most important values and join them together before we begin our analytics.

```scala
val persons: Collection[Person] = getPersons()
val shoeStores: Collection[ShoeStore] = getShoeStores()

val personKeyedByZipCode =
  persons
    .map(getAgeZipCodeAndName)
    .keyBy {
      case AgeZipCodeAndName(age, zipCode, name) => zipCode
    }
    
val shoeStoreKeyedByZipCode =
  shoeStores
    .map(getZipCodeAndWarehouseId)
    .keyBy {
      case (warehouseId, zipCode) => zipCode
    }
    
personKeyedByZipCode.join(shoeStoreKeyedByZipCode)
```

This looks great, so we finish the rest of the code and everything works perfectly.

Then one day your friendly co-worker wants to use your extraction methods. But you know what, turns out that `age` isn't used so it gets removed and a new parameter `shoeSize` is added. And of course, we rename the method accordingly.

So the case class get's changed into:

```scala
case class ZipCodeShoeSizeAndName(zipCode: Int, shoeSize: Int, name: String)
```

The code still compiles and we're all happy. At least until we run it and discover that our join suddenly returns no matches.

The reason why is found here, but it's hard to spot just by skimming through the code:

```scala
case ZipCodeShoeSizeAndName(age, zipCode, name) => zipCode
```

We're pattern matching the `ZipCodeShoeSizeAndName` object but what we're extracting as the second parameter is `shoeSize` - and we're naming it `zipCode`. Seen from the compiler, this is just fine, it's still three parameters, `zipCode` and `shoeSize` is even of the same type!

## Naming types
I think most people would find the following code peculiar to say the least:

```scala
if (person.shoeSize > shoeStore.zipCode)
```

Our type system on the other hand would have no problem with this. 

But what if we not only named the variables and functions but also the types in such a way that the type system could catch these problems?

We could create a new type for every primitive used to represent different things:

```scala
case class Age(value: Int)
case class ZipCode(value: Int)
case class Name(value: String)
case class WarehouseId(value: Int)

case class Person(age: Age, zipCode: ZipCode, name: Name)
case class ShoeStore(zipCode: ZipCode, warehouseId: WarehouseId)
```

And use it like this:

```scala
case class Person(Age(30), ZipCode(22474), Name("Anton Fagerberg"))

// Now WarehouseId and ZipCode are types, not type aliases
def getZipCodeAndWarehouseId(shoeStore: ShoeStore): (WarehouseId, ZipCode) = ???
```

Being forced to name the types actually gives us pretty nice documentation, a bit similar to [named parameters](http://docs.scala-lang.org/tutorials/tour/named-parameters.html). (In Scala, we could get rid of having to specify `Age(30)` in the constructor by using [implicit conversions](http://docs.scala-lang.org/tutorials/tour/implicit-conversions).)

Now we'll get an error if we tried to do this:

```scala
person.shoeSize == shoeStore.zipCode
```

Another benefit is that we would catch parameters passed in the wrong order to functions:

```scala
def isShoeInStock(size: ShoeSize, warehouseId: WarehouseId): Boolean = ???

isShoeInStock(WarehouseId(1), ShoeSize(44)) // ==> type mismatch

```

But there are problems, for example, unless we define `>` on our `Age` type, we can't do this:

```scala
person1.shoeSize > person2.shoeSize
```

We can solve this issue but I will stop here. Writing all this boiler plate code and setting up everything is annoying. So annoying that we most likely just won't do it.

## A wish for a future language
I would like to try a programming language in which you can't create any primitive types by themselves - you would be forced to name them, and by doing so, create new types. All comparisons and methods would work just like normal primitives, but only if the name were the same.

In order for this to not be as annoying as the previous examples, I believe minimal setup would be a requirement. 

As an example, I would like to be able to do the following without having to define `Age` or `ZipCode` in any other way:

```scala
val myAge: Int[Age] = 30
val yourAge: Int[Age] = 56
val someZipCode: Int[ZipCode] = 22474

def getAge(person: Person): Int[Age] = ???

val age: Int = 30 // ==> ERROR: no un-named primitives allowed

myAge == yourAge // ==> false, but ok type comparison

myAge == someZipCode // ==> ERROR: type mismatch
```

There are of course many more things to consider. Should the types be scoped or is it better if they are global? How can we convert a named type to another named type? Should we be able to compare two different named types in some scenarios? Does it make sense to [compare a persons age to the age of a dog](https://en.wikipedia.org/wiki/Aging_in_dogs#/media/File:Dog_and_human_year_graph.png) - how specific will your types be in the end?

Nevertheless, I think it is an interesting concept to consider. It could be "overkill" for many applications but I believe certain areas, such as analytics and data processing, could benefit from it and I'm sure it would make certain code bases, at least a bit, less error prone.

### Further reading
 * [Units of measure in F#](https://fsharpforfunandprofit.com/posts/units-of-measure/).
 
Do you have any resources where this or similar concepts are explored? Please let me know!