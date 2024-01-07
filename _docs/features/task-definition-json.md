---
title: Task Definition Formats
category: features
order: 10
---

UFO supports writing your Task Definition in either YAML or JSON. For those who prefer writing the Task Definition in JSON, here's an example:

.ufo/resources/task_definitions/web.json

```json
<%#
# networkMode comment:
# bridge is the default because awsvpc requires specific instance types and private subnets.
# But bridge mode requires user to open up ports on EC2 instances. Ports: 32768-65535.
# awsvpc requires specific instance types and ECS tasks to run on private subnets.
%>
{
  "family": "<%= @family %>",
  "networkMode": "bridge", <%# bridge is default because awsvpc requires specific instance types and private subnets %>
  "containerDefinitions": [
    {
      "name": "<%= @name %>",
      "image": "<%= @image %>",
      "cpu": "<%= @cpu %>",
      <% if @memory %>
      "memory": <%= @memory %>,
      <% end %>
      <% if @memory_reservation %>
      "memoryReservation": <%= @memory_reservation %>,
      <% end %>
      <% if @container_port %>
      "portMappings": [
        {
          "containerPort": <%= @container_port %>,
          "protocol": "tcp"
        }
      ],
      <% end %>
      "command": <%= @command.to_json %>,
      <% if @environment %>
      "environment": <%= @environment.to_json %>,
      <% end %>
      <% if @secrets %>
      "secrets": <%= @secrets.to_json %>,
      <% end %>
      <% if @awslogs_group %>
      "logConfiguration": {
        "logDriver": "awslogs",
        "options": {
          "awslogs-group": "<%= @awslogs_group %>",
          "awslogs-region": "<%= @awslogs_region || 'us-east-1' %>",
          "awslogs-stream-prefix": "<%= @awslogs_stream_prefix %>"
        }
      },
      <% end %>
      "essential": true
    }
  ]
}
```