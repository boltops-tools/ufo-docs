---
title: Automated Clean Up
category: features
order: 8
---

UFO can be configured to automatically clean old images from the ECR registry after the deployment completes by configuring your [Config]({% link _docs/config.md %}) file like so:

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.docker.clean_keep	= 5 # keep 5. remove older ufo produced docker images locally
  config.docker.ecr_keep	= 5   # keep 5. remove older ufo produced docker on ECR
end
```

{% include config/reference/header.md %}
{% include config/reference/docker.md %}
{% include config/reference/footer.md %}
