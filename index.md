---
layout: page
title: Somewhere I Belong
---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.posts %}
    <li>[{{ post.group }}]<span>{{ post.date | date_to_string }}&raquo; </span><a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a> </li>
  {% endfor %}
</ul>
