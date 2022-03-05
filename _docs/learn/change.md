---
title: Change Settings
---

Let's make some simple changes.

## Change Task Definition

Let's change and increase the Docker CPU limit for the dev environment.

.ufo/vars/dev.rb

```ruby
@cpu = 512
@memory = 512
```

These variables are used by the task definition.

.ufo/resources/task_definitions/web.yml

```fuby
family: <%= @family %>
networkMode: bridge
containerDefinitions:
- name: <%= @name %>
  image: <%= @image %>
  cpu: <%= @cpu %>        # <= USED HERE
  memory: <%= @memory %>  # <= USED HERE
  memoryReservation: <%= @memory_reservation %>
# ...
```

This change affects the ECS Task Definition used by the ECS Service. It will allow each Docker container higher CPU and Memory Limits.

## Change AutoScaling Settings

Let's also increase the max capacity ECS Service AutoScaling setting from 2 to 3. You do this in

.ufo/config/web/dev.rb

```ruby
Ufo.configure do |config|
  config.autoscaling.max_capacity = 3 # <= CHANGED
end
```

This affects the ECS Service AutoScaling feature. It allows the app to scale out and add more container capacity automatically.

Next, we'll deploy the updates.
