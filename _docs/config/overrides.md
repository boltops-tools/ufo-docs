---
title: Config Overrides
nav_text: Overrides
categories: config
order: 5
---

You can override `.ufo/config.rb` settings on an env and role basis.

## Env Overrides

You can also override settings on an env basis.

.ufo/config/env/dev.rb

```ruby
Ufo.configure do |config|
  config.vpc.security_groups.ecs = ["sg-111"]
```

## Role Overrides

You can also override on an env-role scoped basis.

.ufo/config/env/dev/web.rb

```ruby
Ufo.configure do |config|
  config.vpc.security_groups.ecs = ["sg-111"]
  config.vpc.security_groups.elb = ["sg-222"]
end
```
