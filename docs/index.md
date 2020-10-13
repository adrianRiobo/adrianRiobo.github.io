---
layout: default
---

<ul class="posts">
{% for post in site.posts %}
  <hr>
  <h1><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></h1>
  <p>{{ post.excerpt }}</p>
  <span>{{ post.date | date: "%Y/%m/%d" }}</span>
  {% endfor %}
</ul>