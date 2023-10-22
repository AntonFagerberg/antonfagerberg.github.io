---
layout: post
title: "Snakexpand"
categories: projects
---

I got this idea, what about if you make the game Snake, but you start with a really small level and if you fill it up the screen will expand.

At first, you're on a 4x4 grid where you (the snake) fill up 1/4th of the world and the dot (apple?) fills up 1/4th of the screen.
When you're 4 blocks long, and you fill up the entire screen, the world will expand.

Was it a good idea in theory? YES! In practice? It's not without problems. One of them being that I had to "unroll" the snake since there's a good chance you're about to eat yourself after reaching the last dot.

It was a fun weekend project, a few hours well spent.

I tried using a GameBoy-inspired color scheme. No images or sprites, only drawn boxes. No sound. No Music.

The game was created using Java (JDK 7 which felt old), and libGDX. I compiled a version for the web so it can be played online below.

[Try it out online! 🕹🐍](http://www.antonfagerberg.com/snakexpand/)

[Project on GitHub](https://github.com/AntonFagerberg/snakexpand)

[Actual code](https://github.com/AntonFagerberg/snakexpand/blob/main/core/src/com/antonfagerberg/snaketon/SnakeTon.java)

### GIF 🎉

![snakexpand](https://github.com/AntonFagerberg/snakexpand/raw/main/snakexpand.gif)