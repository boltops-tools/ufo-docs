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

## Cluster Setting as Callable Option

This is an advanced technique. For really custom control, you can also assign `config.ecs.cluster` a Ruby object that implements the `.call` method.  Example:

.ufo/config.rb

```ruby
class ClusterName
  def call(names)
    case names.env
    when "prod"
      "prod-cluster"
    when "dev", "sbx", "uat"
      "dev-cluster"
    else
      "qa-cluster"
    end
  end
end

Ufo.configure do |config|
  config.ecs.cluster = ClusterName # implements call method
end
```

When `UFO_ENV=prod`, the ECS Service uses the prod-cluster. When the `UFO_ENV` is dev, sbx, or uat, the the dev-cluster. And for other `UFO_ENV` values, the qa-cluster is used.

Note, the `names` argument is an instance of the [Ufo::Names](https://github.com/boltops-tools/ufo/blob/master/lib/ufo/names.rb) class.

{% include config/reference/header.md %}
{% include config/reference/ecs.md %}
{% include config/reference/footer.md %}
