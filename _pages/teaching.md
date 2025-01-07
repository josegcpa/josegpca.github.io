---
layout: page
title: teaching
permalink: /teaching/
description:
nav: true
nav_order: 5 
horizontal: true
---

<!-- pages/teaching.md -->
<div class="projects">
  {%- assign sorted_projects = site.teaching | sort: "date" |reverse -%}
  <!-- Generate cards for each project -->
  <div class="custom-grid">
    {%- for project in sorted_projects -%}
      {% include projects.html %}
    {%- endfor %}
  </div>
</div>