---
title: "Gallery"
layout: archive
permalink: /gallery
sidebar:
  nav: "sidebar-category"
---


{% assign posts = site.categories.gallery %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}