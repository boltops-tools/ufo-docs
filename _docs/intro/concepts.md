---
title: UFO Concepts
nav_text: Concepts
category: intro
order: 3
---

Here are main UFO concepts:

* [Task Definitions]({% link _docs/intro/task-definitions.md %})
* [App, Role, Env]({% link _docs/intro/app-role-env.md %})
* [Layering]({% link _docs/layering.md %})

## Task Definition

You define ECS Task Definition in `.ufo/resources/task_definitions/web.yml`. You can use variables and ERB templating to build different definitions for dev and prod environments.

## App, Role and Env

Most tools have the notion of an app and environment. UFO, like heroku, includes the role concept. Examples of roles are: web, worker, clock.  The default role is web and will conventionally create an ELB. These are just examples, as you can name the roles other names also.

## Layering

UFO supports a concept called layering so you can use the same code to build multiple environments like dev and prod. More details in the [Layering Docs]({% link _docs/layering.md %}).
