---
title: "Projects"
layout: archive
permalink: /projects
sidebar:
  nav: "sidebar-category"
---


{% assign posts = site.categories.projects %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}