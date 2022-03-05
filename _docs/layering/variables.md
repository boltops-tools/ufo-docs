---
title: Variables
category: layering
order: 2
---

Often, you end up using the set of common variables across your task definitions for a project.  You define them in `base.rb` like so:

.ufo/vars/base.rb

```
@image = docker_image # includes the git sha org/repo:ufo-[sha].
@cpu = 128
@memory_reservation = 256
```

You can now use `@image` in your `.ufo/resources/task_definitions/web.yml`

## Layering

Variables also support a concept called layering.  The `base.rb` file is treated specially and will always be evaluated.  Additionally, ufo will also evaluate the `[UFO_ENV].rb` according to what UFO_ENV's value is. Thanks to layering, you can override variables to suit different environments like `dev` and `prod`. For example:

.ufo/vars/dev.rb

```ruby
@cpu = 256
```

When `ufo ship` is run with `UFO_ENV=dev` the `vars/dev.rb` will be evaluated and layered on top of the variables defined in `base.rb`

.ufo/vars/prod.rb

```ruby
@cpu = 512
```

When `ufo ship` is run with `UFO_ENV=dev` the `vars/dev.rb` will be evaluated and layered on top of the variables defined in `base.rb`:
