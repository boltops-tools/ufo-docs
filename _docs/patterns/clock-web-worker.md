---
title: Clock Web Worker Pattern
nav_text: Clock Web Worker
categories: patterns
order: 2
---

A common pattern is to use the same code to run different types of processes like clock, web, worker. UFO supports this pattern.

Note, often the clock process is also called a scheduler.

## Structure

Here's a structure that achieves this pattern with Kubes:

    .ufo/resources/task_definitions
    └── web.yml

Notice, `web.yml` is needed. This is because ufo considers multiple lookup locations. Docs: [Task Definition Location]({% link _docs/intro/task-definitions.md %}#task-definition-location)

## Override

For many cases, using conditional logic in the `web.yml` is enough to account for differences between different roles. For example:

.ufo/resources/task_definitions/web.yml

```yaml
family: <%= @family %>
<% if @role == "worker" %>
networkMode: bridge
<% else %>
networkMode: awsvpc
<% end %>
```

If the worker and web roles start to really diverge, though, you may want to consider creating a separate `worker.yml` entirely. The structure would look like this:

    .ufo/resources/task_definitions
    │── web.yml
    └── worker.yml

## Deploy

To deploy the roles.

    UFO_ROLE=clock  ufo ship
    UFO_ROLE=web    ufo ship
    UFO_ROLE=worker ufo ship

The default is `UFO_ROLE=web`.

You can also use, export to simplify your commands. The nice this that it'll work for all commands.

    export UFO_ROLE=worker
    ufo ship
    ufo ps
    ufo logs