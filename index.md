---
layout: default
---

## Welcome to Paul Chilton's Knowledge Base

This knowledge base is a repository of information that I use to store useful information from projects that I have worked on. The information contained within this site may not be completely accurate or even the best way to achieve the goals but it is the way I have done it on one or more projects.

I started this site to both organise my notes on these topics and to provide the information for anyone else that is interested or working on similar things. Please feel free to get in touch if you know of better ways to do things, have things to contribute or want to discuss anything in more detail.

<div id="home">
  <h1>Blog Posts</h1>
  <ul class="posts">
    {% for post in site.posts %}
      <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
</div>