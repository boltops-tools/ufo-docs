---
title: Names
category: config
order: 8
---

UFO uses conventions over configuration. It has many reasonable defaults to get you going right away, but you can override its conventions. You can even override the stack naming convention and task definition naming.

## Separate AWS Accounts

The default ufo stack naming convention is `:APP-:ROLE-:ENV`. Let's say you use completely separate AWS accounts for your dev and prod environments. And you would like to remove the environment suffix.

```ruby
Ufo.configure do |config|
  config.names.stack = ":APP-:ROLE"
  config.names.task_definition = ":APP-:ROLE"
end
```

Now, when you deploy:

    $ ufo ship
    Will deploy stack demo-web
    Are you sure? (y/N) y
    Task Definition built: .ufo/output/task_definition.json
    Parameters built:      .ufo/output/params.json
    Template built:        .ufo/output/template.yml
    Creating stack demo-web
    Waiting for stack to complete
    01:54:50AM CREATE_IN_PROGRESS AWS::CloudFormation::Stack demo-web User Initiated
    ...

Notice that the stack name is now `demo-web` vs `demo-web-dev`.

Think the UFO defaults are good as they will allow you to add additional environments like `sandbox` and `qa` in the same AWS dev account. Ultimately, it's up to you how you want to handle it, though.

## Names as Callable Option

This is an advanced technique. For really custom control, you can also assign `config.names.stack` or `config.names.task_defintion` a Ruby object that implements the `.call` method.  Example:

.ufo/config.rb

```ruby
class CustomName
  def call(names)
    if names.env == "prod"
      ":APP-ROLE"
    else
      ":APP-ROLE-:ENV"
    end
  end
end

Ufo.configure do |config|
  config.names.stack = CustomName # implements call method
  config.names.task_definition = CustomName # implements call method
end
```

When `UFO_ENV=prod`, a string pattern without `:ENV` is used. And for other environments, `:ENV` in included as a suffix.

Note, the `names` argument is an instance of the [Ufo::Names](https://github.com/boltops-tools/ufo/blob/master/lib/ufo/names.rb) class.

{% include config/reference/header.md %}
{% include config/reference/names.md %}
{% include config/reference/footer.md %}
