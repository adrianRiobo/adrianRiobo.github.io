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


{% for post in site.posts %}
    <li>
      <h1><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></h1>
      <span>{{ post.date | date: "%Y/%m/%d" }}</span> 
    </li>
  {% endfor %}