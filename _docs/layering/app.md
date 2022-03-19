---
title: App Layering
category: layering
order: 4
---

If you are using UFO with a [central deployer pattern]({% link _docs/patterns/central-deployer.md %}), you can activate and use App Layers by setting the `UFO_APP` env var.

## Stick to a Few

Since layering is so powerful, you want to choose a few layers that make sense for your team and stick to them. Here's an example:

    .ufo/config/app1/base.rb
    .ufo/config/app1/dev.rb
    .ufo/config/app1/web/base.rb
    .ufo/config/app1/web/dev.rb

Shown as a tree:

    .ufo/config/app1/
    ├── base.rb
    ├── dev.rb
    └── web
        ├── base.rb
        └── dev.rb

## Example Usage

Configure common app1 settings:

.ufo/config/app1/base.rb

```ruby
Ufo.configure do |config|
  config.logger.level = "info"
end
```

Enable debug logging for dev:

.ufo/config/app1/dev.rb

```ruby
Ufo.configure do |config|
  config.logger.level = "debug"
end
```

Common settings for the web role:

.ufo/config/app1/web/base.rb

```ruby
Ufo.configure do |config|
  config.autoscaling.enabled = true # default
  config.autoscaling.max_capacity = 1
  config.autoscaling.min_capacity = 1
  config.autoscaling.predefined_metric_type = "ECSServiceAverageCPUUtilization"
  config.autoscaling.target_value = 75.0
end
```

Overrides for the web role in dev:


```ruby
Ufo.configure do |config|
  config.autoscaling.max_capacity = 2
end
```

## Full App Layering

Here are the full layers with `UFO_APP=app1`

Config layers:

    .ufo/config.rb
    .ufo/config/base.rb
    .ufo/config/dev.rb
    .ufo/config/env.rb
    .ufo/config/env/base.rb
    .ufo/config/env/dev.rb
    .ufo/config/web.rb
    .ufo/config/web/base.rb
    .ufo/config/web/dev.rb
    .ufo/config/app1.rb
    .ufo/config/app1/base.rb
    .ufo/config/app1/dev.rb
    .ufo/config/app1/env.rb
    .ufo/config/app1/env/base.rb
    .ufo/config/app1/env/dev.rb
    .ufo/config/app1/web.rb
    .ufo/config/app1/web/base.rb
    .ufo/config/app1/web/dev.rb

Vars Layers:

    .ufo/vars.rb
    .ufo/vars/base.rb
    .ufo/vars/dev.rb
    .ufo/vars/web.rb
    .ufo/vars/web/base.rb
    .ufo/vars/web/dev.rb
    .ufo/vars/app1.rb
    .ufo/vars/app1/base.rb
    .ufo/vars/app1/dev.rb
    .ufo/vars/app1/web.rb
    .ufo/vars/app1/web/base.rb
    .ufo/vars/app1/web/dev.rb

As mentioned, layering can get complex and be abused. Would just stick to a few that work for your team.

{% include layering/config-layering-show.md %}
