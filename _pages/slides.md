---
layout: archive
title: "Slides"
permalink: /slides/
author_profile: true
redirect_from:
  - /talks
---

I made lots of slides over the years, feel free to use.

{% include base_path %}

{% for post in site.slides reversed %}
  {% include archive-single-talk.html %}
{% endfor %}

<!-- ### Footer

Last updated: Augst 2023 -->
