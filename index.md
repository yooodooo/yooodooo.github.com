---
layout: page
title: Somewhere I Belong
---
{% include JB/setup %}

<table class="table table-striped">
  {% for post in site.posts %}
	<tr>
		<td><a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></td>
		<td><span class="badge badge-success">{{ post.group }}</span></a></td>
		<td><span>{{ post.date | date_to_string }}</span></a></td>
	</tr>
  {% endfor %}
</table>