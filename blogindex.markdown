---
layout: blog
title: Blog
feed: atom.xml
keywords: Linux, LaTeX, Chemistry, Chemical Engineering, Python, important
important: yes
---

<style>
div.post
{
width: 55em;
}
</style>

Blog [![Feed icon](/files/css/feed-icon-14x14.png)][feed]
=====================

{% for post in site.posts limit: 10 %}
  <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
  <h3 class="datetext" style="float:left">
    Posted on {{ post.date | date_to_string }}
 
  </h3>
 <span style="float:right"><a class="comments" data-disqus-identifier="{{ post.disqus_id }}" href="{{ post.url }}#disqus_thread">View Comments</a></span>  

<div class="c">&nbsp;</div>
  <p>{{ post.content | strip_html | truncatewords: 75 }}</p>
  <p><a href="{{ post.url }}">Read more...</a></p>
{% endfor %}


[feed]: {% include feedname.txt %}



<p>
<a href="past.html">Older Posts &rarr;</a>
</p>

{% include commentcount.js %}
