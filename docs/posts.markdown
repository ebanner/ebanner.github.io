---
layout: default
title: Posts
permalink: /posts/
---

<h1>Posts</h1>
{% for post in site.posts %}
  <h3 style="margin-bottom: 0;">
    <a href="{{ post.url }}">{{ post.title }}</a>
  </h3>
  <p style="margin-top: 5px; color: #666; font-size: 16px;">
    {{ post.description }}
  </p>
{% endfor %}

