---
title: Destroy
categories: hooks-ufo
order: 2
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

    $ ufo destroy -y
    Hook: Running before destroy hook
    => echo 'ufo before destroy hook'
    ufo before destroy hook
    Deleting stack demo-web-dev
    Waiting for stack to complete
    04:23:19PM DELETE_IN_PROGRESS AWS::CloudFormation::Stack demo-web-dev User Initiated
    ...
    04:24:35PM DELETE_COMPLETE AWS::IAM::Role TaskRole 
    Stack demo-web-dev deleted.
    Hook: Running after destroy hook
    => echo 'ufo after destroy hook'
    ufo after destroy hook
    $ 
    