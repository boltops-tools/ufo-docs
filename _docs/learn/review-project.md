---
title: Review Project
---

Let's review the resources.

## ECS Task Definition

UFO creates a task definition resource using the file in `.ufo/resources/task_definitions`. It looks something like this:

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

The generated task definition is in YAML format. You have full control over the task definition and can write it in YAML or JSON format.

Also, notice the `<%= ... %>` notation. UFO adds Ruby sprinkles to your task definition files by allowing you to use ERB templating support.  This will enable you to use the same task definition source code and create different versions for dev and prod environments.

## Setting Variables

To set the template variables, you use the files in `.ufo/vars`. Examples:

.ufo/vars/base.rb

```ruby
@family = task_definition_name
@name = role
@image = docker_image # includes the git sha org/repo:ufo-[sha].
@cpu = 256
@memory = 512
@memory_reservation = 512
@awslogs_group = ["ecs/#{Ufo.app}", Ufo.env, Ufo.extra].compact.join('-')
@awslogs_stream_prefix = role
@awslogs_region = aws_region
```

.ufo/vars/dev.rb

```ruby
@cpu = 384
```

.ufo/vars/prod.rb

```ruby
@cpu = 512
```

The `base.rb` file provides common variables for the Task Definition. The `dev.rb` and `prod.rb` provide environment-specific overrides based on `UFO_ENV`.

Next, we'll deploy the app.
