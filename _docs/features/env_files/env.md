---
title: Env Plain Text
nav_text: Env
category: features-env-files
order: 1
---

Env files contain plain text, non-sensitive data. They are safe to commit to version control.

## Set Values

You can set the values in

.ufo/env_files/dev.env

    RAILS_ENV=development

.ufo/env_files/prod.env

    RAILS_ENV=production

## Call Helper

To use the values, call the `env_files` helper.

.ufo/vars/base.rb

```ruby
@environment = env_files
```

In the task definition

.ufo/resources/task_definitions/web.yml

```yaml
environment: @environment
```

## Build

Build the task definition

    ufo build

This results in

```json
{
  environment: [{
    name: RAILS_ENV,
    value: development
  }]
}
```

## Confirm

After deploying, you can use exec into the container to confirm that the `RAILS_ENV` is available as an env var.

    ufo ship
    ufo exec

In the container

    # echo $RAILS_ENV
    development

{% include features/env_files/layering.md %}
