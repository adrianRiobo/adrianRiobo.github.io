---
layout: default
---

<ul class="posts">
  {% for post in site.posts %}
    <li>
      <span>{{ post.date | date: "%Y/%m/%d" }}</span> 
      <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>

<ul class="posts">
{% for post in site.posts %}
  <h1><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></h1>
  <span>{{ post.date | date: "%Y/%m/%d" }}</span>   
  {% endfor %}
</ul>