---
layout: post
title: "Extending instances on creation for debugging"
categories: blog
tags: misc
---

A neat trick you can do in Scala is extending an object (instance of an existing class) on the fly when you instantiate it. This comes in very handy when debugging code you do not own yourself.

As an example, I was working with Javas `DigestInputStream` to calculate a MD5 checksum on the fly from an `InputStream`. I wanted to know how the MD5 checksum changed during each partial read of the stream, that is when the `read` method of `DigestInputStream` is invoked, and invoke my own method with a local variable.

Here's what the code looked like:

```scala
def myMethod(/* parameters */) = { /* Some implementation */ } 
val localVariable = someThing;
val messageDigest = MessageDigest.getInstance("MD5")
val digestInputStream = new DigestInputStream(inputStream, messageDigest)
```

The implementation for `DigestInputStream` is not in my code base, meaning I can't put things in the `read` method. You can place a break point on it and invoke code on the fly from your IDE, but then you get into the problem of `localVariable` and `myMethod` being out of scope.

Here's where extending the instance on creation comes in handy. It means is that you can override the `read` method on your custom instance, invoke the local method `myMethod` using the local variable `localVariable` in it and then pass everything to `super.read` to keep the existing functionality intact.

```scala
def myMethod(/* parameters */) = { /* Some implementation */ } 
val localVariable = someThing;
val messageDigest = MessageDigest.getInstance("MD5")
val digestInputStream = new DigestInputStream(inputStream, messageDigest) {
  override def read(b: Array[Byte], off: Int, len: Int) = {
    myMethod(localVariable) // Yay!
    super.read(b, off, len)
  }
}
```

Alternatively, you can put a break point in your overridden `read` method to get everything in scope and debug with your IDE.

Happy debugging!