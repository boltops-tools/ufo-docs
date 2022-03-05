---
title: Upgrading
---

If you're upgrading UFO, here are some upgrading docs:

<ul>
{% assign docs = site.docs | where: "categories","upgrading" | sort: "order" %}
{% for doc in docs -%}
  <li><a href='{{doc.url}}'>{{doc.title}}</a></li>
{% endfor %}
</ul>
