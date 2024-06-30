---
title: "Error Notes"
layout: archive
permalink: /errors
---


{% assign posts = site.categories.projects %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}