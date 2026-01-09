---
layout: archive
title: "Posts"
permalink: /posts/
author_profile: true
---

{% assign postsByYear = site.posts | group_by_exp: "post", "post.date | date: '%Y'" %}

{% for year in postsByYear %}
### {{ year.name }}

<ul class="posts-list">
{% for post in year.items %}
  <li>
    <span class="post-date">{{ post.date | date: "%b %d" }}</span> — 
    <a href="{{ post.url }}">{{ post.title }}</a>
  </li>
{% endfor %}
</ul>

{% endfor %}
