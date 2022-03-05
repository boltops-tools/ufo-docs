---
title: Task Definitions
category: intro
order: 4
---

[ECS task definitions](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definitions.html) are what you give to the ECS service to tell it how to run your containers. As such, it's prudent to understand it.

* [AWS Docs: Amazon ECS task definitions](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definition_parameters.html)

## Task Definition Basics

With UFO, you write and fully control Task Definition directly. Here's an example:

.ufo/resources/task_definitions/web.yml

```yaml
family: <%= @family %>
networkMode: bridge
containerDefinitions:
- name: <%= @name %>
  image: <%= @image %>
  cpu: <%= @cpu %>
  memory: <%= @memory %>
  memoryReservation: <%= @memory_reservation %>
  portMappings:
  - containerPort: <%= @container_port %>
    protocol: tcp
  command: <%= @command %>
  essential: true
```

You can set and use variables to control and define the Task Definition. Example:

.ufo/vars/dev.rb

```ruby
@cpu = 384
```

## Task Definition Location

The starter Task Definition is

    .ufo/resources/task_definitions/web.yml

The `web.yml` matches `UFO_ROLE=web`, which is the default role.  If you use other roles like `UFO_ROLE=worker` you do **not** have to create an additional `worker.yml` definition.  UFO looks up and considers different files. It similar to how `LOAD_PATH` works.

## Lookup Precedence

Here's the lookup with the highest precedence at the top:

    :ROLE.yml    # IE: web.yml or clock.yml
    web.yml
    default.yml

The first file considered matches the value of `UFO_ROLE`.  If you only have a web.yml

    .ufo/resources/task_definitions
    └── web.yml

Then

    UFO_ROLE=worker ufo ship # uses web.yml since worker.yml does not exist.

If you create a `worker.yml` like so:

    .ufo/resources/task_definitions
    ├── web.yml
    └── worker.yml

Then

    export UFO_ROLE=worker
    ufo ship # uses worker.yml now

This allows a common pattern where most use the same task definition code for both web and worker roles. Often the differences are minor. You can add conditional logic with ERB within the `web.yml`. If the worker task definition is very different, consider creating a `worker.yml`.