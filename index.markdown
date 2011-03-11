---
layout: default
title: The Rubyists
---

{% for post in site.posts limit:10 %}
  <article class="blogpost">
    <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
    {{ post.content }}
  </article>
{% endfor %}
