---
layout: default
title: Home
---

# Welcome to My Blog

This is the homepage. Below, you can the stuff I wrote.

## Recent Posts
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a> - {{ post.date | date: "%B %d, %Y" }}
      {% if post.categories.size > 0 %}
        <span class="post-tags">
          {% for category in post.categories %}
            <span class="tag tag-{{ category | slugify }}">{{ category }}</span>
          {% endfor %}
        </span>
      {% endif %}
    </li>
  {% endfor %}
</ul>
