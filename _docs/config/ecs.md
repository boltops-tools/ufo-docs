---
title: ECS Cluster
nav_text: ECS
categories: config
order: 2
---

By default, the ECS cluster UFO uses is whatever `UFO_ENV` is set to. Since the default `UFO_ENV=dev`, this means the ECS cluster named "dev" will be used. You can override this setting.

## Cluster Setting

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.ecs.cluster = ":ENV-cluster"
end
```

Here we're overriding the cluster name with `:ENV-cluster`. The setting is a pattern that UFO expands. When `UFO_ENV=dev`

    :ENV-cluster => dev-cluster

So ufo will deploy your application to the ECS cluster name "dev-cluster".

## Deployment Configuration

The `ecs.maximum_percent`	and `ecs.minimum_healthy_percent` can be set to control the rolling deploy percentages. Example:

```ruby
Ufo.configure do |config|
  config.ecs.maximum_percent = 300
  config.ecs.minimum_healthy_percent = 100
end
```

Important: You should **not** set the `ecs.maximum_percent` to below 200 if you only have 1 container and want it to able to scale to 2. See: [Debugging Deployment Configuration]({% link _docs/debug/deployment-configuration.md %}).

{% include config/reference/header.md %}
{% include config/reference/ecs.md %}
{% include config/reference/footer.md %}