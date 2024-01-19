---
layout: post
---

# Hello & welcome!

You've reached the personal website for me - Anton Fagerberg! Here I collect some of my projects, the books I read and other things I find interesting.
It's my contribution to the "the small web".
You can [learn more about me](/about), or [contact me by e-mail](mailto:anton@antonfagerberg.com).
Otherwise, check out everything below!

## Collections
 - [Reading list (books)](/books)
 - [Links (to things I like)](/links)

## Code
{% for post in site.tags.code %}
- [{{ post.title }} ({{ post.date | date_to_string }})]({{ post.url }})
{% endfor %}

## Games
{% for post in site.tags.games %}
- [{{ post.title }} ({{ post.date | date_to_string }})]({{ post.url }})
{% endfor %}

## Miscellaneous
{% for post in site.tags.misc %}
- [{{ post.title }} ({{ post.date | date_to_string }})]({{ post.url }})
{% endfor %}

## Papers
- [Optimising clients with API gateways (08 Jun 2015)](https://lup.lub.lu.se/student-papers/search/publication/5469608)
- [Temporal Information Extraction (13 Jan 2014)](/files/tempex_anton_fagerberg.pdf)