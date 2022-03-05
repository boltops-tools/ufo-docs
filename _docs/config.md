---
title: Config
---

You can customize UFO behavior with:

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.logger.level = "debug"
  config.app = "demo"
  config.docker.repo = "tongueroo/demo"
end
```

We'll cover more configurable settings next.

{% assign docs = site.docs | where: "categories","config" | sort:"order" %}
{% for doc in docs -%}
* [{{ doc.title }}]({{ doc.url }})
{% endfor %}
