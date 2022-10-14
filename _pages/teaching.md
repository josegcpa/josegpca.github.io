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
  {%- assign sorted_projects = site.teaching | sort: "date" -%}
  <!-- Generate cards for each project -->
  {% if page.horizontal -%}
  <div class="container">
    <div class="row row-cols-2">
    {%- for project in sorted_projects -%}
      {% include projects_horizontal.html %}
    {%- endfor %}
    </div>
  </div>
  {%- else -%}
  <div class="grid">
    {%- for project in sorted_projects -%}
      {% include projects.html %}
    {%- endfor %}
  </div>
  {%- endif -%}
</div>