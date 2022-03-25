---
title: Hooks
nav_text: Hooks
category: config
order: 6
---

UFO supports a variety of hooks. They can be used to customize and finely control the ufo deploy process.

{% assign docs = site.docs | where: "categories","hooks" %}
{% for doc in docs -%}
* [{{ doc.title }}]({{ doc.url }})
{% endfor %}
