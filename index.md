---
layout: page
title: Somewhere I Belong
---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.posts %}
    <li><span>[{{ post.group }}]--{{ post.date.strftime('%Y-%m-%d') }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
