---
  layout: post
  title: "BM Elite Force 2"
  categories: projects
---

This is the second game in the BM Elite Force series (not much related to each other). BM Elite Force 2 has been my autumn/winter project of 2012. I started this "tradition" in 2011 where I create a new game in a new (for me) language every year and release it around christmas dedicated to my good old friends when we meet up. This year the game was written in Scala using the Java library Slick 2D (built on LWJGL).

[Source code on GitHub](https://github.com/AntonFagerberg/BM-Elite-Force-II)

<iframe src="//player.vimeo.com/video/56785260?portrait=0&amp;color=c9ff23" width="750" height="422" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>

[Download binaries from Google Code](http://code.google.com/p/bm-elite-force-2/downloads/list)

## Builds & Requirements
Pre-compiled versions for OS X, Windows and Linux is found in the project root (in the folders named OS X, Windows and Linux). If you just want to download and play the game, use the link above (Google Code).

## Controls
This game is meant to be played with and __Xbox 360 controller__. You really should use one to enjoy this game (analog precision makes all the difference) and it has the correct color coding. There is however keyboard bindings avaliable if you just want to try it out.

### Xbox 360 controller
 * Steering: Left stick
 * Change color: Colored buttons A, B, X, Y or Right stick
 * Shoot: Right trigger
 * Toggle health: Press left stick

### Keyboard
 * Steering: Arrow keys
 * Change color: W, A, S, D
 * Shoot: Space or Left Shift
 * Toggle health: Right Shift

## Bugs
There are some bugs which I know of but I'm not currently not trying to fix them. Feel free to submit a pull request if you wish.

### Linux
Fullscreen mode and sound doesn't work in Ubuntu 12.10 as far as I can tell?

## Requirements
Java 1.6+ is required. Everything else is distributed with the pre-compiled builds.

## Resources for hacking / building the game
[Slick 2D](http://www.slick2d.org/)

[LWJGL](http://www.lwjgl.org/)

### java.lang.UnsatisfiedLinkError: no lwjgl in java.library.path
You must specify the native files, for OS X you should provide the following VM option:

```text
-Djava.library.path=lib/native/macosx
```

## BM Elite Force 1 (game from 2011)
This was the previous game which I coded in Ruby with LibGosu.

[github.com/AntonFagerberg/BM-Elite-Force](https://github.com/AntonFagerberg/BM-Elite-Force)

## License
GNU General Public License, version 2

## Music (CC BY-NC-SA 3.0)

### Jimmypig
[XS & GS - Game Over](http://www.newgrounds.com/audio/listen/469781)

[X Sentinel - Lift Off](http://www.newgrounds.com/audio/listen/498935)

### nal1200
[Defiance](http://www.newgrounds.com/audio/listen/500422)

### Avizura
[Avizura - Loyalty](http://www.newgrounds.com/audio/listen/500531)

### MainStreamBeats
[LED (SMB) - (wip)](http://www.newgrounds.com/audio/listen/476561)

### magicalDANI13
[Magically Winterland](http://www.newgrounds.com/audio/listen/476147)

### unknown
[Tangerine](http://www.newgrounds.com/audio/listen/481979)

### Bezo
[The Most Inspiring Song Ever](http://www.newgrounds.com/audio/listen/38773)

## Graphics (CC BY-SA 3.0 / GPL 2.0 / GPL 3.0)

### Surt
[opengameart.org/content/shmup-ships](http://opengameart.org/content/shmup-ships)

### WidgetWorx
[widgetworx.com/widgetworx/portfolio/spritelib.html](http://www.widgetworx.com/widgetworx/portfolio/spritelib.html)

### Wyverii
[opengameart.org/content/unsealed-terrex](http://opengameart.org/content/unsealed-terrex)

### FrenetikFred (Space Invaders icons)
[frenetikfred.deviantart.com/art/Space-Invaders-icons-32203053](http://frenetikfred.deviantart.com/art/Space-Invaders-icons-32203053)
