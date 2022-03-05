---
title: Notification ARNs
category: features
order: 9
---

You can specific notification arns for CloudFormation stack related events with [.ufo/config.rb]({% link _docs/config.md %}). This may be useful for compliance purposes.

## Example

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.cfn.notification_arns = [":arn:aws:sns:us-west-2:112233445566:my-sns-topic1"]
end
```
