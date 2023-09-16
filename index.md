---
layout: default
title: "Drew Tate's Blog"
---

<h1>Drew Tate</h1>

<h2>Blog posts</h2>

<ul class="postList">
  {% for post in site.posts %}
  <li>
    {{ post.date | date: "%D" }} &#10148; <a href="{{ post.url }}">{{ post.title }}</a>
  </li>
  {% endfor %}
</ul>
