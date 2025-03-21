---
layout: post
title: "Advent of Code"
categories: projects
tags: code
---

At the time of writing this, I've been doing [Advent of Code](https://adventofcode.com) for 9 years. 
Or put another way, I've been doing Advent of Code every year since it started in 2015.
Although, this may have been my last year. (I've been saying that for many years but I'll try to keep away.)

### 2015 - first year in Elixir
I was working at a consultancy firm and heard about this new thing called Advent of Code.
Having recently worked a lot with Elixir I decided that I was going to try to do it in that language.

I don't remember much but I do remember that I finished all problems (in the end, probably not in December). 
I also remember talking to an older colleague that it had ruined his Christmas since he couldn't "let go" of the problems when he should've spent time with his family (I was able to relate to this later in life).

It was other times back then, I ended up on ranking 410 with a solution that took over an hour - and even ranked 803 with a solution that took over 24 hours. That just doesn't happen anymore.

### 2016 - second year in Haskell
This year I decided to challenge myself by establishing a new tradition: using a new programming language every year - I don't know why, probably just to challenge myself.

Being completely sold on functional programming, I decided to use Haskell.
I had done some Haskell at University but it didn't click for me then.
Somehow I managed to solve all problems again (was it easier back then?).
I remember using [Hoogle](https://hoogle.haskell.org) a lot which was amazing.
I also remember using something that lets you print things without having to deal with side effects, as I remember it was named pry but I can't find it now.
Probably something inside `Debug.Trace`.

### 2017 - third year in Scala
In 2017 my first child was born. 
She was just two weeks old when Advent of Code started so I decided to play it safe and use a language I knew really well: Scala.
I managed to snatch 37 stars. Probably 37 more than I should've because I was *very* sleep deprived during this time.

### 2018 - fourth year in Java
This year I had little interest in Advent of Code, I was probably a bit worn out from "life". But this year I was moving and had just signed up for a new job in which I was going to use Java.
I hadn't used Java for a few years and was not up to date with the new things (which at the moment was things like the `streams` API).
So I used Advent of Code as a way to practice and it was actually pretty great - but I do remember hating Java at first having worked with Scala for a very long time, I thought I'd made the worst mistake by taking this job.
In the end, I collected 8 stars. (This year a lot more people started competing, my raking was over seventy-six thousand the first day.)

And if you're curious, at the time of writing I'm still on the same job, I write a lot of Java and I really enjoy it!

### 2019 - firth year in Kotlin
I started feeling the Kotlin hype and I felt like Kotlin would be an easy language to learn given that I already knew Java and Scala.
The problems was a bit weird this year, as I remember it, but very fun. 
I especially remember that you had to write something like a virtual machine and build a playable game inside it.
Many puzzles built on top of the solutions for the previous puzzles which was a bit tedious.
Got 28 stars that year. Didn't get hooked on Kotlin.

### 2020 - sixth year in Clojure
This year my second child was born so I had practically no spare time and no sleep (again).
I decided to use Clojure nonetheless as I'd wanted to try a Lisp for a very long time.
In the end, I only solved two days (four stars) in Clojure, collected a total of 7 stars in the end using other languages.
Good decision to throw in the towel.

### 2021 - seventh year in Ruby
My girlfriend was "very pregnant" at this time. Our second son (third child) was expected the next month so I had to take care of our two other children on my own a lot at this point in time. I decided to pick a language I was vaguely familiar with but which would still let me get away with "a new language every year" - so I picked Ruby.
I'm not sure why but I hated using Ruby this year (I liked it before, also, turns out I didn't remember much of it). I felt like it was a stupid idea to use a new language "just because" - but I completed 8 stars in Ruby and then did some more in Java. The end result was 30 stars.

### 2022 - eighth year in Go
If you've been reading along, at this point in time I had three small kids - one of them not one year old yet.
This year I went back and picked a truly new language for me: Go.
I enjoyed this year a lot and actually learned quite a lot of Go.
It's a language I would consider using more.
18 stars was collected that year which felt like enough.

### 2023 - ninth year (competition / back to Java)
At first, I planned to use OCaml to keep the "new language every year" tradition alive. I started learning it a bit in advance and I thought I was going to love it when reading the documentation.
But when I started using it I really didn't like it. It was clunky to write things (like printing a nested map to stdout) and the build tool Dune was finicky for some reason.
(Most of these things are probably my fault, but you know...).
My plan was to solve one problem and then throw in the towel.

Instead, what happened was that my job announced this internal competition. 
I *hate* competing but I thought, you know what, I'm going to go all in on that this year - for once in my life.
End it with a bang.
I've been doing this for so long, and if I'm gonna compete in something it's going to be this.
So I picked Java as my language (breaking tradition) to ensure that I had something I was very comfortable using.

In the end I collected 47 stars. I even got in the top 1000 for one problem (hey, place 999 counts!).
I didn't try to compete globally as that is way out of my league, and wasn't trying to be particularly fast - just a bit faster than everyone in my company.

I learned a lot of new things about algorithms this year and a few new details about Java (like the iterator of a `PriorityQueue` will not return items in the sorted order... ugh...). 

### Links to the pages I wrote every year
{% for post in site.tags.advent-of-code %}
- [{{ post.title }}]({{ post.url }})
{% endfor %}