---
title: Patterns
---

In building and UFO certain patterns have been found. We'll list some of them here:

{% assign docs = site.docs | where: "categories","patterns" | sort:"order" %}
{% for doc in docs -%}
* [{{ doc.title }}]({{ doc.url }})
{% endfor %}
