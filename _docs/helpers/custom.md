---
title: Custom Helpers
nav_text: Custom
order: 2
category: helpers
---

You can define custom helpers and extend the UFO syntax like a first-class citizen.

## New Helper Generator

You can generate a starter helper file like so:

    $ ufo new helper
    => Creating custom_helper.rb
          create  .ufo/helpers/custom_helper.rb

Update the code to something like this:

```ruby
module CustomHelper
  def high_memory
    1024
  end
end
```

Now you can use it in your `task_definition.yml` like so:

```yaml
containerDefinitions:
- name: <%= @name %>
  memory: <%= high_memory %>
# ...
```

To quickly build:

    ufo build --no-docker

Confirm that worked

    $ cat .ufo/output/task_definition.json | jq '.containerDefinitions[].memory'
    1024
