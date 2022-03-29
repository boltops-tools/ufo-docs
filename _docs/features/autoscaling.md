---
title: ECS Service AutoScaling
nav_text: AutoScaling
category: features
order: 1
---

With UFO, you can configure ECS Service AutoScaling settings. Here is an example.

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.autoscaling.enabled = true # default
  config.autoscaling.max_capacity = 5
  config.autoscaling.min_capacity = 1
  config.autoscaling.predefined_metric_type = "ECSServiceAverageMemoryUtilization"
  config.autoscaling.target_value = 75.0
end
```

Your ECS Service will automatically scale in and out based on load.

## Manual AutoScaling Changes and Considerations

You can use the [ufo scale]({% link _reference/ufo-scale.md %}) or the AWS console to manually change the scaling settings. However, when [ufo ship]({% link _reference/ufo-ship.md %}) is next run, the `.ufo/config.rb` settings will override the manual changes. You can enable UFO to retain manual changes with:

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.autoscaling.manual_changes.retain = true
end
```

Doing so means that `autoscaling.max_capacity` and `autoscaling.max_capacity` are only used for initial deployment. Afterward, they are not respected.

{% include config/reference/header.md %}
{% include config/reference/autoscaling.md %}
{% include config/reference/footer.md %}
