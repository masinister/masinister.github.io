---
layout: archive
title: "Slides"
permalink: /slides/
author_profile: true
---

<div class="grid__wrapper">
  {% for post in site.slides reversed %}
    {% include archive-single-cv.html type="grid" %}
  {% endfor %}
</div>

<!-- ### Footer

Last updated: Augst 2023 -->
