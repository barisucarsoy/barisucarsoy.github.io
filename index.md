---
layout: default
title: Home
---

# Welcome to My Simple Blog

This is the homepage. You can list your recent posts here or write a short introduction.

## Recent Posts
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a> - {{ post.date | date: "%B %d, %Y" }}
    </li>
  {% endfor %}
</ul>
