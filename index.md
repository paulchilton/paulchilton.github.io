---
layout: default
title: Paul Chilton Technical Knowledge Base
---

<style>
	img.welcome {
		float: left;
		margin-right: 10px;
		width: 100px;
	}
</style>

## Welcome to Paul Chilton's Knowledge Base

![](/images/logo-blue-200.png){: .welcome }This knowledge base is a repository of information that I use to store useful information from projects that I have worked on. The information contained within this site may not be completely accurate or even the best way to achieve the goals but it is the way I have done it on one or more projects.

I started this site to both organise my notes on these topics and to provide the information for anyone else that is interested or working on similar things. Please feel free to get in touch if you know of better ways to do things, have things to contribute or want to discuss anything in more detail.

<div id="home">
  <h2>Articles</h2>
  <ul class="posts">
    {% for post in site.posts %}
      <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ site.baseurl }}{{ post.url }}#disqus_thread" data-disqus-identifier="{{post.url}}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
</div>

<script type="text/javascript">
  /* * * CONFIGURATION VARIABLES: EDIT BEFORE PASTING INTO YOUR WEBPAGE * * */
  var disqus_shortname = 'pchilton';
  var disqus_developer = 1; // Comment out when the site is live

  /* * * DON'T EDIT BELOW THIS LINE * * */
  (function () {
	var s = document.createElement('script'); s.async = true;
	s.type = 'text/javascript';
	s.src = 'http://' + disqus_shortname + '.disqus.com/count.js';
	(document.getElementsByTagName('HEAD')[0] || document.getElementsByTagName('BODY')[0]).appendChild(s);
  }());
</script>
