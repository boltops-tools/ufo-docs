---
title: Build
categories: hooks-ufo
order: 3
---

## Example

.ufo/config/hooks/ufo.rb

```ruby
before("build",
  execute: "echo 'ufo before build hook'",
)

after("build",
  execute: "echo 'ufo after build hook'",
)
```

Example results:

    $ ufo ship -y
    Will deploy stack demo-web-dev
    Building Docker Image
    Building Task Definition
        .ufo/output/task_definition.json
    Hook: Running before build hook
    => echo 'ufo before build hook'
    ufo before build hook
    Building params
        .ufo/output/params.json
    Building template
        .ufo/output/template.yml
    Hook: Running after build hook
    => echo 'ufo after build hook'
    ufo after build hook
    Updating stack demo-web-dev
    04:28:40PM UPDATE_IN_PROGRESS AWS::CloudFormation::Stack demo-web-dev User Initiated
    ...
    04:30:41PM UPDATE_COMPLETE AWS::CloudFormation::Stack demo-web-dev
    Stack success status: UPDATE_COMPLETE
    Software shipped!
    $
    