---
title: Logging
nav_text: Logging
category: config
order: 1
---

You can customize the logger with:

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.logger.level = "debug"
end
```

## Custom Logger

The default logger logs to `$stderr`. To use a custom logger that logs to `$stdout`.

```ruby
Ufo.configure do |config|
  logger = Logger.new($stdout)
  logger.level = "debug"
  config.logger = logger
end
```

For a list of available config settings, refer to the [Config Reference]({% link _docs/config/reference.md %}) docs.

## Environment Specific Overrides

You can configure environment specific value overrides for `.ufo/config` with corresponding `.ufo/config/env/UFO_ENV.rb` files. Examples:

.ufo/config/env/dev.rb

```ruby
Ufo.configure do |config|
  config.logger.level = "debug"
end
```

.ufo/config/env/prod.rb

```ruby
Ufo.configure do |config|
  config.logger.level = "info"
end
```

{% include config/reference/header.md %}
{% include config/reference/logging.md %}
