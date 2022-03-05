---
title: Layering
---

UFO supports a concept called layering.  This is useful for configuring and deploying the same app for different environments. For example, dev and prod environments.

## Docs

{% assign docs = site.docs | where: "categories","layering" | sort: "order" %}
{% for doc in docs -%}
* [{{ doc.title }}]({{ doc.url }})
{% endfor %}
