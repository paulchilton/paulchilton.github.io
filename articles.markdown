---
layout: default
title: Paul Chilton Technical Knowledge Base - Articles
---

<h1>Articles</h1>
<ul class="posts">
	{% for post in site.posts %}
		<li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
	{% endfor %}
</ul>
