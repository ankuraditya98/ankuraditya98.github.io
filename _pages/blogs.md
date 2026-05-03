---
layout: page
title: blogs
permalink: /blogs/
description: Thoughts on research, systems, and technology.
nav: true
nav_order: 4
---

<div class="projects">
  <div class="row row-cols-1 row-cols-md-3">
    {% assign sorted_blogs = site.blogs | sort: "date" | reverse %}
    {% for blog in sorted_blogs %}
      {% include blogs.liquid %}
    {% endfor %}
  </div>
</div>
