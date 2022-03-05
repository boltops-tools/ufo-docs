---
title: ECS Cluster
nav_text: Cluster
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
