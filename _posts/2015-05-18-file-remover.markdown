---
layout: post
title:  "File Remover"
categories: projects
---

Simple recursive file remover I use for removing annoying files such as the `.DS_Store` files in OS X.

The default behavior is to remove `.DS_Store` files, from the current folder and all its sub-folders, but you can use it with any regular expression.

Written in Elixir.

(I also used this to play with ANSI formatting...)


### How to use

1. Get [Elixir](http://elixir-lang.org/)!
2. Clone [this project](https://github.com/AntonFagerberg/file_remover)
3. `iex -S mix`
4. `FileRemover.run("/cool_folder", ~r/\.DS_Store/, :ask)`
5. Wait and enjoy the show!

You can use the following modes:

 * `:ask` - Ask if you want to delete or not.
 * `:print` - Print all matched files.
 * `:delete` - Delete immediately without asking.
