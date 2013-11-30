---
layout: page
title: Somewhere I Belong
---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.posts %}
    <li>[{{ post.group }}]<a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a><span>&raquo; {{ post.date | date_to_string }}</span> </li>
  {% endfor %}
</ul>
