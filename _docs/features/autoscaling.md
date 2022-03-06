---
title: ECS Service AutoScaling
nav_text: AutoScaling
category: features
order: 1
---

With Ufo, you can configure ECS Service AutoScaling settings. Here are the default settings.

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.autoscaling.enabled = true # default
  config.autoscaling.max_capacity = 5
  config.autoscaling.min_capacity = 1
  config.autoscaling.predefined_metric_type = "ECSServiceAverageCPUUtilization"
  config.autoscaling.target_value = 75.0
end
```

Your ECS Service will automatically scale out and in based on load.

## ufo scale

You can use the [ufo scale]({% link _reference/ufo-scale.md %}) command to manually change the scaling settings. Note, however, when [ufo ship]({% link _reference/ufo-ship.md %}) is next ran, it'll use the `.ufo/config.rb` settings.

{% include config/reference/header.md %}
{% include config/reference/autoscaling.md %}
{% include config/reference/footer.md %}
