---
layout: post
title: "AI & bespoke software"
tags: writing
---

As many developers I've been on an AI journey for a few years that really took off around the end of last year (2025) when Opus 4.5 arrived and things actually got good.

When I first heard of "bespoke software", writing custom software for yourself instead of using existing solutions, I thought it was interesting but it didn't make much sense in practice. Recently I've changed my mind as I've used AI to create two small pieces of bespoke software: [JsonDiff](/projects/json-diff) (for diffing json) and [Tube](/projects/tube) (a YouTube client).

They are perhaps not much to brag about feature wise compared to other existing apps but what makes them interesting is that they do exactly what *I want* them to do in the way *I want* it to be done. I think [Tube](/projects/tube) (the YouTube client) is a particularly interesting example. Compared to the real client (or third-party clients) it doesn't offer much, but I actually want it to *do less*.

When I have the YouTube app installed om my phone I end up using it way too much. I get sucked into the algorithm with more and more fringe and enraging content and watch way too many shorts. YouTube won't change. They want maximum engagement, they will never make a slimmed down version or even decent settings. But now I can do it myself, in an hour, and enjoy it they way I want to enjoy it.

I think in the past I would've tried motivate the effort spent by offering it to others, flex by showing off the source, or sell it for some ROI (never happened). But now it's so effortless that it's no longer a consideration. And since it's just for me, I'm intentionally not releasing the source code or any binaries; no worrying about prying eyes judging the code or dealing with support, questions or feature requests. I'm not looking at the source anyway. Immoral? Maybe.

Also, if I needed to care about other users I would've had to do a lot of polishing, like warn if `mpv` is not installed or figure out where it is installed or if I should bundle it and so on. But now I don't. It only has to work for me, on my machine.