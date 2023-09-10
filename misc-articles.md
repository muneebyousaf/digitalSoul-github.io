---
layout: default
title: Miscellaneous Articles 
permalink: /misc/
---

## Linux miscellaneous  articles 
<ul>
{% for article in site.misc %}
  <li>
    <a href="{{ article.url }}">{{ article.title }}</a>
  </li>
{% endfor %}
</ul>


