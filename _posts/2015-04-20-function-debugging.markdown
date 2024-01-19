---
layout: post
title:  "Function debugging"
date:   2015-04-20 09:09:09
categories: blog
---

I was going through the [OCaml Code Examples](http://ocaml.org/learn/taste.html) the other day after listening to [Eric B. Merritt on the podcast Functional Geekery](http://www.functionalgeekery.com/episode-20-eric-b-merritt/). One thing that really stuck with me was the last paragraph called "Elementary debugging".

It is a very short paragraph which only contain the line "To conclude, here is the simplest way of spying over functions" and a short code example:

```ocaml
let rec fib x = if x <= 1 then 1 else fib (x - 1) + fib (x - 2);;
# #trace fib;;
fib is now traced.
# fib 3;;
fib <-- 3
fib <-- 1
fib --> 1
fib <-- 2
fib <-- 0
fib --> 1
fib <-- 1
fib --> 1
fib --> 2
fib --> 3
- : int = 3
```

This is a feature I think every programming language should have built in to it by default. You simply say which function you want to trace and the console will print each input value and each output value.