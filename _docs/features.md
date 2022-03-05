---
title: Features
---

UFO supports many options and features. We'll list some of them here:

{% assign docs = site.docs | where: "categories","features" | sort:"order" %}
{% for doc in docs -%}
* [{{ doc.title }}]({{ doc.url }})
{% endfor %}
