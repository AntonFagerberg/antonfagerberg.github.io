---
layout: post
title:  "Run specific tests in Elixir (ExUnit)"
date:   2015-04-28 14:21:00
categories: blog
tags: elixir, exunit, tests
---

When developing Elixir applications, there are two options I've learned to use for running my tests in a more specific manner.
The normal approach for running tests in a Mix based Elixir project is to simply run: 

```
mix test
```

This will run all tests, but you can also use this task to only run a specific test file by executing: 

```
mix test test/some_test.exs
```

You can be even more specific by specifying the line-number of a single task you want to run. Say that you have a test on line 42, then you simply execute: 

```
mix test test/some_test.exs:42
```

The line number, 42 in the previous example, can be any line number between the line where the wanted test's `test` keyword is defined until the next test's `test` keyword.