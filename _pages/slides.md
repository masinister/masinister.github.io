---
layout: archive
title: "Slides"
permalink: /slides/
author_profile: true
redirect_from:
  - /talks
---

I made lots of slides over the years, feel free to use.

{% for post in site.slides reversed %}
  {% include archive-single-cv.html %}
{% endfor %}

<!-- ### Footer

Last updated: Augst 2023 -->
