---
title: ELB WAF Support
nav_text: ELB WAF
category: features
order: 6
---

You can associate an exisitng WAF ACL with the ELB. Here's an example:

.ufo/config.rb

```ruby
Ufo.configure do |config|
  config.waf.web_acl_arn = waf("demo-waf") # find web acl by name. returns ARN
end
```

This allows you to reuse one WAF ACL and associate its Rules with multiple ELBs. It can help keep your WAF Rules DRY. IE: Change the rules one time and they protect all the ELB traffic.
