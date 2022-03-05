---
title: Variable Expansion
nav_text: Expansion
category: config
order: 7
---

Notice how some config options are patterns. Like:

```ruby
Ufo.configure do |config|
  config.names.stack = ":APP-:ROLE-:ENV"
  config.names.task_definition = ":APP-:ROLE-:ENV"
end
```

UFO takes the patterns and expands them to specific values. Example:

    :APP-:ROLE-:ENV => demo-web-dev

Here are the available variables

Name | Description
---|---
APP | Ufo app name. Configured by `config.app` or `UFO_APP`
ROLE | Ufo role. Configured by `UFO_APP`. The default is `web`
ENV | Ufo env. Configured by `UFO_ENV`. The default is `dev`
REGION | The current `AWS_REGION`, set by env var, `~/.aws/config`, etc. The default is `us-east-1` when it cannot be discovered.
