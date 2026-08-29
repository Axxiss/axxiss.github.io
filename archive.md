---
layout: page
title: Archive
permalink: /archive/
description: 'Older posts by Alexis Mas, from a previous career writing Android and mobile applications.'
---

Posts from an earlier stretch of my career, when I was mostly writing Android and mobile
applications. They're out of date and out of step with what I work on now, but the links
still work, so here they are.

<ul>
{% for post in site.archive reversed %}
  <li>
    <a href="{{ post.url }}">{{ post.title }}</a>
    <span class="date">— {{ post.date | date: "%-d %b %Y" }}</span>
  </li>
{% endfor %}
</ul>

Current writing lives on the [blog](/blog).
