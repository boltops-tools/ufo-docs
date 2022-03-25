---
title: Ship
categories: hooks-ufo
order: 1
---

## Example

.ufo/config/hooks/ufo.rb

```ruby
before("ship",
  execute: "echo 'ufo before ship hook'",
)

after("ship",
  execute: "echo 'ufo after ship hook'",
)
```

Example results:

    $ ufo ship -y
    Will deploy stack demo-web-dev
    Building Docker Image
    Building Task Definition
        .ufo/output/task_definition.json
    Building params
        .ufo/output/params.json
    Building template
        .ufo/output/template.yml
    Hook: Running before ship hook
    => echo 'ufo before ship hook'
    ufo before ship hook
    Updating stack demo-web-dev
    04:14:51PM UPDATE_IN_PROGRESS AWS::CloudFormation::Stack demo-web-dev User Initiated
    ...
    04:16:54PM UPDATE_COMPLETE AWS::CloudFormation::Stack demo-web-dev
    Stack success status: UPDATE_COMPLETE
    Hook: Running after ship hook
    => echo 'ufo after ship hook'
    ufo after ship hook
    Software shipped!
    $
