---
layout: post
title: ""
---

## Collections
<ul>
    <li><p><a href="/books">Reading list (books)</a></p></li>
</ul>

## Writing
{% for post in site.tags.writing %}
- [{{ post.title }} ({{ post.date | date_to_string }})]({{ post.url }})
{% endfor %}

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