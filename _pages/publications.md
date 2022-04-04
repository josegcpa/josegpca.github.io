---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---

{% if author.googlescholar %}
  You can also find my articles on <u><a href="{{author.googlescholar}}">my Google Scholar profile</a>.</u>
{% endif %}

{% include base_path %}

## Publications
{% for post in site.publications_current reversed %}
  {% include archive-single.html %}
{% endfor %}

{% for post in site.publications_previous reversed %}
  {% include archive-single.html %}
{% endfor %}
